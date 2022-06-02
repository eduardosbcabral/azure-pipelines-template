# Azure Pipelines Templates
Repository that contains our azure pipelines templates that are used in the CI/CD workflow of the .NET applications.

It is a refactored and improved version of the previously version.

# Instructions
We have two templates that we can use in our applications ([github flow](githubflow.azure-pipelines.yml) and [git flow](gitflow.azure-pipelines.yml)). To use them, the only thing you have to do is change the name to azure-pipelines.yaml e put in to the ./devops folder.

For now, it is required to use it together with two other variable templates, variables.production.yml and variables.staging.yml in the devops folder.

You have two edit and change a few things to adapt to your needs. In this part you will have to change the last two lines and change to your default pipeline variable group and the specific api variable group. 
```yaml
variables:
  - template: 'variables/defaults.template.yml@templates' # Default static pipeline variables
  - group: DEFAULT_PIPELINE_LIB
  - group: API_PIPELINE_VARIABLES
```

# Default pipeline variable group
In my case, the default variable group name is **Pipeline_Defaults**. And inside it, you will have a few variables that will be shared across all your azure devops projects pipelines.

## DockerRegistry
The url to the company's docker registry.

## DockerRegistryAccount
Your azure devops organization service docker connection name. You can find or configure it this way:
- Go to **Project Settings** when inside your azure devops project in the left bottom button.
- Go to **Service Connections** and there you will find or configure your docker service name.


## Organization
Our azure devops organization name.

## SonarqubeAccount
Your azure devops organization service Sonarqube connection name. You can repeat the same steps that you used to find the docker registry connection name.

# **Specific pipeline variable group**
In this variable group, you will put your api specific variables that suits your workflow.

## DockerImageName (Required when **ExecuteDockerPublish** is true)
Variable that is needed when the api is deployed using Docker. It is the docker image name.

## EnvVarsPrefix (Required)
Important variable that it's value will be used to preffix all application variables when deployed.

## ExecuteDockerPublish (Required)
Flag variable that controls if the application will be deployed using docker or not.

## ExecuteIntegrationTests (Required)
Flag variable that controls if the integration tests will be executed when deploying to staging.

## ExecuteSdkPublish (Required)
Flag variable that controls if the application will deploy a SDK project to nuget.

## ExecuteSonarqube (Required)
Flag variable that controls if the sonarqube static analysis will be executed every time a build is triggered.

## ExecuteUnitTests (Required)
Flag variable that controls if the application xunit unit tests will be executed every time a build is triggered.

## IntegrationTestsDelay (Required if **ExecuteIntegrationTests** is true)
Time that the pipeline will wait after staging deployment and before executing the integration tests. The format is **15s** for example.

## MainProjectFolder (Required)
Path to the main project folder.

## MainProjectName (Required if ExecuteSonarqube is true)
Name of the main project. It will be used as the name of the Sonarqube project in sonar cloud.

## NugetConfigPath (Not Required)
Full path to the nuget config file. It will be used as an argument when building the application to restore private packages.

## SdkProjectFolder (Required if **ExecuteSdkPublish** is true)
Path to the SDK project folder.

## SonarqubeProject (Required if **ExecuteSonarqube** is true)
Name of the sonar cloud project that was created by an administrator to hold the api analysis.

## TestProjectFolder (Required if **ExecuteUnitTests** is true)
Path to the test project folder.

# Github resources
Now you only have to change to your github project service connection name. Just the value in the **endpoint** property.
```yaml
resources:
  repositories:
  - repository: templates
    type: github
    name: eduardosbcabral/azure-pipelines-templates
    endpoint: YOUR_SERVICE_GITHUB_CONNECTION_NAME
```

Now you are ready to go and using a shared azure pipeline.
