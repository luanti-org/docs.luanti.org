---
title: Translating Mods and Games
aliases:
  - /Translation/Mods_and_Games
  - /translation_mods_and_games
  - /translation/mods-and-games
---

This page explains how to translate mods and games for Luanti. To learn how Luanti itself (the engine) is translated, see [Translation](/for-creators/translation).

## Before you begin

Sometimes, games and mods give an overview on about how it can be translated in the README file. This file may contain valuable information on how to get started.

Apart from that, games and mods generally share the same rules when it comes to translation.

## Overview

Mods can be translated in two file formats: The recommended format is ".po" and comes from Gettext and is supported from Luanti 5.10.0 onwards. This is the same file format as used by the engine. It is recommended because this format is well-developed and numerous tools support it.

The other format is ".tr" and is the old custom file format for Luanti that is less robust and has less features. It is also not well supported by tools. New games and mods should avoid it, it is now only supported for compatibility reasons.

## For translators

To translate a mod, you first need to know if it can even be translated. Check if the following is true:

- The directory `locale` exists **and**
- The file `template.txt` or a `*.pot` file exists in the `locale` directory.

If both conditions are true, the mod is ready for translation. Otherwise, it is not. In that case, ask the project maintainers to add translation support **and** the template file.

If you find the file "template.txt", this mod uses the old ".tr" format. If you find a "\*.pot" file, the mod uses the gettext PO format.

### PO format

The PO format is used by gettext and very widespread in free software. Explaining this format in detail would be too much for this page, it has been described in numerous Internet tutorials already. The file format itself is [explained here](https://www.gnu.org/software/gettext/manual/html_node/PO-Files.html).

Here’s just a quick summary on how to use it:

- A popular tool to edit PO files is Poedit
- To **create** a new translation, use Poedit on the ".pot" file
- To **edit** an existing translation, use Poedit on the file named `<name>.<language>.po`, e.g. "example.de.po" for a German translation

### TR format

To translate a mod (or update an existing translation), that uses the TR format, follow these steps:

1.  Check if a file named `[MODNAME].[LANG_CODE].tr` exists in `locale/`. If yes, skip to step 3. Otherwise, go to step 2.
2.  Copy the provided `template.txt` to a file named `[MODNAME].[LANG_CODE].tr`
3.  Open the `.tr` file in a text editor
4.  You will encounter a list of texts that end in equals signs. Put the translation of each text after the equals sign (note the special rules below)
5.  Save the file

(Note: `[MODNAME]` is the name of the mod and `[LANG_CODE]` is the language code)

Done. Now you probably want to test this translation in-game and submit your changes to the mod/game maintainers.

To translate an entire game, you translate all the mods within that game.

#### TR format rules

Here are the detailed rules of how the ".tr" file is structured.

- The lines that matter for you are all non-empty lines that do not start with "#". These are the translation lines. On the left side of the equals sign, the original text is written. On the right side of the equals sign, the translation has to be written. If the translation is empty, Luanti will display the original text instead.
- Do not change lines that start with "# textdomain:". These are needed for technical reasons.
- You can add as many empty lines you want. They will be ignored
- Other lines that start with "#" are comments. Luanti will ignore them but they might contain some information for you. It is up to you whether you keep or remove these lines.

In the texts (both original and translated texts), some characters will be replaced in the actual game:

- "@1", "@2", ... to "@9" are placeholders. The game will replace it with another text. You **MUST** include all placeholders in the translation, but the order can be different.
- To produce the "@" character, you **MUST** write "@@"
- To produce the "=" character, you **MUST** write "@="
- To produce a newline, you **MUST** write "@n". Alternatively, you can also write "@" followed by an actual newline

### Example

Consider the example mod `example`. You will find the file `template.txt` with this content:

{{% comment %}} cspell:disable {{% /comment %}}

```
# textdomain:example
Apple=
Pickaxe=
#This is a comment, not a translation. You can ignore this line.
Welcome, @1!=
@1 has killed @2 and gained @3 EXP.=
E-mail: <somebody@@example.org>=
“@=” is the equals sign=
This text@nhas 2 lines.=Dieser Text@nhat 2 Zeilen.
```

{{% comment %}} cspell:enable {{% /comment %}}

A possible translation in German would be stored under `example.de.tr` with this content:

{{% comment %}} cspell:disable {{% /comment %}}

```
# textdomain:example
Apple=Apfel
Pickaxe=Spitzhacke
#This is a comment, not a translation. You can ignore this line.
Welcome, @1!=Willkommen, @1!
@1 has killed @2 and gained @3 EXP.=@1 hat @2 getötet und @3 EXP erhalten.
E-mail: <somebody@@example.org>=E-Mail: <somebody@@example.org>
“@=” is the equals sign.=»@=« ist das Gleichheitszeichen.
This text@nhas 2 lines.=Dieser Text@nhat 2 Zeilen.
```

{{% comment %}} cspell:enable {{% /comment %}}

For reference, this is how the English texts could _actually_ show up in Luanti (with the placeholders resolved):

- Apple
- Pickaxe
- Welcome, Merlin!
  {{% comment %}} cspell:disable {{% /comment %}}
- Gnerf has killed Hormel and gained 500 EXP.
  {{% comment %}} cspell:enable {{% /comment %}}
- E-mail: <somebody@example.org>
- “=” is the equals sign
- This text

has 2 lines

## For developers

As a mod or game developer, if you want to implement translation support, please refer to the official Lua API documentation (`lua_api.md`) and [the modding book chapter on Translation](https://rubenwardy.com/minetest_modding_book/en/quality/translations.html).

We highly recommend you use PO instead of TR, if you can.

If you’re stuck with TR, the [modtools](https://github.com/luanti-org/modtools) contain a an extremely useful tool that can automatically generate and update TR files for mods.

### Online translation

It is very useful for translators to be able to translate your thing in the browser.

[Translating a game on Weblate](/for-creators/translation/weblate) is an useful guide on how to set up your game on a Weblate instance for allowing translation in the browser.
