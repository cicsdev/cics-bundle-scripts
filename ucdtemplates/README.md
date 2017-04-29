# CICS bundle
The CICS bundle template for UrbanCode Deploy (UCD) provides the following processes to reliably deploy and undeploy bundles from CICS:
* Deploy
  * Resolves variables in the new version of the bundle, undeploys the BUNDLE resource in CICS, then deploys the new version of the BUNDLE resource in CICS
* Disable
  * Disables the BUNDLE resource in CICS
* Enable
  * Enables the BUNDLE resource in CICS
* Undeploy
  * Disables the BUNDLE resource in CICS, then discards the BUNDLE resource in CICS
* Undeploy and delete 
  * Disables the BUNDLE resource in CICS, discards the BUNDLE resource in CICS, then removes the bundle from zFS

## Requirements
* CICS Transaction Server version 5.1 or later
* CICS build toolkit version 5.3.0 or later installed on zFS
* IBM UrbanCode Deploy version 6.2.3 or later
* IBM CICS TS plug-in for UCD version 38 or later

## Installation
1. Install and configure requirements, including CICS resources in UCD
1. Download CICS+bundle.json
1. Import CICS+bundle.json into UCD
  * Components > Template > Import Template > select "Upgrade Template" > Browse > select CICS+bundle.json > Submit
  
## Usage
1. Set the property cicsbt.directory to the zFS directory where it is installed.
  * This would typically be set in Resources below the agent, for example in the configuration of a CICSplex resource group.
1. Create a component for each CICS bundle
  * Components > Create Component > and complete the details, including:
    * Component Template: CICS bundle
    * cics.bundle.version: 1.0.0
    * cics.bundle.definition.name: <BUNDLE resource name in CICS> 
    * cics.bundle.definition.group.name: <CSD group name in CICS>
    * Optionally, cics.bundle.properties: <properties to use when resolving the bundle>
  * Save
1. Create an application to deploy the CICS bundle
  * Applications > Create Application
1. Add the component to the application
  * Applications > application > Components > Add Component > Select a Component > Save
1. Create an environment
  * Applications > application > Environments > Create Environment
  * Select the environment > Add Base Resources > select CICS resources to deploy the application to 
  * Configuration > Environment Properties > set properties:
    * cics.platform.home to the CICS platform home zFS directory where the CICS bundle will be deployed to
    * Optionally, cics.platform.properties to the properties used when resolving all bundles in this environment
  * Save
1. Create the application processes, such as to deploy and undeploy the application, eg:
  * Applications > application > Processes > Create Process > Name: Deploy application > Save
  * Edit the process
  * Drag "Install Component" from the Application Steps onto the process editor, and edit its properties.
  * Component: select the CICS bundle component 
  * Component Process: select Deploy (Template)
  * OK > Save
1. Deploy the application