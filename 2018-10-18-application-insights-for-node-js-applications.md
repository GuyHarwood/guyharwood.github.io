---
title: Application Insights for Node JS Applications
slug: application-insights-for-node-js-applications
date_published: 2018-10-18T12:54:32.000Z
date_updated: 2026-02-16T14:00:13.000Z
---

At the time of writing, there is no user journey through the Azure Portal to configure Application Insights from your Linux App Service.  The tab is greyed out.

Enabling it is fairly straightforward, but not obvious...

1. Ensure your target web application is created first
2. Add the `[application-insights](https://www.npmjs.com/package/applicationinsights)` npm package to your node app, and [configure it](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-nodejs)
3. Create an Application Insights instance in your Azure resource group
4. Go to the overview tab in the Application Insights instance.
5. Click the **monitor resource group **button on the toolbar
6. You are now presented with the resource group overview page for Application Insights
7. From the list of available resources, navigate to your target web application
8. Now navigate to the actions list on the right, and choose **Enable**
9. Confusingly, you are now presented with the option to create a new Application Insights instance
10. Go to the option below entitled **Select Existing Resource** and select the Application Insights instance you created in step 3
11. Click OK

All this effectively does is add an environment variable called `APPINSIGHTS_INSTRUMENTATIONKEY` to the target web application.
