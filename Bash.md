# Built-In Commands

- `cd` — Change the directory to a different location;
- `ls` — List the contents of the current directory;
- `mkdir` — Create a new directory;
- `rmdir` — Removes a directory if it's empty;
- `touch` — Create a new file;
- `rm` — Remove a file or directory;
- `cp` — Copy a file or directory;
- `mv` — Move or rename a file or directory;
- `echo` — Print text to the terminal;
- `cat` — Concatenate and print the contents of a file;
- `grep` — Search for a pattern in a file;
- `chmod` — Change the permissions of a file or directory;
- `sudo` — Run a command with administrative privileges;
- `df` — Display the amount of disk space available;
- `history` — Show a list of previously executed commands;
- `ps` — Display information about running processes;
- `which` — Locates a program's binary in the user's `$PATH`. Takes a command as its agrument;

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
	do <statement>
fi
```

### Logical Operators

- `-a` — AND;
- `-o` — OR;
- `-gt` — Greater than;
- `-lt` — Less than;

Example:

```bash
if [[ $a -gt 60 -a $b -lt 100 ]]
```

### Variables

-  `-n $<var>` — True if the variable is **not** empty;
- `-z $<var>` — True if the variable **is** empty;

## Keywords

### Input

- `read <variable>` — Reads the user's input and saves it into `variable`;
	- Can be then used or assigned to a different variable by using `$` before the variable's name;

### Debug

#### Script Options With `set`

- `set ` — Sets options for the script:
	- `-x` — Enabled debug mode where the script prints out every command before its execution;
	- `-e` — Exit immediately when any command in the script fails;

#### Command Error Handling

The `$?` variable stores the exit code of the last command executed.

_A value of `0` indicates success, while any other value indicates an error.

Check the exit code of the most recent command:

```bash
if [ $? -ne 0 ]; then
	# Do something
fi
```