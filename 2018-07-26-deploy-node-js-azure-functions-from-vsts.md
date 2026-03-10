---
title: Deploy Node JS Azure Functions from AzureDevOps
slug: deploy-node-js-azure-functions-from-vsts
date_published: 2018-07-26T19:33:11.000Z
date_updated: 2026-02-16T14:00:50.000Z
tags: Serverless, Azure, Azure-Functions, Deployment
---

### UPDATE OCTOBER 2018

Much has changed since this post was written, and Azure Functions V2 has been officially released, with V3 now on the way.  Func pack has been deprecated in favor of [Run From Package](https://docs.microsoft.com/en-us/azure/azure-functions/run-functions-from-deployment-package), and the tooling has been improved.  Some of this content may well now be incorrect, and you should consult the [official documentation](https://docs.microsoft.com/en-us/azure/azure-functions/functions-continuous-deployment).

During October we ran into a lot of issues getting Javascript Azure Functions deployed and working.  The functions team were helpful throughout this time, and a few things became clear....

- Run from package only works on Windows based function apps
- There are deployment timeout issues with Linux based function apps (due to large node_modules folders, apparently)

As of October 2018 there is still no official documentation on how to deploy an Azure Function written in Javascript via Visual Studio Team Services.

To follow this article you will need the following

- [Azure Functions Core Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local) installed locally
- Access to an instance/account on Visual Studio Team Services

# 1) Create a function app in Azure

Head over to Azure and create a function app.  It doesn't matter whether you use an App Service or Consumption plan.

Go to the **Application Settings** and ensure the following are configured...

`FUNCTIONS_WORKER_RUNTIME`=`node`
`WEBSITE_NODE_DEFAULT_VERSION` is either `8.9.4` or `10.6.0`
`WEBSITE_RUN_FROM_PACKAGE`=1

# 2) Create a Javascript function app locally

See the [official getting started article](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local) for up to date instructions if you don't already know how to do this.  You should create a Javascript function with a HTTP trigger.

Alternatively, you can just clone [my function app repository](https://github.com/GuyHarwood/js-azure-function) that i created while writing this article.

# 3) Create the VSTS build definition

Create an empty build definition and hook the source up to your remote repository, or [my function app repository](https://github.com/GuyHarwood/js-azure-function).  When selecting a build agent - if you are not using a hosted agent (which includes `npm`) then ensure your agent has this installed.

Now lets add the build tasks...

### npm install

Add an **npm** task which calls `install`.  Self explanatory.

### install functions core tools

Add a custom **npm** task which calls `install -g azure-functions-core-tools`

### install function bindings

Add a custom **npm** task which calls `func extensions install`

### copy files to staging folder

Add a **copy files** task and specify `$(Build.ArtifactStagingDirectory)/functions` as the target folder

### archive artifacts

Add an **archive** task with root folder set to `$(Build.ArtifactStagingDirectory)`.

You MUST Ensure the **prepend root folder** option remains unchecked.

Set **archive file to create** to `$(Build.ArtifactStagingDirectory)/Functions.zip`

### publish artifacts

Add a **publish artifacts** task with **path to publish** set to `$(Build.ArtifactStagingDirectory)/Functions.zip` and **Artifact Name** set to `drop`

### deploy to functions app in azure

Add an **Azure CLI** task with the content set to...

    az functionapp deployment source config-zip  -g yourResourceGroup -n yourFunctionAppName --src $(Build.ArtifactStagingDirectory)\Functions.zip
    

**Subscription** must be set to your Azure account, if the drop down contains no entries then you need to [connect your Azure subscription to VSTS via a Service Principal](https://docs.microsoft.com/en-us/vsts/pipelines/library/connect-to-azure?view=vsts).

Finally, select the function app in the **App Service Name** box, or type it in if nothing comes up (it can be sporadic!)

That's it.  Now run the build, and you should end up with a successful deployment...

![Successful Node Azure Function Deployment](__GHOST_URL__/content/images/2018/10/Screenshot-2018-10-18-14.08.43.png)

Be aware that Azure functions V2 is very much in development, and is now being extended to support containers and kubernetes.  How much of this article will still be relevant six months from now is anyones guess.  It's great that Microsoft have taken a different approach with V2 and are now giving cross-platform development a priority.  Consumption plans have always felt like the true 'serverless' way to me, but with support for Kubernetes just being announced it puts some interesting options on the table.
