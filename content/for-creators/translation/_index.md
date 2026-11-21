---
title: Translation
aliases:
  - /Translating
  - /Translation
  - /translation
---

This page explains how to translate Luanti.

## Translating mods and games

To learn how Luanti mods and games are translated, go to [Translating Mods and Games](/for-creators/translation/mods-and-games).

## Translating the engine

Translations of the Luanti engine are [automated using Weblate](https://hosted.weblate.org/projects/minetest/minetest/). Register an account, select the “Minetest” project (or “Luanti” if it has been renamed) and start translating. Translation is a continuous effort and the texts will change almost at every version.

The developers usually will update the translation templates shortly before a new release. If you translate a long time before a release, your translation updates will still be included, but your translation will likely be incomplete because almost every new version comes with new things to translate. Check out the [News forum](https://forum.luanti.org/viewforum.php?f=18) to see if a new release is imminent.

In case you wish to translate a new language, send a message in #minetest-dev and a coredev can add it.

### Language-specific notes

Here is a list of wiki pages for translating specific languages:

- [German](/for-creators/translation/de)

### Special strings

In the translations you will find one string with a special meaning. It must **not** be translated literally. They are used internally by Luanti to trigger certain settings. You must enter a special value into the translation field.

- `LANG_CODE`: The language code of the language you're translating (e.g. "de" for German).

### Builtin

Builtin is another core part of Luanti which includes texts for server commands, privilege descriptions, and many other server-related messages. Builtin is translated with a different method, you can't use Weblate here. Builtin is translated like mods (read the section below to learn how) and the translation files work the same like for the mod translation files. You can find the builtin translation files in the Luanti source directory under `builtin/locale`.

For example, if you want to translate Builtin into German, you need to edit `builtin/locale/__builtin.de.tr`.

### Overview

For most strings, Luanti uses the **gettext** library to translate the texts in Luanti. To enable internationalization support, the game must be built with the `-DENABLE_GETTEXT=1` option to `cmake`, which is enabled by default.

Games, mods and builtin are translated using a different method (see below).

### Language detection

Luanti detects the current language by inspecting the `LANG` environment variable. This is not a problem on Unix-based systems (such as GNU/Linux) since the system already defines this variable at login. On Windows platforms, the system language is used.

### Available translations

The available translations are found in source form in the `po/` directory. The `cmake` detects them, and they are built as part of the build process.

The main translation file must be updated now and then using the [this script](https://github.com/luanti-org/luanti/blob/master/builtin/mainmenu/settings/generate_from_settingtypes.lua) (configuration) and [this one](https://github.com/luanti-org/luanti/blob/master/util/updatepo.sh) (C++ and Lua translations). Submit a PR after running the generator or poke a core dev to update the translations when it's needed. Note that builtin translations are handled separately, see the maintenance notes below.

## Maintaining engine translations

There are two types of translations that need maintenance: Client-side (using Gettext) and server-side translations for builtin (using `minetest.get_translator`).

### Client-side translations (Gettext)

#### Contributing a new translation

To create a new translation, one must create a directory named after the [language code](http://www.mathguide.de/info/tools/languagecode.html), creating a copy of the `po/minetest.pot` file as `po/_LANG_/minetest.po`, and translating the original english text into your language; then `cmake` will detect it and `make` builds the language.

However, it is recommended to contact a core dev to create the .po file for you language and then use [weblate](https://hosted.weblate.org/projects/minetest/minetest/) to translate.

_Note to coredevs_: Creating a new language directly from weblate is sufficient, no need to mess with the files directly in the repository.

#### How to merge translations from Hosted Weblate

Translations should be merged in bulk, and not too often, to not create too large "noise" in the commit log.
A good schedule is once every few months and at the start of the **feature freeze**.
This section explains the necessary steps for core devs.
You will need admin access to the Weblate project in order to do this.

As of January 2025, the list contains the following people:

- celeron55
- grorp
- Krock
- nerzhul
- rubenwardy
- sfan5
- shadowninja
- Warr1024

For an up to date list consult someone with access to look at the people page in Weblate.

Add weblate as remote: `git remote add weblate https://hosted.weblate.org/git/minetest/minetest/`

1.  Visit [Repo maintenance](https://hosted.weblate.org/projects/minetest/minetest/#repository), and lock the repository to prevent changes from users while you are editing.
2.  Generate a clean history, without merge commits. Push the "Rebase" button on the repository.
3.  Do `git remote update weblate`. Confirm e.g. with `git log --graph weblate/master` that it bases on upstream's master, and only has "Translated using Weblate" as additional commits, no merge commits.
4.  As every weblate user can freely edit translations, there can be vandalism. Therefore, check the translation commits, e.g. with online translator services like Google Translate, other core devs, or trusted members from the community. Of course, its up to you to how much you want to follow this rule, as checking changes can be quite time consuming. Feel free to refine your scope to new and not yet trusted contributors.
5.  Check out the branch from Weblate's repo: `git checkout weblate/master`
6.  Reorder the commits from the same author, and squash them by running `git rebase -i master`. From a separate terminal run `./util/reorder_translation_commits.py` and then continue the rebase (without re-saving the file).
7.  Confirm that `git diff weblate/master` is empty, to make sure that you didn't mess up at step 6. Otherwise use `git reflog` to find the latest rebase pass that worked, and retry.
8.  If _required/desired_, do these: (use `--author="updatepo.sh <script@mt>"` when committing changes made by the scripts)
    1.  Update `minetest.conf.example` and the dummy `*.cpp` translation file and commit. Do this by uncommenting the lines at the end of `builtin/common/settings/init.lua`.
    2.  Run `util/updatepo.sh`, and commit. Note that it creates lots of unnecessary changes, and enlarges repository size disproportionately, therefore run it even less often.
9.  Push to the [GitHub repo](https://github.com/luanti-org/luanti) with `git push origin HEAD:master`. If you want to check if CI passes first you can also create a temporary branch with `git push origin HEAD:refs/heads/ci`. 
10. Reset the Weblate remote ("Reset" button), rebase it to match now current master, and unlock it. If pushed commits do not yet show up in Weblate you may have to press the "Pull" button.

Note: For the rebasing script to work you need to set git-rebase up to show the author, like so: `git config --add rebase.instructionFormat "(%an <%ae>) %s"`
(source: [Stack Overflow](http://stackoverflow.com/a/35851846))

Completing step 8 is **required** at least once per development cycle before a release. Typically this happens at the start of a feature freeze.

### Server-side builtin translations (`minetest.get_translator`)

The server-side translations of builtin are located in `builtin/locale` with `template.txt` being used as the template file.

The template file as well as all locale files need to be updated before release to allow translators to translate.

To update the builtin locale files, run `../util/mod_translation_updater.py` from the builtin directory (requires Python 3).

To start a new translation, copy `template.txt` to create `__builtin.<LANGUAGE_CODE>.tr`.

## Untranslatable texts

Please note: A couple of things in Luanti can **not** be translated yet:

- Setting help texts for settings of games and mods (`settingtypes.txt`) ([#9070](https://github.com/luanti-org/luanti/issues/9070))
- Drop-down list entries in Luanti settings
- Description of mods, modpacks, games and texture packs ([#9071](https://github.com/luanti-org/luanti/issues/9071))
