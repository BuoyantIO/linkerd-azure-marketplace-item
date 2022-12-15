# Minecraft Server Chats

This repo is designed as a template for a Microsoft Partner to create a Kubernetes App in the Azure Marketplace.

## Features

This repo contains the following:

* [Minecraft Server Helm Chart](charts/minecraft) - The sample application
* [UI Definition](createUIDefinition.json) - Used to define experience when purchasing via Azure Portal
* [ARM Template](mainTemplate.json) - Used to define the Azure resources and variables passed during deployment
* [GitHub Action](.github/workflows) - Defines the GitHub Actions to enable CI automation of re-publishing the latest bundle on commit
* [Manifest](mainfest.yaml) - Defines the CNAB bundle, Used for packaging

## Testing

Prior to pushing the offer to the Azure Marketplace, validate the Helm install

```
helm install ./charts/minecraft --generate-name --dry-run --debug
```

## GitHub Actions

The [GitHub Action Workflow](.github/workflows/CNABPackage.yml) will bundle the package and then publish to the ACR described in the manifest file.  To use this Action Workflow make the following changes:

1. Cerate a Repository Secret Named "AZURE_ACR".  Must be the same as the ACR in your manifest file.  This is used in the "Build and Publish Bundle" step.
    1. On the top of the repository select "Settings"
    2. On the left hand side select "Secrets" followed by "Actions" in the dropdown.
    3. Select "New repository secret"
    4. In the name, enter "AZURE_ACR"
    5. For the secret, enter your ACR login server
2. Create the following Repository Secrets: AZURE_CLIENT_ID, AZURE_SUBSCRIPTION_ID, and AZURE_TENANT_ID.  This enables "OIDC Login to Azure Public Cloud" step
    * The values must match the values of your service principal which has access to the ACR
    * Follow the same steps when adding the AZURE_ACR secret
    * For additional information about OIDC Setup with GitHub and Azure, please reference the link [here](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-azure)

## Notes

Helm chart inspired by https://github.com/itzg/minecraft-server-charts
