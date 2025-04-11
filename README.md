# This repository is a saved experiment of converting Jenkinsfile into TeamCity Kotlin DSL using JetBrains Junie AI agent

## Step 1

* Generate Guidelines how to convert Jenkinsfile into intermediate json with TeamCity entities representation using JetBrains AI assistant

### Prompt
I have this guidelines file for creating intermediate json file with TeamCity representation for migrating from Jenkins to TeamCity. Now I want to try a bigger Jenkins file with dynamic stages (created via functions)

I need a better guidelines for that - some parts could not be migrated at all, I want to see all build stages, or idea on the function that creates them so later I can convert this json representation into TeamCity kotlin dsl

Give me these improved guidelines for this Jenkinsfile

### Result - commit 
https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/48a6a26e69b4ab2e0a72efbc10875f50fa8e2f6b



## Step 2
* Run Junie with provided guidelines and ask to create the json representation of TeamCity entities for a specific Jenkinsfile

### Prompt

1 Use guidelines on how to convert this Jenkinsfile into json representation of corresponding TeamCity entities and create this json file for this Jenkinsfile

2 Now go through the json file and think about each entity and if it is probably wont be migrated or cannot be mapped to TeamCity entity - move these parts to a second json, with things that are most likely wont be automatically migrated

### Result
TC json representation - https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/74a9d3f70adb6030c3bc393d16e1cf048c242ef0

Second json file - https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/c354a6bd4eb465785e0d776a9527c1b7ab05ff35

## Step 3
* Generate guidelines for converting from this intermediate json into TeamCity kotlin configs

### Prompt

Context:
I want another AI agent to read this jenkins file and migrate it to TeamCity into Kotlin DSL format (like build configurations and projects etc)
Use two json files - teamcity-config.json with representation of TeamCity entities in this Jenkinsfile and teamcity-config-manual-migration.json for comments of parts that maybe cannot be automatically migrated

For examples you can use the settings.kt file

Provide specific guidelines for this AI agent

### Result

New guidelines for the Jenkinsfile, json -> TC kDSL
https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/5203f38bdd3f18a9909f30355fc8aca5553d5cf3

## Step 4
* Run Junie with all provided guidelines, intermediate JSON and ask to convert this Jenkinsfile into TC Kotlin DSL

### Prompt

Migrate this Jenkinsfile into TeamCity Kotlin dsl configuration and use guidelines provided and example settings in json which has the main abstract parts of the Jenkinsfile, and also json file with comments on parts that could be migrated only manually. If Kotlin cannot be built or maven has some problems, or there are unresolved references - thats ok, dont need to fix that

### Result
https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/5203f38bdd3f18a9909f30355fc8aca5553d5cf3

## Step 5
* 
* Ask AI assistant to estimate quality of migrated Kotlin DSL

### Prompt
I have this Jenkinsfile and I migrated it into the kotlin dsl file for TeamCity - check them and say what is not migrated correctly, what was missed and what could work and what wont work

* Ask Junie - Please address this feedback and change the settings.kts file

### Result 
https://github.com/ChildOfJustice/JenkinsMigrationJunie_1/commit/423da89eef41129914457070f8d53c47f16a84e3