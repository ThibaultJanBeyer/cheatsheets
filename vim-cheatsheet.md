## Save and Exit

Write and quit:

```vim
:wq
```

Quit without saving:

```vim
:q!
```

## Search and replace

The :substitute command searches for a text pattern, and replaces it with a text string. There are many options, but these are what you probably want:  

```bash
:%s/foo/bar/g
# Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.
:s/foo/bar/g
# Find each occurrence of 'foo' (in the current line only), and replace it with 'bar'.
:%s/foo/bar/gc
# Change each 'foo' to 'bar', but ask for confirmation first.
:%s/\<foo\>/bar/gc
# Change only whole words exactly matching 'foo' to 'bar'; ask for confirmation.
:%s/foo/bar/gci
# Change each 'foo' (case insensitive due to the i flag) to 'bar'; ask for confirmation.
:%s/foo\c/bar/gc is the same because \c makes the search case insensitive.
# This may be wanted after using :set noignorecase to make searches case sensitive (the default).
:%s/foo/bar/gcI
# Change each 'foo' (case sensitive due to the I flag) to 'bar'; ask for confirmation.
:%s/foo\C/bar/gc is the same because \C makes the search case sensitive.
# This may be wanted after using :set ignorecase to make searches case insensitive.
```
