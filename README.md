The SQL Script Assembler (SSAsm) is designed to streamline a number of activities when organizing and maintaining multiple SQL scripts by:

* Consolidating Execution Scripts -- SSAsm assembles .sql scripts ("fragments") into a single turn-key master execution script that can be easily moved / e-mailed / run between environments (versus manually maintaining such scripts which can introduce loads of human errors and rework)

* Automating Assembly - SSAsm will scan and assemble multiple .sql fragments for a particular schema whether they exist within a single directory or multiple directories

* Maintaining Integrity - SSAsm leaves the original scripts untouched for historical purposes to reduce the chance of human error between script handoff, manual integration, etc.

* Consistent Fragment Ordering - SSAsm always assembles .sql fragements based on SCHEMA name, date, and then by fragment filename, reducing the risk of someone accidentally executing the .sql out of the sequence
 
* Individualized Logging -- SSAsm individually spools the output of each .sql script/fragment (in /tmp/ss_{SCHEMA_NAME}{N}.txt) and includes the name of the original file it was sourced from. These files can then be used for validation, comparison to the original fragments, auditing purposes, etc.

* Supporting Automatic Review Points -- For some databases, SSAsm will (by default) include pause points after each .sql "fragment" executes so the developer / DBA can review the execution (either in the scrollback or the log files) to make any necessary corrections or mitigate any issues before continuing. These review points can be modified / removed by editing the included ssasm_footer.txt file.

---------------------------------------------------------------------------------------------------------------------
Usage Synopsis:

ssasm [-d database_type] [-s schema_name] [-f file_type] path


Options:

-d   - database_type - The DB / client platform the assembled script will be executed against. Accepted values:
                       oracle (default), mysql

-s   - schema_name   - The name of schema (embedded in the folder names) to process. Case sensitive

-f   - file_type      - The file type (suffix) to assemble (default is ".sql" (minus the quotes)). Case insensitive


If schema_name and file type are not specified, the user will be prompted for these.

---------------------------------------------------------------------------------------------------------------------

SSAsm assumes that all changes are organized in directories with the following format:

YYYY-MM-DD-\<SCHEMA NAME\>-\<COMMENTS\>

For example:

_Default name with no comments:_

`2016-01-02-MY_SCHEMA-`                        



_Name with comments added:_

`2016-01-05-MY_SCHEMA-Modified_Version_Per_Scott`


**PLEASE NOTE:**
   * SSAsm will assemble scripts by folder date and then by alphanumeric sort order of each script *within* the folder
   * SSAsm will *ignore* any directories / scripts placed within a "Rejected" folder (case sensitive)

---

**REQUIREMENTS:** 

* Bash Shell
* Linux or OS X
* Oracle+SQL Plus (any platform) or MySQL (non-Windows)

---

**KNOWN ISSUES:**

* SSAsm does not support cross-schema build order dependencies (A.TABLE -> B. PROCEDURE -> A.PROCEDURE)
* MySQL Fragments do not echo the SQL being executed, just the messages / errors
