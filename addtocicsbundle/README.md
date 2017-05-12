# addtocicsbundle

Script to add pre-build Java archive files to a CICS bundle, including:

* .jar files for OSGI bundles
* .war files for web archives
* .eba files for enterprise business archives
* .ear files for enterprise archives
 
If the CICS bundle does not exist, it will be created. If the CICS bundle contains only pre-built Java archive files, it does not need to be built by the CICS build toolkit.
 
## Requirements

* [bash](https://www.gnu.org/software/bash/bash.html?cm_mc_uid=33935548072714933125385&cm_mc_sid_50200000=1493879051&cm_mc_sid_52640000=1493879738#downloading) is used to run the script. For Linux this is likely pre-installed.
* [xmlstarlet](http://xmlstar.sourceforge.net/overview.php) used to update the CICS bundle manifest file cics.xml.
 
## Usage
 
~~~~
Usage:	addtocicsbundle [-hv] [-j JVMSERVER] -a FILES DIRECTORY

Add Java pre-built archive files to the CICS bundle specified by DIRECTORY.

Options:

-a FILES, --add            Files or directory of files to add to the CICS bundle
-h, --help                 Help
-j JVMSERVER, --jvmserver  CICS JVMSERVER resource name
                           The default is DFHWLP for files with .war .ear .eba extensions, and DFHOSGI .jar extensions
-V, --version              Set the bundle version in major.minor.micro format
-v, --verbose              Verbose messages

DIRECTORY is the CICS bundle directory
~~~~

## Example

This example will create a CICS bundle in directory bundles/MyBundle and add *.war web archive files that will be installed into the JVM sserver called DFHWLP.

Note the `-a "*.jar"` parameter includes quotation marks to avoid the shell interpreter expansion.

~~~~console
$ addtocicsbundle -v -j DFHWLP -a "*.jar" bundles/com.example.mybundle

Creating directory bundles/com.example.mybundle/META-INF
Creating file bundles/com.example.mybundle/com.ibm.cics.server.examples.hello_1.0.0.jar.osgibundle
Adding bundle part file to the manifest file bundles/com.example.mybundle/META-INF/cics.xml
Copying file com.ibm.cics.server.examples.hello_1.0.0.jar to bundles/com.example.mybundle
Creating file bundles/com.example.mybundle/com.ibm.cics.server.examples.jcics_1.0.0.jar.osgibundle
Adding bundle part file to the manifest file bundles/com.example.mybundle/META-INF/cics.xml
Copying file com.ibm.cics.server.examples.jcics_1.0.0.jar to bundles/com.example.mybundle
Creating file bundles/com.example.mybundle/com.ibm.cics.server.examples.web_1.0.0.jar.osgibundle
Adding bundle part file to the manifest file bundles/com.example.mybundle/META-INF/cics.xml
Copying file com.ibm.cics.server.examples.web_1.0.0.jar to bundles/com.example.mybundle
Summary for directory:
bundles/com.example.mybundle:
total 44
-rw-r--r-- 1 cockerma cockerma  2124 May 12 04:54 com.ibm.cics.server.examples.hello_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   156 May 12 04:54 com.ibm.cics.server.examples.hello_1.0.0.jar.osgibundle
-rw-r--r-- 1 cockerma cockerma 15977 May 12 04:54 com.ibm.cics.server.examples.jcics_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   156 May 12 04:54 com.ibm.cics.server.examples.jcics_1.0.0.jar.osgibundle
-rw-r--r-- 1 cockerma cockerma  5841 May 12 04:54 com.ibm.cics.server.examples.web_1.0.0.jar
-rw-r--r-- 1 cockerma cockerma   154 May 12 04:54 com.ibm.cics.server.examples.web_1.0.0.jar.osgibundle
drwxr-xr-x 2 cockerma cockerma  4096 May 12 04:54 META-INF

bundles/com.example.mybundle/META-INF:
total 4
-rw-r--r-- 1 cockerma cockerma 917 May 12 04:54 cics.xml
Exiting with RC=0
~~~~

The bundles/MyBundle directory can now be copied to the CICS [platform directory structure in z/OS UNIX](https://www.ibm.com/support/knowledgecenter/SSGMCP_5.3.0/com.ibm.cics.ts.productoverview.doc/concepts/platforms_directory_structure.html), such as /var/cicsts/CICSPlex/_platform1_/ or another location ready for installing in CICS.

Alternatively, the bundles/MyBundle directory can be copied (staged) to IBM UrbanCode Deploy using the [pushcicsbundletoucd](../pushcicsbundletoucd) script and deployed using the [ucdtemplates](../ucdtemplates).
