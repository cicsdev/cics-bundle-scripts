# cics-bundle-scripts
Sample scripts to process the contents in a CICS bundle:

* [addtocicsbundle](addtocicsbundle) adds pre-built Java archive files to a CICS bundle.
    Can be used by build systems to dynamically package Java applications ready for deployment into CICS without the need to use CICS Explorer.

* [tagcicsbundle](tagcicsbundle) tags text files in a CICS bundle with an encoding based on the file extension.
    Once files in the CICS bundle have the correct file tag encoding, editors such as ISPF can be used to easily view them which can be useful for problem diagnosis.

* [validatecicsbundle](validatecicsbundle) validates the contents of a CICS bundle against a set of rules.
    Can be used by build systems to check certain company convensions or best practices are implemented, for example naming conventions or specific resource attributes are enabled or disabled.

For using CICS bundles with IBM UrbanCode Deploy (UCD):

* [pushcicsbundletoucd](pushcicsbundletoucd) pushes a CICS bundle into UCD as a component version.
    Can be used by build systems to copy (stage) a CICS bundle into UCD ready for later deployment.

* [ucdtemplates](ucdtemplates) is a component template for UCD to reliably deploy and undeploy bundles from CICS.
    Can be used to easily deploy and undeploy applications that include CICS bundles.

## License
This project is licensed under [Apache License Version 2.0](LICENSE).
