### **1. Overview of Migration Process**
The goal is to migrate the Jenkins CI pipeline script into TeamCity's Kotlin DSL format while ensuring functional parity. The migration will include dynamic builds, stages, configurations, and external integrations where feasible.
### **2. Use of JSON Files**
- **`teamcity-config.json` **: This JSON contains the parsed representation of entities already mapped from the Jenkinsfile. Utilize this file to create Project (`project`), VCS Root (`vcsRoot`), Build Configurations (`buildConfiguration`), and Templates in TeamCity.
- **`teamcity-config-manual-migration.json` **: This file identifies components of the Jenkinsfile that are hard to migrate automatically and provides reasoning and guidelines for manual efforts. Any missing features or areas requiring manual implementation should be logged in this file for completeness. **You must consult and follow the manual steps provided for such cases.**

### **3. Migration Guidelines**
#### **3.1 Project Creation**
- The top-level TeamCity **project** should be created using the details from the `project` section in `teamcity-config.json`.
- **Description** and **parameters** (like environment variables) should be added to the Project's DSL file.
- Map Jenkins `properties()` to appropriate Project/Build Template configurations.

#### **3.2 VCS Root**
- Use the `vcsRoot` property from `teamcity-config.json` to define a VCS Root in Kotlin DSL.
- Map GitHub repository URL, branch (`refs/heads/master`), and mirroring options (`useMirrors`, `useAlternates`) to TeamCity's VCS Root settings.

#### **3.3 Build Templates**
- Convert Jenkins stages to reusable TeamCity **Build Templates**. The `steps`, `features`, and `parameters` in the `teamcity-config.json` template provide the basis for this mapping.
- Use `artifactRules` from the template to define artifact handling.

#### **3.4 Build Configurations**
- If the Jenkinsfile contains dynamic combinations (e.g., of platforms and JDK versions), translate these into separate **Build Configurations** within TeamCity.
   - For configurations requiring matrix-like parameterization, consider TeamCity Parameterized Builds.
   - Reference the guidelines in `teamcity-config-manual-migration.json` for dynamic stage generation.

#### **3.5 Build Steps**
- Map Jenkins `stage()` into sequential TeamCity `steps`:
   - Use the provided `steps` section in `teamcity-config.json` for predefined mappings.
   - For unlisted cases, transform remaining Groovy-based logic into Bash/PowerShell or Maven commands (depending on the use).
   - Example mappings:
      - Jenkins `node()` maps to TeamCity's agent specification.
      - Jenkins `retry(condition, count: X)` should be replaced using a retry count mechanism in TeamCity's settings or the manual fallbacks provided in `teamcity-config-manual-migration.json`.

#### **3.6 Parallel Execution**
- TeamCity does not directly support `parallel` execution as defined in Jenkins. Instead:
   - Use **Snapshot Dependencies** or **Build Chains** to implement parallelism.
   - Refer to the `parallelExecution` section in `teamcity-config-manual-migration.json`.

#### **3.7 Conditional Stages**
- For Jenkins `if (...)` blocks or stage conditions:
   - Use **Build Parameters** to define static conditions.
   - Implement parameter validation and rules for execution as suggested in the manual migration JSON.

#### **3.8 Artifact Handling**
- Replace `stash/unstash` in Jenkins with TeamCity's artifact dependencies:
   - Use artifact resolution and storage as defined in `teamcity-config.json`.
   - Configure artifact caching between builds.

#### **3.9 Credential Management**
- Replace Jenkins `withCredentials` with TeamCity's secure storage of credentials.
   - Refer to the steps in the `launchableIntegration` section of `teamcity-config-manual-migration.json`.

#### **3.10 Timeout Handling**
- For `timeout(time: ..., unit: ...)` in Jenkins, map this to TeamCity's step-specific or build configuration-wide timeout settings.

#### **3.11 Agent Selection**
- Map Jenkins `node('label')` to TeamCity agent pools and compatibility rules.
   - Use the `agentSelection` guidelines in the manual migration JSON for this setup.

#### **3.12 Retry Logic**
- Jenkins' advanced retry conditions (like `kubernetesAgent`) cannot be directly mapped. Use build triggers or retry logic in build scripts where needed.
- Refer to the `retryLogic` section in the manual migration JSON.

#### **3.13 Custom Groovy Functions**
- Move custom Groovy functionality into external scripts or meta-runners for TeamCity.
   - Follow steps in `customGroovyFunctions` for converting them to reusable scripts.

### **4. Output Steps**
#### **4.1 Kotlin DSL Files**
- For every element, create Kotlin DSL files such as:
   - **Projects** matching `teamcity-config.json > project`.
   - **Build Templates** that consolidate build steps.
   - **Build Configurations** for every platform-JDK combination.
   - **VCS Root** for the Git repository.

#### **4.2 Update JSON Files**
1. **`teamcity-config.json` Updates**:
   - Include any newly derived build configurations, parameters, or templates.
   - Specify changes to artifact storage or repository settings.

2. **`teamcity-config-manual-migration.json` Updates**:
   - Identify additional parts of the Jenkinsfile that could not be migrated automatically.
   - Log comments for manual migration or adjustments following guidelines.

### **5. Example Mapping**
Here's a snippet of how the `Jenkinsfile` components could map to TeamCity Kotlin DSL:
**Jenkinsfile:**
``` groovy
stage('Record build') {
  retry(conditions: [kubernetesAgent()], count: 2) {
    node('maven-17') {
      sh 'launchable record ...'
    }
  }
}
```
**TeamCity Kotlin DSL:**
``` kotlin
import jetbrains.buildServer.configs.kotlin.v2023_2.*

object BuildStepRecord : BuildType({
    name = "Record Build"
    vcs {
        root(JenkinsCore.vcsRoot)
    }
    steps {
        script {
            scriptContent = "launchable verify && launchable record build..."
        }
    }
    agentRequirement {
        contains("teamcity.agent.name", "maven-17")
    }
    failureConditions {
        executionTimeoutMin = 360
    }
})
```
### **6. Manual Review Checklist**
- Ensure all build configurations match their Jenkinsfile counterparts.
- Verify dynamic stage replacements align with TeamCity's limitations.
- Double-check custom scripts and external integrations for functionality.
