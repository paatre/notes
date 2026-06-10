# Linux commands

## GNU

### Findutils

Findutils is a set of utilities for finding files on a system and performing actions on them.
Findutils consists of the following utilities:
- `find`
- `locate`
- `updatedb`
- `xargs`

Here's an example from the GNU Findutils manual on how to use `find` and `xargs` together:

> Here is an example operation to make all HTML files in the subdirectory _htdocs_ readable by all using find and xargs.
This is a typical example of how find and xargs are used with other utilities to provide powerful directory traversal capability.
> 
> ```bash
> find htdocs -name '*.html' -print0 | xargs -0 chmod a+r 
> ```

#### find

##### Synopsis

```bash
find [-H] [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]
```

First `-H`, `-L` and `-P` options are mutually exclusive, the last one specified will be used, and they
specify how to handle symbolic links.

`-D` is used for debugging purposes.

`-O` is used to specify the optimization level.

Specify one or more `starting-point` directories to specify the where to search in.

`expression` is a list of tests and actions to perform on the files found.

##### Excluding directories

To exclude directories from the search, use the `-prune` action.

For example, to search for all `.txt` files in the current directory and its subdirectories, excluding `.git` directories:

```bash
find . -name .git -prune -o -name '*.txt' -print
```

To exclude multiple directories, use multiple `-prune` actions:

```bash
find . \( -name .git -o -name .venv \) -prune -o -name '*.txt' -print
```

We could also use the `!|-not -path` test to exclude directories like this:

```bash
find . -type d ! -path '*/.git/*' ! -path '*/.svn/*' -o -name '*.txt' -print
```

The problem with using `!` or `-not` instead of `-prune` is that `!` and `-not`
with `-path` will traverse all the directories and will test the path to the given
directory name while `-prune` will stop traversing the directory as soon as it finds
the directory name. So, using `-prune` is more efficient.

#### locate

TBW

#### updatedb

TBW

#### xaargs

TBW
