# tagcicsbundle
Script to tag files in a CICS bundle with their file encoding based on the file extension. The file tag is used by some file editors in ISPF to automatically convert files to and from non-native EBCDIC code pages to browse and edit files.
 
## Requirements
* chtag available on z/OS 
 
## Usage
 
~~~~
tagcicsbundle [-hv] DIRECTORY

Options:
-h, --help     Help
-v, --verbose  Verbose messages

DIRECTORY is the CICS bundle directory
~~~~
