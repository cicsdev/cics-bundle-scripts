# cics-bundle-scripts
Sample scripts to process the contents in a CICS bundle:

* [tagcicsbundle](tagcicsbundle/README.md) - tag files in a CICS bundle with their file encoding based on the file extension. The file tag is used by some file editors in ISPF to automatically convert files to and from non-native EBCDIC code pages to browse and edit files.
* [validatecicsbundle](validatecicsbundle/README.md) - validate contents of a CICS bundle against a set of rules.

For using CICS bundles with IBM UrbanCode Deploy (UCD):

* [pushcicsbundletoucd](pushcicsbundletoucd/README.md) - script to push a CICS bundle into UCD as a component version.
* [ucdtemplates](ucdtemplates/README.md) - component template for UCD to reliably deploy and undeploy bundles from CICS.

## License
This project is licensed under [Apache License Version 2.0](LICENSE).  
