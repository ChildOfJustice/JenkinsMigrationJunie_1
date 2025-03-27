# Step 1
Generate Guidelines how to convert Jenkinsfile into intermediate json with TeamCity entities representation using JetBrains AI assistant

## Prompt
I have this guidelines file for creating intermideate json file with TeamCity representation for migrating from Jenkins to TeamCity. Now I want to try a bigger Jenkins file with dynamic stages (created via functions)

I need a better guidelines for that - some parts could not be migrated at all, I want to see all build stages, or idea on the function that creates them so later I can convert this json representation into TeamCity kotlin dsl

Give me these improved guidelines for this Jenkinsfile



# Step 2
Run Junie with provided guidelines and ask to create the json representation of TeamCity entities for a specific Jenkinsfile

1 Use guidelines on how to convert this Jenkinsfile into json representation of corresponding TeamCity entities and create this json file for this Jenkinsfile

2 Now go through the json file and think about each entity and if it is probably wont be migrated or cannot be mapped to TeamCity entity - move these parts to scond json, with things that are most likely wont be automatically migrated



# Step 3
Generate guidelines for converting from this intermediate json into TeamCity kotlin configs

prompt

Context:
I want another AI agent to read this jenkins file and migrate it to TeamCity into Kotlin DSL format (like build configurations and projects etc)
Use two json files - teamcity-config.json with representation of TeamCity entities in this Jenkinsfile and teamcity-config-manual-migration.json for comments of parts that maybe cannot be automatically migrated

For examples you can use the settings.kt file

Provide specific guidelines for this AI agent

# Step 4
Run Junie with all provided guidelines, intermediate JSON and ask to convert this Jenkinsfile into TC Kotlin DSL

Migrate this Jenkinsfile into TeamCity Kotlin dsl configuration and use guidelines provided and example settings in json which has the main abstract parts of the Jenkinsfile, and also json file with comments on parts that could be migrated only manually. If Kotlin cannot be built or maven has some problems, or there are unresolved references - thats ok, dont need to fix that


# Step 5
Ask AI assistant to estimate quality of migrated Kotlin DSL

Prompt
I have this Jenkinsfile and I migrated it into the kotlin dsl file for TeamCity - check them and say what is not migrated correctly, what was missed and what could work and what wont work


Ask Junie - Please address this feedback and change the settings.kts file
