---
title: "Build a SharePoint web part with the Microsoft Graph Toolkit"
description: "Get started using the Microsoft Graph Toolkit to build a SharePoint web part."
localization_priority: Normal
author: elisenyang
---

# Build a SharePoint web part with the Microsoft Graph Toolkit

This topic covers how to use Microsoft Graph Toolkit components in a [SharePoint client-side web part](/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts). Getting started involves the following steps:

1. Set up your development environment and create a web part.
1. Add the Microsoft Graph Toolkit.
1. Add the Microsoft Graph Toolkit SharePoint Framework package.
1. Add the SharePoint Provider.
1. Add components.
1. Configure permissions.
1. Deploy the Microsoft Graph Toolkit SharePoint Framework package.
1. Build and deploy your web part.
1. Test your web part.

## Set up your SharePoint Framework development environment and create a new web part

Follow the steps to [Set up your SharePoint Framework development environment](/sharepoint/dev/spfx/set-up-your-development-environment) and then [create a new web part](/sharepoint/dev/spfx/web-parts/get-started/build-a-hello-world-web-part).

## Add the Microsoft Graph Toolkit

Install the Microsoft Graph Toolkit npm package and polyfills with the following command:

```bash
npm install @microsoft/mgt
```
If you plan to support IE11 in your web parts, you'll need to follow additional steps to ensure cross-browser compatibility:

1. Install the following packages:
```bash
npm install -D babel-loader @babel/core @babel/preset-env webpack
npm install -D @webcomponents/webcomponentsjs regenerator-runtime core-js
```

2. Add the following code to `gulpfile.js`, right above `build.initialize(gulp)`:
```ts
build.configureWebpack.mergeConfig({
  additionalConfiguration: (generatedConfiguration) => {
    generatedConfiguration.module.rules.push(
      {
        test: /\.m?js$/, use:
        {
          loader: "babel-loader",
          options:
          {
            presets: [["@babel/preset-env",
              {
                targets: {
                  "ie": "11"
                }
              }]]
          }
        }
      }
    );

    return generatedConfiguration;
  }
});
```
3. In your `src\webparts\<your-project>\<your-web-part>.ts` file, import the following polyfills before the SharePoint provider in the next step.

```ts
import 'regenerator-runtime/runtime';
import 'core-js/es/number';
import 'core-js/es/math';
import 'core-js/es/string';
import 'core-js/es/date';
import 'core-js/es/array';
import 'core-js/es/regexp';
import '@webcomponents/webcomponentsjs/webcomponents-bundle.js';
```

## Add the Microsoft Graph Toolkit SharePoint Framework package

To prevent multiple SharePoint Framework components from registering their own set of Microsoft Graph Toolkit components on the page, you should deploy the Microsoft Graph Toolkit SharePoint Framework package to your tenant and reference Microsoft Graph Toolkit components that you use in your solution from this package.

The Microsoft Graph Toolkit SharePoint Framework package contains a SharePoint Framework library that registers a single instance of Microsoft Graph Toolkit components in SharePoint.

Install the Microsoft Graph Toolkit SharePoint Framework npm package using the following command:

```bash
npm install @microsoft/mgt-spfx
```

## Add the SharePoint Provider

The Microsoft Graph Toolkit providers enable authentication and access to Microsoft Graph for the components. To learn more, see [Using the providers](../providers/providers.md). SharePoint web parts always exist in an authenticated context because the user has already had to sign in in order to get to the page that hosts your web part. Use this context to initialize the [SharePoint provider](../providers/sharepoint.md).

First, add the provider to your web part. Locate the `src\webparts\<your-project>\<your-web-part>.ts` file in your project folder, and add the following line to the top of your file, right below the existing `import` statements:

```ts
import { Providers, SharePointProvider } from '@microsoft/mgt-spfx';
```

Next, you need to initialize the provider with the authenticated context inside the `onInit()` method of your web part. In the same file, add the following code right before the `public render(): void {` line:

```ts
protected async onInit() {
  if (!Providers.globalProvider) {
    Providers.globalProvider = new SharePointProvider(this.context);
  }
}
```

## Add components

Now, you can start adding components to your web part. Simply add the components to the HTML inside of the `render()` method, and the components will use the SharePoint context to access Microsoft Graph. For example, to add the [Person component](../components/person.md), your code will look like:

