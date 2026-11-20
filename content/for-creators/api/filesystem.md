---
title: Filesystem
aliases:
  - /api/filesystem
---

# Filesystem

Luanti provides various utility functions to help with managing paths, files & directories.

> [!IMPORTANT]
> Mod security restricts file system access to the mod path (at load time) and the world path later on in secure environments, see [Lua Environment](/Lua_Environment/).

## Paths

> [!WARNING]
> Always operate on absolute paths, never on relative ones; no assumption about the current working directory can be made.

### Normalization

Luanti normalizes paths you pass into filesystem-related functions, replacing `/` with the platform-specific path delimiter (usually `\` on Windows, `/` on UNIX).

Using `/` in paths is thus always fine. However, paths returned by Luanti are usually not normalized.

### `DIR_DELIM`

Global variable which holds the path delimiter as a `{type-string}` (usually `\` on Windows, `/` on UNIX).

Usually not necessary due to path normalization (perhaps only necessary when parsing paths),
but may be used "for good measure" (user-friendly paths using the platform-specific delimiters).

### `core.get_modpath`

Gets the path of a mod, _if it is loaded_. If the mod isn't loaded, it will return nil.

**Arguments:**

- `modname` - `{type-string}`: The mod name

> [!TIP]
> Use `local modpath = core.get_modpath(core.get_current_modname())` rather than hardcoding your modname.

**Returns:**

- `modpath` - `{type-string}`: The absolute, non-normalized path to the mod directory

### `core.get_worldpath`

No arguments.

**Returns:**

- `worldpath` - `{type-string}`: The absolute, non-normalized path to the world directory

## Directories

**Common Returns**

- `success` - `{type-bool}`: Whether the operation succeeded.

> [!TIP]
> Use `assert` to error on failure.

### `core.mkdir`

Creates a directory and nonexistent parent directories.

**Arguments:**

- `path` - `{type-string}`: Path to the directory to create

### `core.cpdir`

Copies a source directory to a destination.

**Arguments:**

- `source_path` - `{type-string}`: Path to the source directory to copy
- `destination_path` - `{type-string}`: Path to the destination directory

> [!WARNING]
> If the destination directory exists already, it will be overwritten.

### `core.mvdir`

Moves a source directory to a destination.

If the destination is a non-empty directory, the move will fail.

**Arguments:**

- `source_path` - `{type-string}`: Path to the source directory to copy
- `destination_path` - `{type-string}`: Path to the destination directory

### `core.rmdir`

Removes a directory.

**Arguments:**

- `path` - `{type-string}`: Path to the directory to remove.
- `recursive` - `{type-bool}`: Whether to recursively delete the directory's content; if this is not set to `true`, removal will fail if the directory is nonempty

### `core.get_dir_list`

Lists direct directory contents.

**Arguments:**

- `path` - `{type-string}`: Path to the directory to remove.
- `is_dir` - `{type-nil}` or `{type-bool}`: Whether to list:
  - `nil`: Both directories and files
  - `true`: Only directories
  - `false`: Only files

**Returns:**

- `entry_list` - list of `{type-string}`: List of entry names - **not paths!**; you should make no assumptions concerning the order of this list.

## Files

### `core.safe_file_write`

Performs an _atomic binary file write_: Either all or nothing is overwritten. This is done by creating a temporary file, writing to it, then swapping with the temporary file.

Use this function to prevent corruption of save files.

Using `core.safe_file_write(path, content)` would be equivalent to

```lua
local f = io.open(path, "wb")
f:write(content)
f:close()
```

if this code block was atomic.

**Arguments:**

- `path` - `{type-string}`: Path to the file to write.
- `content` - `{type-string}`: The content to write.

**Returns:**

- `success` - `{type-bool}`: Whether the operation succeeded.

### Lua builtins

All properly overridden by Luanti to apply mod security restrictions.

- [`io.open(filename, mode)`](https://www.lua.org/manual/5.1/manual.html#pdf-io.open): Open a file for reading or writing.

> [!IMPORTANT]
> Use `rb` and `wb` rather than `r` and `w` for working with binary files, otherwise your code will break on Windows.

- [`io.lines([filename])`](https://www.lua.org/manual/5.1/manual.html#pdf-io.lines]): Get an iterator over the lines of a file.
- [`io.tmpfile()`](https://www.lua.org/manual/5.1/manual.html#pdf-io.tmpfile): Get a handle for a temporary file.
- [`os.tmpname()`](https://www.lua.org/manual/5.1/manual.html#pdf-os.tmpname): Get a name for a temporary file.
- [`os.remove(filename)`](https://www.lua.org/manual/5.1/manual.html#pdf-os.remove): Removes an entry (file or empty directory)
- [`os.rename(oldname, newname)`](https://www.lua.org/manual/5.1/manual.html#pdf-os.rename): Moves/renames an entry (file or directory)
