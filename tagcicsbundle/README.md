# tagcicsbundle

Script to run on z/OS to tag files in a CICS bundle with a file encoding based on the file extension. The file tag is used by some file editors to automatically convert files to and from non-native EBCDIC code pages to browse and edit files. This can make it easier to quickly view CICS bundles when diagnosing issues.
 
## Requirements

* [chtag](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.bpxa500/chtag.htm) is used to set the character set associated with the file as part of the the file tags. This is installed as part of z/OS.
 
## Usage
 
~~~~
tagcicsbundle [-hv] DIRECTORY

Options:

-h, --help     Help
-v, --verbose  Verbose messages

DIRECTORY is the CICS bundle directory.
~~~~
