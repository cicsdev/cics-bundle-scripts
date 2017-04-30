# pushcicsbundletoucd
Script to push a CICS bundle into IBM UrbanCode Deploy as a component version.

## Requirements
* [xmllint](http://xmlsoft.org/xmllint.html)
* udclient - download from the UCD web interface by selecting **Help** > **IBM UrbanCode Deploy Client** 

## Usage

~~~~
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
~~~~
	