# Vim

## Folding

A new fold can be created with `zf`. A fold can be opened and closed with `zo` and `zc`, respectively, but this affects only one level of fold. A count can also be given to these command which opens and closes that many levels of fold. Using the capital letter version of the commands, `zO` and `zF`, opens and closes all fold levels. `za` works as a toggle for the fold and `zA` is a recursive version of the toggle.

## Filetypes

Vim can detect the type of file that is being edited. You can also enable plugin and indentation files for detected file types. File types are also used for syntax highlighting, which is enabled via `:syntax on` and it enables file type detection as well. Plugin and indent files can be found from `$VIMRUNTIME/plugin/` and `$VIMRUNTIME/indent/`.

To enable file type detection, use this command in your vimrc: 

```vim
:filetype on
```

You can enable loading the plugin files for specific file types with:

```vim
:filetype plugin on
```

You can enable loading the indent file for specific file types with: 

```vim
:filetype indent on
```

### Filetype plugings

#### Markdown

Markdown has its own folding plugin that works with the heading levels.

To enable folding per heading in Markdown, use the following setting in vimrc:

```vim
let g:markdown_folding = 1
```
