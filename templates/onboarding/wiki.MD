# Onboarding Agent

## Overview

The Onboarding Agent automates the process of importing repositories into Harness Code and generating CI pipelines. It uses AI to analyze repository contents and create appropriate CI/CD configurations.

## Key Capabilities

- **Repository Import**: Automatically imports repositories from GitHub into Harness Code
- **AI Analysis**: Uses Claude Sonnet to analyze repository structure, technologies, and build requirements
- **Pipeline Generation**: Uses Claude Opus to generate production-ready Harness CI pipelines
- **Activity Tracking**: Creates Harness activities to track onboarding progress

## How It Works

1. Clones the source repository from GitHub
2. Imports the repository into Harness Code
3. Analyzes the repository using Claude Sonnet to understand:
   - Technologies and frameworks used
   - Build and test commands
   - Dependencies and entry points
4. Generates a CI pipeline using Claude Opus based on the analysis
5. Creates the pipeline in Harness

## Inputs

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| anthropicKey | secret | Yes | Anthropic API key for Claude AI |
| harnessKey | secret | Yes | Harness API key for creating pipelines and activities |
| githubToken | secret | No | GitHub token for accessing private repositories |
| repoNamespace | string | Yes | GitHub organization or user (e.g., 'myorg') |
| repoName | string | Yes | Repository name to onboard (e.g., 'my-app') |

## Environment Variables

The agent automatically receives these from Harness CI:
- `HARNESS_ACCOUNT_ID`
- `HARNESS_ORG_ID`
- `HARNESS_PROJECT_ID`

## Example Usage

Run this agent to onboard a GitHub repository:

```yaml
pipeline:
  stages:
    - name: onboarding
      steps:
        - name: onboarding_agent
          run:
            container:
              image: your-registry/onboarding-agent:latest
            env:
              ANTHROPIC_API_KEY: <+secrets.getValue("anthropic_api_key")>
              HARNESS_API_KEY: <+secrets.getValue("harness_api_key")>
              GITHUB_TOKEN: <+secrets.getValue("github_token")>
```

## Troubleshooting

### Common Issues

- **Missing API keys**: Ensure ANTHROPIC_API_KEY and HARNESS_API_KEY are properly configured as secrets
- **Pipeline creation fails**: Verify HARNESS_API_KEY has permissions to create pipelines in the target project
