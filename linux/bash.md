# Bash

*Bash* is an acronym for *Bourne-Again Shell*.

Bash is conformant of the IEEE POSIX Shell and Tools portion of the IEEE POSIX Specification (IEEE Standrad 1003.1).

A shell provides users with utilities and tools from the operating system such as listing files from file system, editing files or search utilities. A shell is also a programming language for using the shell provided utilities in combination and use them as new commands.

## Syntax

### Operation

Here's how Bash reads and executes a command on following steps:

1. Reads input.
2. Breaks input *tokens* separated by *metacharacters*, and performs alias expansion.
3. Parses tokens into commands.
4. Performs shell expansions.
5. Performs redirections.
6. Executes the command.
7. Waits for command completion and exit status.

### Quoting

Quotes remove the special meaning of characters or words to the shell. For example, `>` is a metacharacter for redirection but if it's quoted, `">"`, it's treated as a simple greater-than sign.

There are three types of quotings: escapes characters, single quotes and double quotes.

#### Singe quotes

Single quotes, `''`, are the most basic type of quoting characters. They preserve the literal value of each enclosing character. When single quotes are preceeded by `$`, Bash allows you to include backslash-escaped [ANSI C standardized characters](https://www.gnu.org/software/bash/manual/html_node/ANSI_002dC-Quoting.html).

#### Double quotes

Double quotes preserve the literal value of each enclosing character, with the exception of `$`, `` ` ``, `\ ` (when followed by `$`, ``` ``, `"`, `\ ` or `newline`), and `!` for history expansion (when not in POSIX mode). `*` and `@` have special meaning in the context of parameter expansion when in double quotes. When double quotes are preceeded by `$`, the quoted string is translated according to the current locale by `gettext`.

#### Backslash

A backslash, `\ `, is used as an escape character to preserve the literal value of a character. If the following character is `newline` (`\n`), the backslash needs to be quoted or it will be treated as a line continuation.

Properly quoted backslash would look like this:

```bash
printf "Hello\nWorld"
```

Line continuation would be something like this:

```bash
$ echo "foo" \
> "bar"
foo bar
```

### Comments

Comments starts with the comment symbol, `#`, and are ignored by the shell.

## Commands

### Simple commands

Most shell commands are *simple commands*. A simple command consists of multiple words separated by a space character and terminated by a control operator. The first word is generally the command itself and the rest are arguments to the command.

### Pipelines

A *pipeline* is a sequence of multiple commands separated by the control character `|` or `|&`.

Each command in the pipeline reads the previous command's standard output (stdout) as an input when a pipe character, `|`, is used between commands. Reading an output from a pipe is perfomed before redirections. For example, in `command1 > file.txt | command2`, the `command1` output is piped into `commands2` which ends up emptying the `stdout` for the redirection and the `file.txt` will not receive any data from `command1`.

When `|&` is used, standard error (stderr) is also sent to the next command as combined input stream.

`time` command can be used to measure a command's run time. It can also be used for pipelines.

### Lists

A *list* is a sequence of one or more pipelines separated by `;`, `&`, `&&`, or `||`.

- `;`: commands are executed sequentially
- `&`: command is executed in the background asynchronously
- `&&`: logical `AND` operator, can be used as a short-circuit operator
- `||`: logical `OR` operator, can be used as a short-circuit operator

#### Example

```bash
command1 && command2 || command3; command4
```

### Compound commands

Bash provides different progamming constructs to group and control the flow of commands. These are called *compound commands*.

#### Looping constructs

Bash supports three looping constructs (a `;` in the constuct syntaxes may be replaced with one or more newlines):

##### `until`

syntax: `until test-commands; do consequent-commands; done`
description: executes `consequent-commands` as long as `test-commands` has a non-zero exit status

```bash
#!/bin/bash

until git pull &> /dev/null
do
    echo "Waiting for the git host ..."
    sleep 1
done

echo -e "\nThe git repository is pulled."
```

##### `while`

syntax: `while test-commands; do consequent-commands; done`
description: executes `consequent-commands` as long as `test-commands` has a zero exit status

```bash
#!/bin/bash

x=1
while [ $x -le 5 ]
do
  echo "Welcome $x times"
  x=$(( $x + 1 ))
done
```

or

```bash
x=1; while  [ $x -le 5 ]; do echo "Welcome $x times" $(( x++ )); done
```

