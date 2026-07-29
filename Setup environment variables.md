# Setup environment variables

A common requirement is to set environment variables. This can be done by adding an environment variable config file and referencing it in the pipelines. This is now a default part of the template, but older implementations may not have it. You need to do the following:

- Create a new file in the [config folder](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config/EnvironmentVariableConfig.json&version=GBexamples/v1&_a=contents). For example, name it 'EnvironmentVariableConfig.json'. New implementations already have this file.
- Fill in the json file according to the sample syntax below.
- Update the [deployment yaml file](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Templates/Deploy.yml&version=GBexamples/v1&line=58&lineEnd=59&lineStartColumn=1&lineEndColumn=1&lineStyle=plain&_a=contents) to refer to the file just created. New implementations already have this setup.

```json
{
  "Environments": [
    {
      "Name": "Dataverse_Test",
      "Variables": [
        {
          "Name": "schemaname",
          "Value": "value"
        }
      ]
    }
  ]
}
```
