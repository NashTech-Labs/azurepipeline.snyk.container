# ADO.Pipelines.Templates.frameWork.common.snyk

This template uses the Snyk Container test of Snyk Task.

**Note:** Snyk Task is available on market place at <https://marketplace.visualstudio.com/items?itemName=Snyk.snyk-security-scan>

## Pipeline Requirements

The Snyk Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| nodeVersion | Version of Node for Snyk | string | '20.x' | | Required | |
| snykServiceConnectionName  | Azure Service Connection for Snyk in your Azure pipeline | string | | | Required |  Remember to provide permission to pipeline for the snyk token used |
| dockerImageName | name of Docker /  Container image to be tested | string | | | Required | |
| dockerfilePath | Dockerfile location used for dockerimage creation | string | | | Required | |
| severityThreshold | severity type to be used as threshold by snyk | string | 'always' |'always' / 'onIssuesFound' /'never' | Optional | |
| monitorWhen | Condition to run Snyk monitor to capture dependency tree of the application | string | "always" | "always" / "onIssuesFound" / "never" | |
| failOnIssues | specifies whether pipeline should fail or continue based on issues found | boolean | true | true / false | Optional |  |
| projectName | Name of project to be associated with the test report | string | | |  |
| organization | id or name of the organisation under which the project is tested| string | 'always' |'always' / 'onIssuesFound' /'never' | Optional |  |
| testDirectory | Alternate Working Directory | string | | | Optional | Used to test manifest file from this directory other than root |
| additionalArguments | Additional command line arguements related to command selected | string | | | Optional | Reference: <https://docs.snyk.io/snyk-cli/guides-for-our-cli/cli-reference> |
--------------------------------------------------------------------------------------------------------------------------------------------------

## Use Cases

### Snyk Test for Docker / Container Images

  ```yaml
  # azure-pipeline.yml
  resources:
    repositories:
      - repository: Snyk
        type: github
        name: knoldus/azurepipeline.snyk.container
        ref: <respective branch name>
        endpoint: '<Service Connection to Github>'

    steps:
    - template: containerTest.yml@Snyk
      parameters:        
        nodeVersion: '${{parameters.nodeVersion}}'
        snykServiceConnectionName: '${{parameters.snykServiceConnectionName}}'
        severityThreshold: '${{parameters.severityThreshold}}'
        dockerImageName: '${{parameters.dockerImageName}}'
        dockerfilePath: '${{parameters.dockerfilePath}}'
        monitorWhen: '${{parameters.monitorWhen}}'
        failOnIssues: ${{parameters.failOnIssues}}
        projectName: '${{parameters.projectName}}'
        organization: '${{parameters.organisation}}'
        additionalArguments: '${{parameters.additionalArguments}}'
  ```
