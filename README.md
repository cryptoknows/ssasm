SQL Script Assembler

This utility will scan through subdirectories looking for database scripts (fragments) to assemble into a 
master build script. The master script will be built with appropriate headers for formatting/readability purposes 
(editable in ssasm-header.txt) and will incorporate:

	* The name of script where the fragment originated
	* Pause points (editable in the ssasm-footer.txt file) between each script fragment for review purposes 
 	* Spool commmands to output each fragment executed in /tmp/ss_{SCHEMA_NAME}{N}.txt (N starting at 1) for review, etc.
