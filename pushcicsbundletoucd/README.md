# pushcicsbundletoucd
Script to push a CICS bundle into IBM UrbanCode Deploy (UCD) as a component version.

## Requirements

* [bash](https://www.gnu.org/software/bash/bash.html?cm_mc_uid=33935548072714933125385&cm_mc_sid_50200000=1493879051&cm_mc_sid_52640000=1493879738#downloading) is used to run the script. For Linux this is likely pre-installed. For z/OS it is available from [Ported Tools](https://www-03.ibm.com/systems/z/os/zos/features/unix/bpxa1ty1.html).
* [xmllint](http://xmlsoft.org/xmllint.html) XML parser, in particular a version that supports the --xpath parameter. Used to parse CICS bundle parts. Typically pre-installed on Linux as part of the libxml2 library. It is not currently available on z/OS.
* [udclient](https://www.ibm.com/support/knowledgecenter/en/SS4GSP_6.2.4/com.ibm.udeploy.reference.doc/topics/cli_ch.html) command-line client for UCD. This is used to interact with UCD to remove and create a component version, and to add properties and files to a component version. Install from the UCD web interface by selecting **Help** > **IBM UrbanCode Deploy Client**.

## Usage

```
pushcicsbundletoucd [-hr] [-n COMPONENT_NAME] [-v COMPONENT_VERSION_NAME] [-q VERSION_QUALIFIER] DIRECTORY

Options:

-h, --help       Help
-n, --name       Component name
                 If not specified, the CICS bundle ID of the first bundle found is used.
-q, --qualifier  Component version qualifier
-v, --version    Component version name
                 If not specified, the CICS bundle version of the first bundle found is used.
-r, --replace    Replace the existing component version if it exists

DIRECTORY is the output directory created by the CICS build toolkit to be pushed.
```

## Example

1. Create the component using the UCD web interface.

    **Components** > **Create Component** > Name: `com.example.service` > **Save**

1. Create the CICS bundle using the CICS Explorer.

    **File** > **New** > **CICS Bundle Project** > **Project name:** `com.example.service` > **Finish**

1. Export the CICS bundle ready for building using the CICS Explorer.

    **File** > **Export..** > **File System** > select the project > To directory: `/build/input` > **Finish**

1. Build the CICS bundle using the CICS build toolkit.

    `cicsbt --input /build/input/* --build com.example.server --output /build/output`

1. Push the CICS bundle into UCD as a component version.

    `pushcicsbundletoucd ./build/output`

    Alternateively, to append a time stamp to the name of the component version.
    
    ````pushcicsbundletoucd -q "-"`date +%Y-%m-%d-%H:%M:%S` ./build/output````

