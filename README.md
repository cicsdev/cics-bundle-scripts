> [!WARNING]
> **This repository has been archived**
> 
> IBM Legacy Public Repository Disclosure: All content in this repository including code has been provided by IBM under the associated open source software license and IBM is under no obligation to provide enhancements, updates, or support.
> IBM developers produced this code as an open source project (not as an IBM product), and IBM makes no assertions as to the level of quality nor security, and will not be maintaining this code going forward.

You can build and deploy Java applications using [cics-bundle-gradle](https://github.com/IBM/cics-bundle-gradle) and [cics-bundle-maven](https://github.com/IBM/cics-bundle-maven). To automate management of CICS resources, use the [IBM z/OS CICS Ansible collection](https://github.com/ansible-collections/ibm_zos_cics).

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
