# tagcicsbundle

Script to add pre-build Java archives to a CICS bundle.

Archives include:

* .jar for OSGI bundles
* .war for Web Archives
* .eba for Enterprise Business Archives
* .ear for Enterprise Archives
 
If the CICS bundle does not exist, it will be created.
 
## Requirements

* [sed](https://www.gnu.org/software/sed/manual/sed.html) stream editor. Used to update the CICS manifest. Typically pre-installed on Linux and [z/OS](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.bpxa400/bpxug375.htm).
 
## Usage
 
~~~~
Usage:	addtocicsbundle [-hv] [-j JVMSERVER] -a FILES DIRECTORY

Add Java files to the CICS bundle specified by DIRECTORY.

Options:

-a FILES, --add             Files or directory of files to add to the CICS bundle
-j JVMSERVER, --jvmserver	CICS JVMSERVER resource name
                            The default is DFHWLP for .war .ear .eba file extensions, and DFHOSGI for .jar file extensions
-h, --help                  Help
-v, --verbose               Verbose messages

DIRECTORY is the CICS bundle directory
~~~~
