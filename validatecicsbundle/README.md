# validatecicsbundle

Script to validate contents of a CICS bundle against a set of rules.

## Requirements

* xmllint
* sed
* Linux bash shell

## Usage

Validate the CICS bundle specified by DIRECTORY against a rule specified by the -f -x -e options and/or the rules specified in FILE. The script return code is set to 0 if all rules validate to true, otherwise it is set to > 0.

```
validatecicsbundle -hv -f FILEPATTERN -x XPATH -e REGEX -r FILE DIRECTORY

Options:

-h, --help                      Help
-v, --verbose                   Verbose messages
-f, --filepattern FILEPATTERN   Rule file pattern
-x, --xpath XPATH               Rule XPath to evaluate
-e, --regex REGEX               Rule regular expression that the XPath is required to match
-r, --rules FILE                File containing a set of rules
DIRECTORY is the CICS bundle directory to be validated.

FILE is a file containing a logical set of rules, one on each line. Escape characters can be used.
Comments starting with #, blank lines, and tab characters are ignored.
Each rule in FILE should follow the format: FILEPATTERN XPATH REGEX
```

## Examples

For CICS bundle catalog.example.service validate that all *.urimap files contain an attribute 'name' with a value that starts with the characters EX.
`validatecicsbundle -f "*.urimap" -x "string(//@name)" -e "^EX" -v catalog.example.service`

For CICS bundle catalog.example.service validate the rules specified in file rules.txt.
`validatecicsbundle -r rules.txt -v catalog.example.service`

Example rules.txt file.
```
# The resource name for all URIMAPs must start with EX
*.urimap string(//@name) ^EX

# For URIMAPs the scheme attribute must be HTTP
*.urimap string(//@scheme) HTTP

# For URIMAPs the authenticate attribute must be NO
*.urimap string(//@authenticate) NO

# For WEBSERVICEs the pipeline attribute must be PIPE01
*.webservice string(//@pipeline) PIPE01

# For WEBSERVICEs the wsdlfile attribute must be present
*.webservice boolean(//*[@validation]) true

# The JVMSERVER resource must not be present
cics.xml boolean(//*[local-name()='define'][@type="http://www.ibm.com/xmlns/prod/cics/bundle/JVMSERVER"]) false
```
