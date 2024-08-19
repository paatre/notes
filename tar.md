# GNU tar: an archiver tool

`tar` can be used to work with *archives*. It is mostly used with file-based archives even though it's possible to also work with devices such as tapes etc. This file introduces information for only file-based archives. Archives created with `tar` also also called *tarballs*.

## Use cases

- Pack multiple files into a simple archive file which can be easily sent to another person
- Create a software release to internet for people to easily download


## Operations

These **operations** define what kind of an action is being taken with `tar`:

- `--create`, `-c`: Create an archive
- `--list`, `-t`: List the contents of an archive
- `--extract`, `--get`, `-x`: Extract files from an archive

## Options

Here are some frequently used **options** which instructs how to run operations more specifically:

- `--file=archive-name`, `-f archive-name`: Specify the name of an archive file
- `--help`, `-?`: Shows the help listing
- `--verbose`, `-v`: Shows details about the results of running `tar` similar to `ls -1` or `ls -l` depending on the operation and verbosity level

Then there are other options as well which might not be used that often.

### Options regarding running `tar`

- `--show-defaults`: Show built-in defaults for `tar`
- `--usage`: Display a list of available options without descriptive texts, stripped down version of `tar --help`

### Options for `tar` output and checking progress

- `--block-number`, `-R`: Show block number within the archive where the message was triggered
- `--checkpoint[=N]`: Display progress messages every `N`th record, default is 10 when `N` is not provided
- `--checkpoint-action=ACTION`: Run `ACTION` on each checkpoint, use together with `--checkpoint` (read [Possible actions for `--checkpoint-action`](#possible-actions-for---checkpoint-action))
- `--index-file=FILE`: Send verbose output to `FILE`
- `--show-omitted-dirs`: Lists each directory that does not match search criteria during listing or extracting
- `--totals`: Print total amount of bytes transferred after processing the archive

#### Possible actions for `--checkpoint-action`

- `bell`: Produce an audible bell sound
- `dot`: Print a single dot
- `echo=string`: Display a custom message
- `exec=command`: Execute the given command 
- `sleep=time`: Wait for `time` seconds
- `ttyout`= Output string on the current console (`/dev/tty`)
- `totals`: Print statistics on the number of bytes transferred
- `wait=signo`: Wait for signal `signo` 

## How to create an archive

```bash
$ tar -cf collection.tar blues folk jazz
```

## How to list contents of an archive

```bash
$ tar -tf collection.tar
blues
folk
jazz
```

## How to extract members from an archive

```bash
$ ls
collection.tar
$ tar -xf collection.tar
$ ls
blues collection.tar folk jazz
```

```bash
$ ls
collection.tar
$ tar -xf collection.tar blues
$ ls
blues collection.tar
```

**Note**: What should be aware of when extracting from an archive is that if there are files in the pre-existing directory with the same names as the members which you extract, the files from the extracted archive will replace the files already in the working directory (and possible subdirectories). This will happen regardless of whether or not the files in the working directory were more recent than those extracted. Create a new directory and extract files there to mitigate any overriding if you're unsure of the consequences.

## My `tar` version

```bash
$ tar --version
tar (GNU tar) 1.35
Copyright (C) 2023 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Written by John Gilmore and Jay Fenlason.
```
