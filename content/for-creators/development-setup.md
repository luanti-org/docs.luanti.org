---
title: Development Setup
aliases:
  - /Development_Setup
  - /development-setup
  - /content-dev/development-setup
---

# Development Setup

## Introduction

You can use any text editor to write mods,
but it is recommended to pick an editor
that supports the Language Server Protocol (LSP).
The LSP provides autocompletions and diagnostics
to aid you in development.

Examples of Editors that support the LSP are:

- [Visual Studio Code](https://code.visualstudio.com/)
- [Kate](https://apps.kde.org/kate/)
- [NeoVim](https://neovim.io/)
- [Helix](https://helix-editor.com/)

## Installing the Language Server

How you install the language server depends on the editor and operating system you use.

For VS Code, install the [Lua Plugin](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)
which bundles the Language Server.

If you use NeoVim, you may install the Language Server via mason or another
LSP plugin.

If your editor does not bundle the language server, you need to
[install it](https://luals.github.io/#install) on the operating system level.

## Configuring the Language Server

### Downloading the type documentation

The type documentation allows autocompletion
including type hints to be shown in your editor.

We will use [Luanti API](https://git.minetest.land/archie/luanti-api/)
as a source of type documentation.

There are two ways to setup autocompletion with this.

### 1. Globally

Download the repository "luanti-api" and put it somewhere on
your PC. Find guidance on how to download the repository as
a zip archive or clone it via git.

Let's suppose luanti-api is now at `/home/user/luanti-api`.

Lastly, you need to configure the Lua language server. For
VS Code, it's done via settings.json. Look up the respective
guide for your editor on how to configure the language server.

To make the language server pick up the type documentation,
set `workspace.library` to the path you downloaded the
type documentation to. In our example, it is `/home/user/luanti-api`.

For VS Code, you would put add this to settings.json (User):

```json
{
  "Lua.workspace.library": ["/home/user/luanti-api"]
}
```

### 2. Per project

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
git submodule add https://git.minetest.land/archie/luanti-api.git
```

The final step is to configure the language server.
To do this per project, we will add a `.luarc.json`
file to the project root:

```json
{
  "$schema": "https://raw.githubusercontent.com/LuaLS/vscode-lua/master/setting/schema.json",
  "workspace.library": ["./luanti-api"]
}
```