```ts
public render(): void {
    this.domElement.innerHTML = `
      <mgt-person person-query="me" view="twolines"><mgt-person>
    `;
}
```

## Configure permissions

To call Microsoft Graph from your SharePoint Framework application, you need to request the needed permissions in your solution package and a Microsoft 365 tenant administrator needs to approve the requested permissions.

To add the permissions to your solution package, locate and open the `config\package-solution.json` file and set:

```json
"isDomainIsolated": false,
```

Just below that line, add the following:

```json
"webApiPermissionRequests":[],
```

Determine which Microsoft Graph API permissions you need depending on the components you're using. Each component's documentation page provides a list of the permissions that component requires. You will need to add each permission required to `webApiPermissionRequests`. For example, if you're using the Person component and the Agenda component, your `webApiPermissionRequests` might look like:

```json
"webApiPermissionRequests": [
  {
    "resource": "Microsoft Graph",
    "scope": "User.Read"
  },
  {
    "resource": "Microsoft Graph",
    "scope": "Calendars.Read"
  }
]
```

## Deploy the Microsoft Graph Toolkit SharePoint Framework package

Before deploying your SharePoint Framework package to your tenant, you will need to deploy the Microsoft Graph Toolkit SharePoint Framework package to your tenant. You can download the package corresponding to the version of Microsoft Graph Toolkit that you used in your project, from the [Releases](https://github.com/microsoftgraph/microsoft-graph-toolkit/releases) section on GitHub.

**Important:** Since there can be only one version of the SharePoint Framework library for Microsoft Graph Toolkit installed in the tenant, before using MGT in your solution, consult with your organization/customer if they already have a version of SharePoint Framework library for Microsoft Graph Toolkit deployed in their tenant and use the same version to avoid issues.

After downloading the Microsoft Graph Toolkit SharePoint Framework .sppkg package, upload it to your SharePoint Online App Catalog. Go to the [More features page of your SharePoint admin center](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true). Select **Open** under **Apps**, then click **App Catalog**, and **Distribute apps for SharePoint**. Upload your `.sppkg` file, and click **Deploy**.

## Build and deploy your web part

Now, you will build your application and deploy it to SharePoint. Build your application by running the following commands:

```bash
gulp build
gulp bundle
gulp package-solution
```

In the `sharepoint/solution` folder, there will be a new `.sppkg` file. You will need to upload this file to your SharePoint Online App Catalog. Go to the [More features page of your SharePoint admin center](https://admin.microsoft.com/sharepoint?page=classicfeatures&modern=true). Select **Open** under **Apps**, then click **App Catalog**, and **Distribute apps for SharePoint**. Upload your `.sppkg` file, and click **Deploy**.

Next, you need to approve the permissions as an administrator.

Go to your **SharePoint Admin center**. In the left-hand navigation, select **Advanced** and then **API Access**. You should see pending requests for each of the permissions you added in your `config\package-solution.json` file. Select and approve each permission.

## Test your web part

You're now ready to add your web part to a SharePoint page and test it out. You will need to use the hosted workbench to test web parts that use the Microsoft Graph Toolkit because the components need the authenticated context in order to call Microsoft Graph. You can find your hosted workbench at **https://<YOUR_TENANT>.sharepoint.com/_layouts/15/workbench.aspx**.

Open the `config\serve.json` file in your project and replace the  value of `initialPage` with the url for your hosted workbench:
```json
"initialPage": "https://<YOUR_TENANT>.sharepoint.com/_layouts/15/workbench.aspx",
```
Save the file and then run the following command in the console to build and preview your web part:

```bash
gulp serve
```

Your hosted workbench will automatically open in your browser. Add your web part to the page and you should see your web part with the Microsoft Graph Toolkit components in action! As long as the gulp serve command is still running in your console, you can continue to make edits to your code and then just refresh your browser to see your changes.

## Next Steps
- Check out this step-by-step tutorial on [building a SharePoint web part](https://developer.microsoft.com/graph/blogs/a-lap-around-microsoft-graph-toolkit-day-9-microsoft-graph-toolkit-sharepoint-provider/).
- Try out the components in the [playground](https://mgt.dev).
- Ask a question on [Stack Overflow](https://aka.ms/mgt-question).
- Report bugs or leave a feature request on [GitHub](https://aka.ms/mgt).