# addtocicsbundle

Script to add pre-build Java archive files to a CICS bundle, including:

* .jar files for Java archives
* .war files for web archives
* .eba files for enterprise business archives
* .ear files for enterprise archives
 
If the CICS bundle does not exist, it will be created.

If the CICS bundle contains only pre-built Java archive files, it does not need to be built by the CICS build toolkit.
 
## Requirements

* [bash](https://www.gnu.org/software/bash/bash.html?cm_mc_uid=33935548072714933125385&cm_mc_sid_50200000=1493879051&cm_mc_sid_52640000=1493879738#downloading) is used to run the script. For Linux this is likely pre-installed.
* [xmlstarlet](http://xmlstar.sourceforge.net/overview.php) is used to update the CICS bundle manifest file cics.xml. For Linux this is likely available for installation from your distribution package manager.
 
## Usage
 
~~~~
Usage: addtocicsbundle [-hv] [-j JVMSERVER] [-V VERSION] -a FILES DIRECTORY

Add Java pre-built archive files to the CICS bundle specified by DIRECTORY.

Options:

-a FILES, --add            Files or directory of files to add to the CICS bundle
-h, --help                 Help
-j JVMSERVER, --jvmserver  CICS JVMSERVER resource name
                           The default is DFHWLP for files with .war .ear .eba extensions, and DFHOSGI with .jar extensions
-V VERSION, --version      Set the bundle version in major.minor.micro format
-v, --verbose              Verbose messages

DIRECTORY is the CICS bundle directory
~~~~

## Example

This example will create a CICS bundle in directory output/bundles/com.ibm.cics.server.examples.bundle and add all the Java archives found at javaarchives/*.

Note in the following example the `-a "javaarchives/*"` parameter includes quotation marks to avoid the shell interpreter expanding the * character.

~~~~console
$ addtocicsbundle -v -a "javaarchives/*" output/bundles/com.ibm.cics.server.examples.bundle

Creating CICS bundle manifest file output/bundles/com.ibm.cics.server.examples.bundle/META-INF/cics.xml
Creating CICS bundle part file output/bundles/com.ibm.cics.server.examples.bundle/com.ibm.cics.server.examples.hello.osgibundle
Adding bundle part to the CICS bundle manifest file output/bundles/com.ibm.cics.server.examples.bundle/META-INF/cics.xml
Adding property jvmserver.DFHOSGI=DFHOSGI to output/bundles/com.ibm.cics.server.examples.bundle/variable.properties
Copying file javaarchives/com.ibm.cics.server.examples.hello_1.0.0.jar to output/bundles/com.ibm.cics.server.examples.bundle
Creating CICS bundle part file output/bundles/com.ibm.cics.server.examples.bundle/com.ibm.cics.server.examples.jcics.osgibundle
Adding bundle part to the CICS bundle manifest file output/bundles/com.ibm.cics.server.examples.bundle/META-INF/cics.xml
Copying file javaarchives/com.ibm.cics.server.examples.jcics_1.0.0.jar to output/bundles/com.ibm.cics.server.examples.bundle
Creating CICS bundle part file output/bundles/com.ibm.cics.server.examples.bundle/com.ibm.cics.server.examples.web.osgibundle
Adding bundle part to the CICS bundle manifest file output/bundles/com.ibm.cics.server.examples.bundle/META-INF/cics.xml
Copying file javaarchives/com.ibm.cics.server.examples.web_1.0.0.jar to output/bundles/com.ibm.cics.server.examples.bundle
Creating CICS bundle part file output/bundles/com.ibm.cics.server.examples.bundle/com.ibm.cics.server.examples.wlp.hello.warbundle
Adding bundle part to the CICS bundle manifest file output/bundles/com.ibm.cics.server.examples.bundle/META-INF/cics.xml
Adding property jvmserver.DFHWLP=DFHWLP to output/bundles/com.ibm.cics.server.examples.bundle/variable.properties
Copying file javaarchives/com.ibm.cics.server.examples.wlp.hello.war to output/bundles/com.ibm.cics.server.examples.bundle
Summary for directory:
output/bundles/com.ibm.cics.server.examples.bundle:
total 112
-rw-r--r-- 1 cockerma cockerma  2124 May 15 15:23 com.ibm.cics.server.examples.hello_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   169 May 15 15:23 com.ibm.cics.server.examples.hello.osgibundle
-rw-r--r-- 1 cockerma cockerma 15977 May 15 15:23 com.ibm.cics.server.examples.jcics_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   169 May 15 15:23 com.ibm.cics.server.examples.jcics.osgibundle
-rw-r--r-- 1 cockerma cockerma  5841 May 15 15:23 com.ibm.cics.server.examples.web_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   167 May 15 15:23 com.ibm.cics.server.examples.web.osgibundle
-rw-r--r-- 1 cockerma cockerma 60618 May 15 15:23 com.ibm.cics.server.examples.wlp.hello.war
-rw-r--r-- 1 cockerma cockerma   155 May 15 15:23 com.ibm.cics.server.examples.wlp.hello.warbundle
drwxr-xr-x 2 cockerma cockerma  4096 May 15 15:23 META-INF
-rw-r--r-- 1 cockerma cockerma   122 May 15 15:23 variable.properties

output/bundles/com.ibm.cics.server.examples.bundle/META-INF:
total 4
-rw-r--r-- 1 cockerma cockerma 1064 May 15 15:23 cics.xml
Exiting with RC=0
~~~~

The output directory can now be copied to the CICS [platform directory structure in z/OS UNIX](https://www.ibm.com/support/knowledgecenter/SSGMCP_5.3.0/com.ibm.cics.ts.productoverview.doc/concepts/platforms_directory_structure.html) such as /var/cicsts/CICSPlex/_platform1_/. A BUNDLE resource can then be defined and installed to install in CICS.

Alternatively, the output directory can be copied (staged) to IBM UrbanCode Deploy using the [pushcicsbundletoucd](../pushcicsbundletoucd) script and deployed using the [CICS bundle](../ucdtemplates) template.
