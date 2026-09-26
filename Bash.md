# Built-In Commands

Actual Bash builtins (no separate binary, run by the shell itself):

- `cd` — Change the directory to a different location;
- `echo` — Print text to the terminal;
- `history` — Show a list of previously executed commands;
- `read` — Read the user's input into a variable;
- `set` — Set options or positional parameters for the shell;
# External Commands

Everything below is a separate program (mostly from `coreutils`, except where noted), not a builtin:

- `ls` — List the contents of the current directory;
- `mkdir` — Create a new directory;
- `rmdir` — Removes a directory if it's empty;
- `touch` — Create a new file;
- `rm` — Remove a file or directory;
- `cp` — Copy a file or directory;
- `mv` — Move or rename a file or directory;
- `cat` — Concatenate and print the contents of a file;
- `grep` — Search for a pattern in a file (package `grep`);
- `chmod` — Change the permissions of a file or directory;
- `sudo` — Run a command with administrative privileges (package `sudo`);
- `df` — Display the amount of disk space available;
- `ps` — Display information about running processes (package `procps-ng`);
- `which` — Locates a program's binary in the user's `$PATH`. Takes a command as its argument (package `which`);
# Syntax

Valid variable naming:

```
name
count
_var
myVar
MY_VAR
```
# Scripting
# Shebang

It's conventional to add so called "Shebang" in the beginning of a Bash script file to point the system to the Bash binary file:

```bash
#!/bin/bash
```

Then goes the script itself.
## Syntax

- `# <text>` — Comment;
### Conditions
#### If-Elif-Else

```bash
if [[ <condition> ]]; then
	<statement>
elif [[ <condition> ]]; then
	<statement>
else
	<statement>
fi
```
### Logical Operators

- `-gt` — Greater than;
- `-lt` — Less than;

`-a` (AND) and `-o` (OR) only work inside single-bracket `[ ]` tests — they're a syntax error inside `[[ ]]`. Inside `[[ ]]`, use `&&` and `||` instead:

```bash
if [[ $a -gt 60 && $b -lt 100 ]]
```
### Variables

- `-n $<var>` — True if the variable is **not** empty;
- `-z $<var>` — True if the variable **is** empty;
## Keywords
### Input

- `read <var>` — Reads the user's input and saves it into `var`;
	- Can be then used or assigned to a different variable by using `$` before the variable's name;
### Debug
#### Script Options With `set`

- `set ` — Sets options for the script:
	- `-x` — Enabled debug mode where the script prints out every command before its execution;
	- `-e` — Exit immediately when any command in the script fails;
#### Command Error Handling

The `$?` variable stores the exit code of the last command executed.

_A value of `0` indicates success, while any other value indicates an error._

Check the exit code of the most recent command:

```bash
if [ $? -ne 0 ]; then
	# Do something
fi
```