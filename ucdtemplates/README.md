# CICS bundle

The CICS bundle template for IBM UrbanCode Deploy (UCD) provides the following processes to reliably deploy and undeploy bundles from CICS:

* `Deploy` - Resolves variables in the new version of the bundle, undeploys the BUNDLE resource in CICS, then deploys the new version of the BUNDLE resource in CICS.
* `Disable` - Disables the BUNDLE resource in CICS.
* `Enable` - Enables the BUNDLE resource in CICS.
* `Undeploy` - Disables the BUNDLE resource in CICS, then discards the BUNDLE resource in CICS.
* `Undeploy and delete` - Disables the BUNDLE resource in CICS, discards the BUNDLE resource in CICS, then deletes the bundle from zFS.

## Requirements

* [CICS Transaction Server version](https://www.ibm.com/ms-en/marketplace/cics-transaction-server) 5.1 or later.
* [CICS build toolkit version](http://www.ibm.com/support/docview.wss?uid=swg24041185) 5.3.0 or later installed on zFS.
* [IBM UrbanCode Deploy](https://developer.ibm.com/urbancode/products/urbancode-deploy/) version 6.2.3 or later.
* [IBM CICS TS plug-in for UCD version](https://developer.ibm.com/urbancode/plugin/cics-ts/) 38 or later.

## Installation

1. Download [CICS+bundle.json](CICS+bundle.json) from this GitHub directory.
1. Import CICS+bundle.json into UCD.
   1. **Components** > **Template** > **Import Template** > select Upgrade Template > **Browse** > select CICS+bundle.json > **Submit**
1. Set the property cicsbt.directory to the zFS directory where the CICS build toolkit is installed. This would typically be set in the resources below the agent, for example in a resource group for the CICSplex. The template Deploy process calls the CICS build toolkit to resolve variables in the CICS bundle.
  
## Usage

1. Create a component for each CICS bundle.
   1. **Components** > **Create Component** > complete the dialog, including:
      1. Component Template: CICS bundle
      1. cics.bundle.version: 1.0.0
      1. cics.bundle.definition.name: BUNDLE resource name in CICS 
      1. cics.bundle.definition.group.name: CSD group name in CICS
      1. cics.bundle.properties: properties to use when resolving the bundle. This is optional.
   1. **Save**
1. Create an application to deploy the CICS bundle.
   1. **Applications** > **Create Application** > complete the dialog
1. Add the component to the application.
   1. **Applications** > select application > **Components** > **Add Component** > Select a Component: select the CICS bundle component > **Save**
1. Create an environment.
   1. **Applications** > select application > **Environments** > **Create Environment**
   1. Select the environment > **Add Base Resources** > select CICS resources to deploy the application to 
   1. **Configuration** > **Environment Properties** > set properties:
      1. cics.platform.home to the CICS platform home zFS directory where the CICS bundle will be deployed to
      1. cics.platform.properties to the properties used when resolving all bundles in this environment. This is optional.
   1. **Save**
1. Create the application processes, such as to deploy and undeploy the application. For example:
   1. **Applications** > select application > **Processes** > **Create Process** > Name: Deploy application > **Save**
   1. Edit the process
   1. Drag "Install Component" from the Application Steps onto the process editor, and edit its properties.
   1. Component: select the CICS bundle component 
   1. Component Process: select Deploy (Template)
   1. **OK** > **Save**
1. Configure your build process to build the CICS bundle from the Eclipse source projects and push it into UCD as a version of the CICS bundle component. For example, see the [pushcicsbundletoucd](https://github.com/cicsdev/cics-bundle-scripts/tree/master/pushcicsbundletoucd) script.
1. Deploy the application.

## Reference
