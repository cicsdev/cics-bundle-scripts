# cics-bundle-scripts
Sample scripts to process a CICS bundle.


# validatecicsbundle
Script to validate contents of a CICS bundle against a set of rules

## Requirements

* xmllint
* sed
* Linux bash shell

## Usage

~~~~
Usage:  validatecicsbunde [-hv] -r FILE DIRECTORY

Validate the CICS bundle specified by DIRECTORY using the rules specified by FILE.
The script return code is set to 0 if all rules validate, otherwise it is set to >0

Options:
        -h, --help              Help
        -r, --rules FILE        Rules file
        -v, --verbose           Verbose output

Example:
        validatecicsbunde -r ~/rules/production.txt ~/built/bundles/MyBundle

DIRECTORY is the CICS bundle directory to be validated.
FILE is a text file containing rules, one on each line, in the following format:
        file xpath regex

        where:

        file is the file patthern, such as *.urimap or cics.xml
        xpath is the xpath expression to evaluate
        regex is the regular express the xpath expression must match

        Comments starting with #, blank lines, and tab characters are ignored

Example ~/rules/production.txt:
        # The resource name for all URIMAPs must start with EX
        *.urimap string(//@name) ^EX

        # All URIMAP scheme attribute must be HTTP
        *.urimap string(//@scheme) HTTP

        # All URIMAP authenticate attribute must be NO
        *.urimap string(//@authenticate) NO

        # All WEBSERVICE pipeline attribute must be PIPE01
        *.webservice string(//@pipeline) PIPE01

        # All WEBSERVICE wsdlfile attribute must be present
        *.webservice boolean(//*[@validation]) true

        # JVMSERVER resource must not be present
        cics.xml boolean(//*[local-name()='define'][@type=http://www.ibm.com/xmlns/prod/cics/bundle/JVMSERVER]) false
~~~~