##### `for`
  - syntax: `for name [ [in [words …] ] ; ] do commands; done`
  - alternative syntax: `for (( expr1 ; expr2 ; expr3 )) ; do commands ; done`
  - description: expands `words` and executes `commands` for each iteration with `name` bound to the current iteration of `in words`, if `in words` isn't provided `commands` are executed for each positional parameters (as if `$@` was given)

```bash
#!/bin/bash

for i in {1..5}
do
   echo "Welcome $i times"
done
```

or

```bash
#!/bin/bash

for (( c=1; c<=5; c++ ))
do
   echo "Welcome $c times"
done
```

`break` and `continue` are also available in Bash.

#### Conditional constructs

##### `if`

###### Syntax

```bash
if test-commands; then
  consequent-commands;
[elif more-test-commands; then
  more-consequents;]
[else alternate-consequents;]
fi
```

###### Example

```bash
#!/bin/bash

if [ $1 -gt 100 ]
then
  echo Hey that\'s a large number.
  pwd
fi

date
```

##### `case`

`case` tries to match clause patterns to given word and executes clause's command(s).

###### Syntax

```bash
case word in
    [ [(] pattern [| pattern]…) command-list ;;]…
esac
```

`case` syntax has special characters:
- Pipe character separates patterns in a clause
- `)` operator terminates a pattern list
- `*)` works as the default pattern
- `;;` breaks execution after the first pattern match
- `;&` executes `command-list` of the next clause
- `;;&` executes `command-list` of the next clause if the pattern is matched

###### Example

```bash
echo -n "Enter the name of an animal: "
read ANIMAL
echo -n "The $ANIMAL has "
case $ANIMAL in
  horse | dog | cat) echo -n "four";;
  man | kangaroo ) echo -n "two";;
  *) echo -n "an unknown number of";;
esac
echo " legs."
```

##### `select`

`select` generates menus from given strings.

###### Syntax

```bash
select name [in words …]; do commands; done
```

Some additional notes:
- `PS3` is a prompt environment variable for `select` question
- The read line is saved to `REPLY` variable
- `select` completes on a `break` command

###### Example

```bash
select fname in *;
do
  echo you picked $fname \($REPLY\)
  break;
done
```

##### `(( ... ))`

`(( ... ))` is a construct for arithmetic evaluation. It allows arithmetic operations and arithmetic comparisons.

###### Syntax

```bash
(( expression ))
```

###### Example

```bash
count=0

while (( count < 5 )); do
    echo "Count is $count"
    (( count++ ))
done
```

##### `[[ ... ]]`

`[[ ... ]]` is a construct for conditional expression. It allows primaries, expansions and conditional comparisons. Usually used in an `if` statement.

###### Syntax

```bash
[[ expression ]]
```

Notes:
- Quoted (regardless of the quoting mechanism) parts of the expression or patterns are forced to match literally
* When `=~` is used for pattern matching, POSIX extended regular expressions (ERE) are used instead of shell pattern matching which is more limited compared to regular expressions

###### Example

```bash
filename="example.txt"

if [[ $filename == *.txt ]]; then
    echo "The file is a text file."
else
    echo "The file is not a text file."
fi
```

or

```bash
[[ $line =~ [[:space:]]*a?b ]]
```

#### Grouping commands

Bash has two ways to group a list: `( list )` and `{ list; }`. When a command list is grouped, redirections can be applied to the output of each command  on the list as one stream. The difference in these two ways is that the parenthesis create a new *subshell*, curly braces use the current shell context. Also, curly braces require the trailing semicolon.

### Coprocesses

Lorem ipsum.

## Variables

### `local` keyword

Declares a local variable for a scope such as function scope.

## Arguments

You can access arguments passed to a function or a script with `$1` or `${1}` where `1` represents the first passed argument. You need to use curly braces if the digit is not a single digit.

## Shell expansions

### Parameter expansions

- `${parameter:offset:length}`: substring expansion up to *length* characters of the value of `parameter` starting at the character specified by `offset`
- `${#parameter}`: length of the parameter

## Links

- Metacharacters: [https://www.gnu.org/software/bash/manual/html_node/Definitions.html#index-metacharacter](https://www.gnu.org/software/bash/manual/html_node/Definitions.html#index-metacharacter)
- Reserved words: [https://www.gnu.org/software/bash/manual/html_node/Reserved-Words.html](https://www.gnu.org/software/bash/manual/html_node/Reserved-Words.html)
- Control operators: [https://www.gnu.org/software/bash/manual/html_node/Definitions.html#index-control-operator](https://www.gnu.org/software/bash/manual/html_node/Definitions.html#index-control-operator)
- Conditional expressions: [https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)
