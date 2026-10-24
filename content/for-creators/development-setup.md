---
title: Development Setup
---

# Development Setup

## Introduction

You can use any text editor to write mods,
but it is recommended to pick an editor
that supports the [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/).
The LSP provides auto completions and diagnostics
to aid you in development.

Examples of Editors that support the LSP are:

- [Visual Studio Code](https://code.visualstudio.com/)
- [Kate](https://apps.kde.org/kate/)
- [NeoVim](https://neovim.io/)
- [Helix](https://helix-editor.com/)

## Quick start for VS Code

If you use VS Code or one of its forks (VSCodium, Cursor, Windsurf, Theia, ...),
the quickest way to get autocompletion and diagnostics is the "Luanti API" extension
([VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=Voxrame.luanti-ide-helper),
[Open VSX Registry](https://open-vsx.org/extension/Voxrame/luanti-ide-helper)).
It installs the [EmmyLua](https://marketplace.visualstudio.com/items?itemName=tangzx.emmylua) language server
and the Luanti type documentation for you, so no further setup is needed.
Please note that this is not an official project.

If you use another editor or prefer to set everything up manually, follow the steps below.

## Installing the Language Server

How you install the language server depends on the editor and operating system you use.

For VS Code, install the [Lua Plugin](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) or [EmmyLua](https://marketplace.visualstudio.com/items?itemName=tangzx.emmylua),
which bundles the Language Server.

If you use NeoVim, you may install the Language Server via mason or another
LSP plugin.

If your editor does not bundle the language server, you need to
[install it](https://luals.github.io/#install) on the operating system level.

## Setting up autocompletion

### Downloading the type documentation

The type documentation allows autocompletion
including type hints to be shown in your editor.

You can use one of the following sources of type documentation:
- [Voxrame/luanti-ide-helper](https://github.com/Voxrame/luanti-ide-helper):
  works with both the [Lua LS](https://marketplace.visualstudio.com/items?itemName=sumneko.lua) and [EmmyLua](https://marketplace.visualstudio.com/items?itemName=tangzx.emmylua) (EmmyLua is much faster)
- [andro/luanti-api](https://git.minetest.land/andro/luanti-api): experimental

Please note that these are not official projects.
For more context, you can take a look at [this issue](https://github.com/luanti-org/luanti/issues/13926).

If you would not like
to proceed with downloading either
repository, you can adapt the below instructions
for the path `<Luanti installation>/builtin` to
get untyped completions.

There are two ways to setup autocompletion.

### Option 1: Globally

Download the repository "luanti-ide-helper" or "luanti-api" and put it somewhere on
your PC. Find guidance on how to download the repository as
a zip archive or clone it via git.

Let's suppose you downloaded it to `/home/user/luanti-types`.

Lastly, you need to configure the Lua language server. For
VS Code, it's done via settings.json. Look up the respective
guide for your editor on how to configure the language server.

To make the language server pick up the type documentation,
set `workspace.library` to the path you downloaded the
type documentation to. In our example, it is `/home/user/luanti-types`.

For VS Code, you would put add this to settings.json (User):

```jsonc
// for LuaLs:
{
  "Lua.workspace.library": ["/home/user/luanti-types"]
}
// for EmmyLua:
{
  "emmylua.workspace.library": ["/home/user/luanti-types"]
}
```

### Option 2: Per project

You can configure the autocompletion per project. First, you
need to add the type documentation as a git submodule.
You could also download the repository files and put them
into the project root if you don't want to use git submodules,
but that is bad practice.

1. Initialize a git repository

```bash
git init
```

2. Setup submodules

```bash
git submodule init
```

3. Add the submodule

```bash
# luanti-ide-helper:
git submodule add https://github.com/Voxrame/luanti-ide-helper.git
# or luanti-api:
git submodule add https://git.minetest.land/archie/luanti-api.git
```

The final step is to configure the language server.
To do this per project, we will add a config file to the project root.

For LuaLS, add `.luarc.json`:

```json
{
  "$schema": "https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json",
  "workspace.library": ["./luanti-types"]
}
```

For EmmyLua, add `.emmyrc.json`:

```json
{
  "$schema": "https://raw.githubusercontent.com/EmmyLuaLs/emmylua-analyzer-rust/refs/heads/main/crates/emmylua_code_analysis/resources/schema.json",
  "workspace": {
    "library": ["./luanti-types"]
  }
}
```
