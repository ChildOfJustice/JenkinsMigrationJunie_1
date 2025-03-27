# Step 1
Generate Guidelines how to convert Jenkinsfile into intermediate json with TeamCity entities representation using JetBrains AI assistant

## Prompt
Context:
I want another AI agent to read this jenkins file and convert/migrate it to json format representing TeamCity entities (like build configurations and projects etc)

And this agent should use these steps:
1. define parts of the Jenkinsfile that could be easily migrated to teamcity
2. only migrate the easy parts and highlight the part that could not be properly mapped to TeamCity entities in this result JSON file

# Step 2
Run Junie with provided guidelines and ask to create the json representation of TeamCity entities for a specific Jenkinsfile


# Step 3
Generate guidelines for converting from this intermediate json into TeamCity xml configs

# Step 4
Run Junie with all provided guidelines, intermediate JSON and ask to convert this Jenkinsfile into TC xml configs
