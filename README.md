# Notes stash

These are my private (at least for now) notes that I'll keep on growing to understand the stuff I'm trying to learn. I want to write things on these notes so that it makes me remember them better and to come back to them later. Future me says "thanks!"

## Style guide

These notes are done in Markdown. Don't cut down long paragraphs to support proper automatic resizing of the paragraphs. This is why `textwidth` setting isn't set in my `~/.vimrc`:

```vim
augroup vimrcEx
  au!
  autocmd FileType * if &filetype != 'markdown' | setlocal textwidth=78 | endif
augroup END
```

Always, I mean ALWAYS, use proper heading hierarchy: h1 > h2 > h3 > ...

Use stars (\*) for normal text formatting (*italic* and **bold**).

Use dashes (\-) for list items.

---
&copy; 2023 Teemu Viikeri
