# GitHub Enterprise Server, OpenID Connect, and Azure Demo

Standard .NET app used to demonstrate using OpenID Connect (OIDC) with GitHub Enterprise Server (GHES) to deploy to Azure App Service.

## Getting Started

### OIDC and Azure Service Principal Setup

To get started, follow these instructions to create and register an application and to set up the federated credentials:

1. [Create an Azure Active Directory application and service principal](https://docs.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Clinux#create-an-azure-active-directory-application-and-service-principal)

1. [Add federated credentials](https://docs.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-portal%2Clinux#add-federated-credentials)

   * The issuer should be `https://{{YOUR-GHES-ADDRESS}}/_services/token`, for example `https://bxtp4p.ghes-demo.com/_services/token`

   * The [dotnet workflow](./.github/workflows/dotnet.yml) in this repo is configured to run on pull requests. Therefore, choose `Pull request` for the `Entity type`.

   * For `Organization` and `Repository`, choose your org/user and this repository name.

      Full credential configuration example:

      ![Federated credentials](/assets/README/2022-06-06-09-31-32.png)

1. Save the `Application (client) ID` and the `Directory (tenant) ID`:

   ![Client ID and Tenant ID](/assets/README/2022-06-06-09-36-41.png)

1. Find your `Subscription ID` and save it as well.

1. Create or use an existing `Resource Group`.

1. In the resource group, navigate to `Access control (IAM)`.

1. Add a new role assignment

   ![New role assignment](/assets/README/2022-06-06-09-45-25.png)

1. Select `Contributor` for the role and then select the service principal (app) you created earlier:

   ![Service principal role assignment](/assets/README/2022-06-06-09-48-12.png)

1. Review and assign the role.

   ![Review and confirm assignment](/assets/README/2022-06-06-09-49-28.png)

### Azure App Service Setup

1. Under App Services, create a new app:

   ![Create app](/assets/README/2022-06-06-09-53-00.png)

1. Configure the Web App with the following settings:

   * **Resource Group:** Select the resource group you previously created.

   * **Publish:** Code

   * **Runtime stack:** .NET 6 (LTS)

   * **Operating System:** Linux

   * Other settings (e.g., region, sku and size) are your choice.

1. Skip through to the `Review + create`, confirm your choices, then click the `Create` button.

1. Once the app is created, under the app's Overview page, click the `Get publish profile` button to download the publishing profile.

1. Open the publish profile and remove the `userName` and `userPWD` attributes and values from each `publishProfile` and save the file for later use in GitHub.

### GitHub Setup

Add the following `Repository secrets` and use the values you saved previously:

* AZURE_CLIENT_ID
* AZURE_TENANT_ID
* AZURE_SUBSCRIPTION_ID
* AZURE_PUBLISH_PROFILE - copy and paste the entire contents of the publishing profile file saved earlier.

## Running the workflow and viewing the results

The workflow can be triggered manually or by creating a pull request. Once executed, you can navigate to the site from the links found in the logs:

![Results](/assets/README/2022-06-06-10-15-28.png)

![Logs](/assets/README/2022-06-06-10-16-03.png)

![Website](/assets/README/2022-06-06-10-17-19.png)
