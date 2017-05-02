# CICS bundle

The CICS bundle component template for IBM UrbanCode Deploy (UCD) provides the following processes to reliably deploy bundles to CICS:

* **Deploy** resolves variables in the new version of the bundle, undeploys the old version of the BUNDLE resource in CICS, then deploys the new version of the BUNDLE resource in CICS.
* **Undeploy** disables the BUNDLE resource in CICS, then discards the BUNDLE resource in CICS.
* **Undeploy and delete** disables the BUNDLE resource in CICS, discards the BUNDLE resource in CICS, then deletes the bundle from zFS.
* **Enable** enables the BUNDLE resource in CICS.
* **Disable** disables the BUNDLE resource in CICS.

The processes assume the component contains only one CICS bundle.

The processes will wait up to 300 seconds for actions to complete in CICS.

## Requirements

* [IBM UrbanCode Deploy](https://developer.ibm.com/urbancode/products/urbancode-deploy/) version 6.2.3 or later. Used to store the CICS bundle as a component version, and to coordinate the deployment of the CICS bundle to CICS.
* [IBM CICS TS plug-in for UCD](https://developer.ibm.com/urbancode/plugin/cics-ts/) version 38 or later. Provides the process steps to deploy, undeploy, enable, and disable a CICS bundle.
* [CICS build toolkit](http://www.ibm.com/support/docview.wss?uid=swg24041185) version 5.3.0 or later installed on zFS. Used by the Deploy template process to resolve variables in the CICS bundle.
* [CICS Transaction Server](https://www.ibm.com/ms-en/marketplace/cics-transaction-server) version 5.1 or later. Used to host the CICS bundle.

## Installation

Import [CICS+bundle.json](CICS+bundle.json) into UCD as a component template.

* **Components** > **Template** > **Import Template** > select Upgrade Template > **Browse** > select CICS+bundle.json > **Submit**

## Usage

1. Apply the template to the component:

    * For a new component: **Components** > **Create Component** > Component Template: `CICS bundle`
    * For an existing component that does not already have a template:  **Components** > select the component > **Configuration** > **Basic Settings** > Component Template: `CICS bundle`

1. Set the following properties:

    Property | Description | Example | Required | Where to set
--- | --- | --- | --- | ---
cics.bundle.definition.group.name | CICS CSD group name | MYGROUP  | Yes | Component
cics.bundle.definition.name | The name of the BUNDLE resource in CICS. | MYBUNDLE | Yes | Component
cics.bundle.properties | Properties used to resolve variables in the CICS bundle variables by the deploy process. Each property should be in the format _name=value_. Separate properties with a new line. | jvmserver=DFH$WLP | Optional | Component
cics.bundle.version | The CICS bundle version used to define the BUNDLE  resource in CICS. | 1.0.0 | Yes | Component version
cics.platform.home | The CICS platform home directory in zFS. See [Preparing zFS for platforms](https://www.ibm.com/support/knowledgecenter/en/SSGMCP_5.3.0/com.ibm.cics.ts.doc/eyua7/topics/creating_platform_zfsdirectory.html). | /var/cicsts/CICSplex/platform1 | Yes | CICSplex resource
cics.platform.properties| Properties used to resolve variables in the CICS bundle variables by the deploy process. Each property should be in the format _name=value_. Separate properties with a new line. | | Optional | CICSplex resource
cicsbt.directory | The zFS directory where the CICS build toolkit is installed. This would typically be set in a resource below the agent, for example in a resource group for the CICSplex. | /usr/lpp/cicsbt | Yes | CICSplex resource

## Example

1. Create a component for the CICS bundle.
    1. **Components** > **Create Component** > complete the dialog, including:
        1. Component Template: `CICS bundle`
        1. Component Type: `Standard`
        1. cics.bundle.definition.group.name: `MYGROUP`
        1. cics.bundle.definition.name: `MYBUNDLE`
        1. cics.bundle.version: `1.0.0`
    1. **Save**
1. Create an application to deploy the CICS bundle.
    * **Applications** > **Create Application** > complete the dialog
1. Add the component to the application.
    * **Applications** > select application > **Components** > **Add Component** > Select a Component: select the CICS bundle component > **Save**
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
    1. Drag "Install Component" from the Application Steps onto the process editor, and edit the step properties.
    1. Component: select the CICS bundle component 
    1. Component Process: Deploy (Template)
    1. **OK** > **Save**
1. Configure your build process to build the CICS bundle from the Eclipse source projects and push it into UCD as a version of the CICS bundle component. For example, see the [pushcicsbundletoucd](https://github.com/cicsdev/cics-bundle-scripts/tree/master/pushcicsbundletoucd) script.
1. Deploy the application.

## References

* [Creating Applications Based on Templates - UrbanCode Deploy v.6.2](https://developer.ibm.com/urbancode/videos/creating-applications-based-on-templates-urbancode-deploy-v-6-2/)
