---
title: Releasing Luanti
aliases:
  - /Releasing_Luanti
  - /releasing-luanti
  - /engine-dev-process/releasing-luanti
---

# Releasing Luanti

The [feature freeze](#feature-freeze) and release date is set by core developers.

Historically, the Minetest release intervals were in between half and an entire year. To deliver bugfixes and features more quickly, the goal is to release more often.

 * Goal: January, April, July, October
 * A release before November should be prioritized because the Game Jam ([2024 forum post](https://forum.luanti.org/viewtopic.php?t=31059)) would like to make use of new features.

Patch releases are exempted from this schedule.

## Checklist

```
- Feature freeze
  - [ ] Announced
  - [ ] Release candidate advertised
  - [ ] Weblate sources regenerated and strings frozen
- Pre-release (day of release)
  - [ ] Autogenerate files
  - [ ] Correct special strings on Weblate (`LANG_CODE`)
  - [ ] Update translations from Weblate
  - [ ] Update changelog
  - [ ] Update credits in repo
  - [ ] Update credits on website
  - [ ] (Minor/Major) Write blog post
  - [ ] Ensure protocol version codes have been bumped
- Release
  - [ ] Run bump_version and verify
  - [ ] Push new tag
  - [ ] Tag luanti_android_deps repo
  - [ ] Build and upload Windows version
  - [ ] Build and upload Android version
  - [ ] Update Launchpad stable build (Ubuntu version)
  - [ ] (Minor/Major) Publish blog post
  - [ ] Create forum topic
  - [ ] Notify rubenwardy to announce on Twitter, etc.
  - [ ] Notify downstream maintainers
```

## Feature Freeze

### Announce a feature freeze

(_Skip for patch releases_)

New features aren't accepted in this time and people focus on finding and fixing bugs.

1. Move all open feature PRs of the current [milestone](https://github.com/luanti-org/luanti/milestones) to the next one.
2. Announce the **feature freeze in /topic of [#luanti-dev](/about/irc)**.
3. Post the release candidate builds on the forums ([News section](https://forum.luanti.org/viewforum.php?f=18)) and (optional) as pre-release on GitHub ([Releases page](https://github.com/luanti-org/luanti/releases)).
   * This helps to find high priority issues faster.
   * You may use the buildbot/CI artifacts from `master`. If needed: do provide manual signing instructions for the MacOS and Android builds.

### Autogenerate files

Also update the [translation](/for-creators/translation) templates:

- Engine: Regenerate Gettext files with `util/updatepo.sh`. Note that before that, you most likely want to [import existing changes](#Update_translations_from_Weblate) first.
- Builtin: Change directory to `builtin`, then run `util/mod_translation_updater.py` from there

Also ensure that the `language` setting enum values contains `en`: there is no "en" directory, but Luanti supports it.

Read [Translation](/for-creators/translation) for details.

### Update source strings on Weblate

Make sure that the source strings on hosted.weblate.org are up-to-date.

Do **not** just blindly run the scripts without checking. Check if the source strings on Weblate were _actually_ updated. A simple way to check is to look at any translation that was at 100% (on Weblate) _before_ the update. After the update, the percentage should drop because of new strings. If it didn't drop but you know there are new strings in Luanti, this means the update failed.

## Before releasing

### Verify special translation strings

The translation files contain a special string: [LANG_CODE](https://hosted.weblate.org/translate/minetest/minetest/en/?q=LANG_CODE&checksum=&offset=1#translations) (see [Translation](/for-creators/translation)).

Verify that all \*.po files have a valid value for these strings because translators frequently misunderstand them and enter an invalid value. Fix any invalid values on Weblate by either entering the correct one or by removing the bad translation.

### Update translations from Weblate

**How to do this** -> [How to merge translations from Hosted Weblate](/for-creators/translation/#how-to-merge-translations-from-hosted-weblate)

If doing a backported release, you can use the following command to cherry-pick all translation commits from weblate:

```
git log --reverse --pretty=format:"%h" $BASE..weblate/master -- po | xargs -L1 git cherry-pick
```

BASE is the commit on master to start from when looking for translation commits. This commit should be newer than the last translation commit on the backport-X branch.

### Update main menu credits

This consists of:

- Making use of `util/gather_git_credits.py`
  - edit the `REVS_ACTIVE` variable to contain the version number of two major versions in the past (e.g. 5.6.0 if you are releasing 5.8.0)
  - run the script
- Editing `builtin/mainmenu/credits.json` and putting the results there
- Don't forget to update the list of core developers if that changed

### Update website credits

Once the credits are decided on in the previous step, update the website to be in sync with the mainmenu. Simply copy the same `credits.json` to here: [https://github.com/luanti-org/luanti.github.io/tree/master/\_data](https://github.com/luanti-org/luanti.github.io/tree/master/_data)

### Update changelog

Update [the changelog](/about/changelog).

### Ensure protocol version codes have been bumped

If not a patch release: ensure that `PROTOCOL_VERSION` has been increased since the last release. ([deciding discussion](https://github.com/luanti-org/luanti/issues/11603))

If formspec features have been added: ensure `LATEST_FORMSPEC_VERSION` has been increased since the last release.

## The process

This is mostly done by several core developers.

### Update version in source

The process with patch releases is slightly different but the script will take care of it correctly in any case.

- **Define** a new version number by running `util/bump_version.sh`. Verify that this script correctly:
  - changes TRUE to FALSE for the line _set(DEVELOPMENT_BUILD TRUE)_ in CMakeLists.txt
  - updates the version number and release date in misc/net.minetest.minetest.metainfo.xml
  - and commits.
- **Tag** the version in local git (the script does this) to allow the CMake versioning script to remove the git hash from the version. Do not push the tag to GitHub yet.
- **Build**, run and check if the thing seems to be working.

### Build Windows version

As of [December 2023](https://github.com/luanti-org/luanti/pull/14098), we use the **mingw** artifacts of the "windows" CI workflow.

- Extract the outer ZIP file so that users only have one ZIP file to extract (should be named `luanti-5.x.x-win64.zip`)

Note that the correct build only shows up after the release commit has been pushed to Github.

→ → → _**Make sure that the Windows builds work before continuing to do anything**_ ← ← ←

#### Mini checklist of things to test

_Note_: Don't cheat on this by testing in Wine, it has happened that things crash/break in Wine while they are fine on real Windows.

- check that the build identifies itself as 5.x.x not 5.x.x-dev or 5.x.x-abc4de7
- click some menu buttons
- create world with MTG, enter it, exit back to menu
- open multiplayer tab, attempt to join a server
- install a package from CDB, uninstall it again
- enable dynamic shadows, join in-game and look

### Upload packages to somewhere

- All official builds are hosted at Github: [https://github.com/luanti-org/luanti/releases](https://github.com/luanti-org/luanti/releases)
- The Windows build is created by CI
  - (see above)
- The macOS build is built and signed by SFENCE
  - the file is named something like `luanti_5.11.0-macos11.3_arm64.zip`, which contains the minimum macOS version it can run on.
- Android APKs are also uploaded here when they're done
  - these are signed before uploading, see a few sections below
- There is a PPA for stable builds on Ubuntu (see below).

### Update branches and tags on GitHub

Tagging is handled by the script for the engine.

The new release should be merged to the stable-5 branch. **Its important to merge, and not just rebase**, so that git describe works.

#### The problem on the stable-5 branch

Usually, merging releases onto the stable branch just consists of adding the commits to the branch, as it contains direct ancestors of master commits, and git can do a fast forward. During release/freeze of 5.0.1 (both luanti and minetest_game), the ancestor rule has been broken.

Therefore, you'll generate merge commits, but this shouldn't be a problem. In the case of merge conflicts, ensure that the changes on stable-5 are all discarded in favor of the tagged commit at master, by doing a merge commit like:

```
   git checkout version-tag
   git merge -s ours origin/stable-5
   git push origin HEAD:stable-5

```

### Tag Android deps

Create a new tag [on this repo](https://github.com/luanti-org/luanti_android_deps/tags) with the version number of the release. This is to make it easier to figure out which state an APK was built from.

### Update Launchpad stable build to get Ubuntu builds for the new version

celeron55, rubenwardy, ShadowNinja, and luatic have access.

Process:

- Go to [minetest-c55/upstream](https://code.launchpad.net/~minetestdevs/minetest-c55/+git/upstream) and click _start import_.
- Now visit the [recipe](https://code.launchpad.net/~minetestdevs/+recipe/minetest-stable).
- At the bottom of the page there is a section called "Recipe contents". In this section you need to edit the recipe. Make sure you update:
  - The version number at the end of the first line. Doing this is a must otherwise there would be duplicate packages which would lead to a fail. The version number has a format like `5.11.0-ppa0`. You should keep the ppa postfix so that it's easy to differentiate the package by origin, ppa or upstream Debian.
  - The Git tag corresponding to the Luanti version (for example `5.11.0`) in the second line.
- Check whether everything has been updated correctly.
- Click the green "Request builds" link, enable the newer distro versions, and click confirm.

The build has two steps: first it assembles the source code and uploads it, then it builds the code. If the first step completed successfully but the second one failed, you need to update the version number in the recipe (e.g. 1.2.3-ppa1) before rebuilding.

### Build and publish Android APK

**nerzhul** or **rubenwardy** have access to Google Play. Both also hold the signature keys for the app.

#### Signing APKs for the Play Store

- Wait for CI to finish
- Download the .aab from GitHub CI
- Sign: `Android/Sdk/build-tools/34.0.0/apksigner sign --ks ~/Documents/keystore-minetest.jks --min-sdk-version 21 ~/Downloads/app-release.aab`
- Upload to Google Play

#### Creating Native Debug Symbols

**FIXME**: nobody does this anymore. what now?

- Copy native/build/intermediates/ndkBuild/release/obj/local/ to new folder
- Remove all but .so files from new folder
- Check the .so contain debug info using the file command
- `zip -r symbols.zip .`

## After releasing

### Reenable -dev version suffix

(_Skip for patch releases_) Check that the util/bump_version.sh script did the following steps:

- **Update** the version number in CMakeLists.txt and the titles of doc/client_lua_api.txt and doc/menu_lua_api.txt
- **Change** FALSE to TRUE for the line _set(DEVELOPMENT_BUILD FALSE)_ in CMakeLists.txt. This will add the -dev suffix to the version name again.
- **Commit.**

### Write a release notice

- Don't forget to edit the luanti.org page ([Repo](https://github.com/luanti-org/luanti-org.github.io)).
  - currently you will need to update machine readable metadata in \_data/release.yml and downloads.html itself.
- Post a new topic in the [News section](https://forum.luanti.org/viewforum.php?f=18) of the forum. **See [Changelog](/about/changelog).**
  - It is customary to sticky the newest release topic and lock older ones.
- Add a new post to the [blog](https://github.com/luanti-org/blog/)
- Announce the release on [Twitter](https://twitter.com/MinetestProject) and [Mastodon](https://mastodon.online/@Luanti@fosstodon.org). rubenwardy has access.

### Notify known package maintainers

- **Arch Linux**/Manjaro: can be flagged outdated on the [package page](https://archlinux.org/packages/extra/x86_64/luanti/)
- **Debian**/Ubuntu: has [own version tracking](https://tracker.debian.org/pkg/luanti), no need to contact
- **Fedora** and others: should automatically show up [here](https://release-monitoring.org/project/1978/), but can be flagged manually
- **F-Droid**: has volunteer maintainers, if nobody notices consider opening an issue [here](https://gitlab.com/fdroid/fdroiddata/-/issues)
- **Snap**: open an issue (or contribute) [here](https://github.com/snapcrafters/minetest)
- **Flatpak**: open an issue (or contribute) [here](https://github.com/flathub/net.minetest.Minetest)
- **Gentoo**: has [own version tracking](https://packages.gentoo.org/packages/games-engines/minetest), no need to contact

You can find out how quick various distro are to adopt new versions by visiting [Repology](https://repology.org/project/minetest/history)
(older history under the `minetest` name, newer history is [here](https://repology.org/project/luanti/history)).

### ContentDB

#### Add a new version

Add the new version to the drop-down list of compatible Luanti versions that authors can select for their things.

Note that CDB tells Luanti versions apart by their protocol version so this is obviously not applicable to patch releases.

People who have access: rubenwardy + ???

#### Package devtest

Minetest Game is no longer connected to our release cycle, so we can ignore it.

The [Development Test](https://content.luanti.org/packages/Luanti/devtest/) package needs to be released **manually**. Make a new release, upload a ZIP file with Development Test as it looks like the Luanti source tree in the stable branch, and set the minimum and maximum Luanti versions to the exact Luanti version it is intended for.
