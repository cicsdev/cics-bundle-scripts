# pushcicsbundletoucd
Script to push a CICS bundle into IBM UrbanCode Deploy as a component version.

## Requirements

* [xmllint](http://xmlsoft.org/xmllint.html) that supports the --xpath parameter. This is used to parse CICS bundle parts.
* udclient - download from the UCD web interface by selecting **Help** > **IBM UrbanCode Deploy Client**. This is used to interact with UCD to remove and create a component version, and add properties and files to a component version.

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

1. Export the CICS bundle ready for building.
    **File** > **Export..** > **File System** > select the project > To directory: `/build/input` > **Finish**

1. Build the CICS bundle using the CICS build toolkit.
    `cicsbt --input /build/input/* --build com.example.server --output /build/output`

1. Push the CICS bundle into UCD as a component version.
    `pushcicsbundletoucd ./build/output`

    Alternateively, to append a time stamp to the name of the component version.
    ```pushcicsbundletoucd -q "-"`date +%Y-%m-%d-%H:%M:%S` ./build/output` ```

