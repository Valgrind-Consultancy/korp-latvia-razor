# GitHub Actions Deployment Setup

This repository is configured to automatically deploy the Korp.Latvia Razor Pages application to Azure Web App when code is pushed to the `main` branch.

## Setup Instructions

### 1. Create an Azure Web App

1. Go to [Azure Portal](https://portal.azure.com)
2. Create a new Web App with these settings:
   - **Runtime stack**: .NET 10
   - **Operating System**: Linux or Windows
   - **Region**: Choose your preferred region

### 2. Download Publish Profile

1. Navigate to your Web App in Azure Portal
2. Click on **Overview** in the left menu
3. Click **Get publish profile** button at the top
4. Save the downloaded `.PublishSettings` file

### 3. Configure GitHub Secrets

1. Go to your GitHub repository
2. Click **Settings** > **Secrets and variables** > **Actions**
3. Click **New repository secret**
4. Create a secret with:
   - **Name**: `AZURE_WEBAPP_PUBLISH_PROFILE`
   - **Value**: Paste the entire contents of the `.PublishSettings` file

### 4. Update Workflow Configuration

Edit `.github/workflows/azure-webapp-deploy.yml` and update:
```yaml
env:
  AZURE_WEBAPP_NAME: 'your-webapp-name'    # Change this to your actual Azure Web App name
```

### 5. Deploy

Once configured, every push to the `main` branch will automatically:
1. Build the .NET 10 application
2. Run tests (if configured)
3. Publish the application
4. Deploy to Azure Web App

## Manual Deployment

You can also trigger the deployment manually:
1. Go to **Actions** tab in GitHub
2. Select **Deploy to Azure Web App** workflow
3. Click **Run workflow**
4. Select the `main` branch and click **Run workflow**

## Monitoring Deployments

- View deployment status in the **Actions** tab
- Check Azure Web App logs in Azure Portal under **Monitoring** > **Log stream**

## Alternative: Using Azure CLI for Authentication

If you prefer using Azure Service Principal instead of Publish Profile:

1. Create a Service Principal:
```bash
az ad sp create-for-rbac --name "github-actions-sp" --role contributor \
    --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group-name} \
    --sdk-auth
```

2. Create a GitHub secret named `AZURE_CREDENTIALS` with the JSON output

3. Update the workflow to use Azure Login action instead of publish profile

See [Azure/login](https://github.com/Azure/login) for more details.
