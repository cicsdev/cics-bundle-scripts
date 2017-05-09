# tagcicsbundle

Script to add pre-build Java archive files to a CICS bundle, including:

* .jar files for OSGI bundles
* .war files for web archives
* .eba files for enterprise business archives
* .ear files for enterprise archives
 
If the CICS bundle does not exist, it will be created. If the CICS bundle contains only pre-built Java archive files, it does not need to be built by the CICS build toolkit.
 
## Requirements

* [bash](https://www.gnu.org/software/bash/bash.html?cm_mc_uid=33935548072714933125385&cm_mc_sid_50200000=1493879051&cm_mc_sid_52640000=1493879738#downloading) is used to run the script. For Linux this is likely pre-installed. For z/OS it is available from [Ported Tools](https://www-03.ibm.com/systems/z/os/zos/features/unix/bpxa1ty1.html).
* [sed](https://www.gnu.org/software/sed/manual/sed.html) stream editor. Used to update the CICS manifest. Typically pre-installed on Linux and [z/OS](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.bpxa400/bpxug375.htm).
 
## Usage
 
~~~~
Usage:	addtocicsbundle [-hv] [-j JVMSERVER] -a FILES DIRECTORY

Add Java pre-built archive files to the CICS bundle specified by DIRECTORY.

Options:

-a FILES, --add            Files or directory of files to add to the CICS bundle
-j JVMSERVER, --jvmserver  CICS JVMSERVER resource name
                           The default is DFHWLP for files with .war .ear .eba extensions, and DFHOSGI .jar extensions
-h, --help                 Help
-v, --verbose              Verbose messages

DIRECTORY is the CICS bundle directory
~~~~

## Example

This example will create a CICS bundle in directory bundles/MyBundle and add *.war web archive files that will be installed into the JVM sserver called DFHWLP.

Note the `-a "*war"` parameter includes quotation marks to avoid the shell interpreter expansion.

~~~~console
$ addtocicsbundle -v -j DFHWLP -a "*.war" bundles/MyBundle

Creating empty CICS bundle at bundles/MyBundle
Copying file com.ibm.cics.server.examples.wlp.hello.war.war to bundles/MyBundle
Creating CICS bundle part file bundles/MyBundle/com.ibm.cics.server.examples.wlp.hello.war.war.warbundle
Copying file com.ibm.cics.server.examples.wlp.link.war to bundles/MyBundle
Creating CICS bundle part file bundles/MyBundle/com.ibm.cics.server.examples.wlp.link.war.warbundle

Summary for directory:
bundles/MyBundle:
total 80
-rw-r--r-- 1 cockerma cockerma 60618 May  3 19:32 com.ibm.cics.server.examples.wlp.hello.war.war
-rw-r--r-- 1 cockerma cockerma   143 May  3 19:32 com.ibm.cics.server.examples.wlp.hello.war.war.warbundle
-rw-r--r-- 1 cockerma cockerma  5761 May  3 19:32 com.ibm.cics.server.examples.wlp.link.war
-rw-r--r-- 1 cockerma cockerma   138 May  3 19:32 com.ibm.cics.server.examples.wlp.link.war.warbundle
drwxr-xr-x 2 cockerma cockerma  4096 May  3 19:32 META-INF

bundles/MyBundle/META-INF:
total 4
-rw-r--r-- 1 cockerma cockerma 748 May  3 19:32 cics.xml
Exiting with RC=0
~~~~

The bundles/MyBundle directory can now be copied to the CICS [platform directory structure in z/OS UNIX](https://www.ibm.com/support/knowledgecenter/SSGMCP_5.3.0/com.ibm.cics.ts.productoverview.doc/concepts/platforms_directory_structure.html), such as /var/cicsts/CICSPlex/_platform1_/ or another location ready for installing in CICS.

Alternatively, the bundles/MyBundle directory can be copied (staged) to IBM UrbanCode Deploy using the [pushcicsbundletoucd](../pushcicsbundletoucd) script and deployed using the [ucdtemplates](../ucdtemplates).
