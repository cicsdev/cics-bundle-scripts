# tagcicsbundle

Script to add pre-build Java archive files to a CICS bundle, including:

* .jar files for OSGI bundles
* .war files for Web Archives
* .eba files for Enterprise Business Archives
* .ear files for Enterprise Archives
 
If the CICS bundle does not exist, it will be created.
 
## Requirements

* [sed](https://www.gnu.org/software/sed/manual/sed.html) stream editor. Used to update the CICS manifest. Typically pre-installed on Linux and [z/OS](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.bpxa400/bpxug375.htm).
 
## Usage
 
~~~~
Usage:	addtocicsbundle [-hv] [-j JVMSERVER] -a FILES DIRECTORY

Add Java files to the CICS bundle specified by DIRECTORY.

Options:

-a FILES, --add            Files or directory of files to add to the CICS bundle
-j JVMSERVER, --jvmserver  CICS JVMSERVER resource name
                           The default is DFHWLP for .war .ear .eba file extensions, and DFHOSGI for .jar file extensions
-h, --help                 Help
-v, --verbose              Verbose messages

DIRECTORY is the CICS bundle directory
~~~~

## Example

Create a CICS bundle in directory input/MyBundle, add *.war Java archive files into it with the CICS bundle parts specifying they shoud be installed in the WLP JVM server. Note the `-a "*war"` parameter includes quotation marks to avoid the shell interpreter expansion.

~~~~console
addtocicsbundle -v -j WLP -a "*.war" input/MyBundle

Copying file com.ibm.cics.server.examples.wlp.hello.war.war to input/MyBundle
Creating CICS bundle part file input/MyBundle/com.ibm.cics.server.examples.wlp.hello.war.war.warbundle
Copying file com.ibm.cics.server.examples.wlp.link.war to input/MyBundle
Creating CICS bundle part file input/MyBundle/com.ibm.cics.server.examples.wlp.link.war.warbundle

Summary for directory:
input/MyBundle:
total 80
-rw-r--r-- 1 cockerma cockerma 60618 May  3 18:20 com.ibm.cics.server.examples.wlp.hello.war.war
-rw-r--r-- 1 cockerma cockerma   147 May  3 18:20 com.ibm.cics.server.examples.wlp.hello.war.war.warbundle
-rw-r--r-- 1 cockerma cockerma  5761 May  3 18:20 com.ibm.cics.server.examples.wlp.link.war
-rw-r--r-- 1 cockerma cockerma   142 May  3 18:20 com.ibm.cics.server.examples.wlp.link.war.warbundle
drwxr-xr-x 2 cockerma cockerma  4096 May  3 18:20 META-INF

input/MyBundle/META-INF:
total 4
-rw-r--r-- 1 cockerma cockerma 748 May  3 18:20 cics.xml
Exiting with RC=0
~~~~
