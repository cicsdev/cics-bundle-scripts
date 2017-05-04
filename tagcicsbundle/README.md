# tagcicsbundle

Script to run on z/OS to set the character set associated with each file in a CICS bundle based on the file extension. The character set is used by some file editors to automatically convert files to and from non-native EBCDIC code pages to browse and edit files. This can make it easier to quickly view CICS bundles when diagnosing issues.
 
## Requirements

* [bash](https://www.gnu.org/software/bash/bash.html?cm_mc_uid=33935548072714933125385&cm_mc_sid_50200000=1493879051&cm_mc_sid_52640000=1493879738#downloading) is used to run the script. For Linux this is likely pre-installed. For z/OS it is available from [Ported Tools](https://www-03.ibm.com/systems/z/os/zos/features/unix/bpxa1ty1.html).
* [chtag](https://www.ibm.com/support/knowledgecenter/en/SSLTBW_2.2.0/com.ibm.zos.v2r2.bpxa500/chtag.htm) is used to set the character set associated with the file as part of the the file tags. This is installed as part of z/OS.
 
## Usage
 
~~~~
tagcicsbundle [-hv] DIRECTORY

Options:

-h, --help     Help
-v, --verbose  Verbose messages

DIRECTORY is the CICS bundle directory.
~~~~
