# CICS bundle

The CICS bundle component template for IBM UrbanCode Deploy (UCD) provides the following processes to reliably deploy bundles to CICS:

* **Deploy** resolves variables in bundle, undeploys the old version of the BUNDLE resource in CICS, copies the bundle to the CICS platform home, then deploys the BUNDLE resource in CICS. You can select if the BUNDLE resource deployed as enabled, available, or disabled.
* **Disable** disables the BUNDLE resource in CICS.
* **Enable** enables the BUNDLE resource in CICS.
* **Make available** makes the BUNDLE resource in CICS available.
* **Make unavailable** makes the BUNDLE resource in CICS unavailable.
* **Undeploy** undeploys the BUNDLE resource in CICS. Optionally you can select to delete the bundle from the CICS platform home.

The processes assume the UCD component contains only one CICS bundle, and the bundle is in a bundles directory.

The processes will wait up to 300 seconds for actions to complete in CICS.

## Requirements

* [IBM UrbanCode Deploy](https://developer.ibm.com/urbancode/products/urbancode-deploy/) version 6.2.3 or later. Used to install the CICS bundle template, create CICS bundle components based on the template, store versions of the CICS bundles, and to run the teplate processes.
* [IBM CICS TS plug-in for UCD](https://developer.ibm.com/urbancode/plugin/cics-ts/) version 38 or later. Provides the process steps used by the CICS bundle template.
* [CICS build toolkit](http://www.ibm.com/support/docview.wss?uid=swg24041185) version 5.3.0 or later installed on zFS. Used by the Deploy template process to resolve variables in the CICS bundle.
* [CICS Transaction Server](https://www.ibm.com/ms-en/marketplace/cics-transaction-server) version 5.1 or later. Used to host the CICS bundle.

## Installation

Import [CICS+bundle.json](CICS+bundle.json) into UCD as a component template.

* **Components** > **Template** > **Import Template** > select Upgrade Template if it was previously installed > **Browse** > select CICS+bundle.json > **Submit**

## Usage

1. Apply the template to the component.

    * For a new component: **Components** > **Create Component** > Component Template: `CICS bundle`
    * For an existing component that does not already have a template:  **Components** > select the component > **Configuration** > **Basic Settings** > Component Template: `CICS bundle`

1. Set the template properties listed in [Template properties](#template-properties).

    * To set a template property in a component: **Components** > select component > **Basic Settings**
    * To set a property in a component version: This is typicaly set by a build system or UCD after it creates the component version, otherwise it can be set manually via **Components** > select component > **Versions** > select version > **Configuration** > **Version Properties**
    * To set a property in the component environment: **Applications** > select application > **Environments** > select envronment > **Configuration** > **Environment Properties** > **Component Environment Properties**
    * To set a property in a resource: **Resources** > select resource > **Configuration** > **Resource Properties**

1. Use the processes provided by the template.

    * In an application process: **Applications** > **Processes** > select process > drag an application step such as **Install Component** onto the process > edit the step > Component Process: select a process such as `Deploy (Template)`

**Note:** To avoid failures in inflight workloads, action should be taken to move or stop the workload in the target CICS systems ahead of deploying or undeploying the CICS bundle, and re-enabling the workload once completed. For example in a rolling deployment scenario, the CICS systems in resource group _CICS-SYSTEM-GROUP-1_ are removed as available targets for the workload and inflight work is drained. CICS systems in other resources groups continue processing the workload. The application is then updated in _CICS-SYSTEM-GROUP-1_ and once complete it is added back in as a target for the workload. The application can similarly be deployed to each of the remaining resource groups in turn. This should allow for a robust, repeatable deployment without failures.

## Template properties

Property | Description | Example | Required | Where to set
--- | --- | --- | --- | ---
cics.bundle.definition.group.name | CSD group name for the BUNDLE resource. | MYGROUP  | Yes | Component
cics.bundle.definition.name | BUNDLE resource name. | MYBUNDLE | Yes | Component
cics.bundle.properties | Property names and values used to resolve variables in the CICS bundle during bundle deployment. Each property should be in the format _name=value_. Separate properties with a new line. | jvmserver=DFHWLP | Optional | Component
cics.bundle.version | Bundle version used to define the BUNDLE  resource in CICS. This would typically be set to the same as bundle version in the bundle manifest file META-INF/cics.xml. | 1.0.0 | Yes | Component version
cics.platform.home | Platform home directory in zFS into which the CICS bundle is copied ready for use by CICS. See [Preparing zFS for platforms](https://www.ibm.com/support/knowledgecenter/en/SSGMCP_5.3.0/com.ibm.cics.ts.doc/eyua7/topics/creating_platform_zfsdirectory.html). | /var/cicsts/CICSplex/platform1 | Yes | Component Environement
cics.platform.properties| Property names and values used to resolve variables in the CICS bundle during bundle deployment. Each property should be in the format _name=value_. Separate properties with a new line. | DSN.HLQ=DEV | Optional | Component Environment
cicsbt.directory | Directory where the CICS build toolkit is installed. | /usr/lpp/cicsbt | Yes | Component Environment or Resource

## Example

1. Create a component for the CICS bundle.
    1. In the UCD web user interface: **Components** > **Create Component** > complete the dialog, including:
        1. Component Template: `CICS bundle`
        1. Component Type: `Standard`
        1. CICS BUNDLE group name: `MYGROUP`
        1. CICS BUNDLE name: `MYBUNDLE`
        1. CICS BUNDLE version: `1.0.0`
    1. **Save**
1. Create an application to deploy the CICS bundle.
    * **Applications** > **Create Application** > complete the dialog.
1. Add the component to the application.
    * **Applications** > select application > **Components** > **Add Component** > Select a Component: select the CICS bundle component > **Save**
1. Create an environment.
    1. **Applications** > select application > **Environments** > **Create Environment**
    1. Select the environment > **Add Base Resources** > select the CICS resources to deploy the application to.
    1. **Configuration** > **Environment Properties** > set the following properties:
        1. cics.platform.home to the CICS platform home zFS directory where the CICS bundle will be deployed to.
        1. cics.platform.properties to the properties used when resolving all bundles in this environment. This is optional.
    1. **Save**
1. Create the application processes, such as to deploy and undeploy the application. For example:
    1. **Applications** > select application > **Processes** > **Create Process** > Name: Deploy application > **Save**
    1. Edit the process.
    1. Drag **Install Component** from the Application Steps onto the process editor, and edit the step properties.
        1. Component: select the CICS bundle component 
        1. Component Process: select Deploy (Template)
        1. **OK** > **Save**
1. Configure your build process to build the CICS bundle from the Eclipse source projects using the CICS build toolkit, then to push the built output into UCD as a version of the CICS bundle component. For example, see the [pushcicsbundletoucd](../pushcicsbundletoucd) script.
1. Deploy the application.

## References

* [Creating Applications Based on Templates - UrbanCode Deploy v.6.2](https://developer.ibm.com/urbancode/videos/creating-applications-based-on-templates-urbancode-deploy-v-6-2/)
