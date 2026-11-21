---
title: Version Number
aliases:
  - /Version_number
  - /version-number
---

The **version number** of Luanti is used to keep different versions of Luanti apart.

## Stable releases

A stable release is any publicly released version of Luanti where downloadable builds are officially made available on the [downloads page](https://www.luanti.org/downloads/), and sometimes patch releases are only made available for specific platforms.

The version number of a stable release is a sequence of 3 natural numbers (including 0). The leftmost number is the most significant number, denoting more and more significant changes. The full version number of Luanti is always displayed in the window title.

```
MAJOR.MINOR.PATCH
```

- `MAJOR` is increased by 1 for very significant (and usually gigantic) releases that change a lot of things (likely to be in a compatibility-breaking manner).
- `MINOR` is increased by 1 for regular releases that contain features and/or bugfixes. These releases can be large or small. The core developers try to keep backwards-compatibility, but this is not always guaranteed.
- `PATCH` is increased by 1 for releases that only contain bugfixes. Usually these releases only change a few things and are usually very small and simple. These releases should be backwards-compatible.

When a new version is released, one of the numbers is increased, depending on the complexity of the release. The less-significant numbers are reset to 0. For example, if the current version is 5.1.2, and we want to make a `MINOR` release, the next version will be 5.2.0.

#### Old format (before 5.0.0)

_This section is only for historic interest._

Before version 5.0.0, the version number was slightly different:

```
ZERO.MAJOR.MINOR
```

- `ZERO` is the first number, which was always 0 (before 5.0.0). It is meant to follow [Zero-Ver](https://0ver.org/), but with the development of 0.5.0 it was decided that the versioning scheme should be changed as the second number essentially acted as the major version number and the zero would realistically never be bumped.
- The meaning of `MAJOR` and `MINOR` is unchanged.

For patch releases, the `PATCH` number was appended:

```
ZERO.MAJOR.MINOR.PATCH
```

The first public version of Luanti was version 0.0.1 (back then it was still called "Minetest-c55"). The last version using the old format was version 0.4.17.1.

## Developer versions

A so-called "developer version" (or "unstable version") of Luanti is any state of Luanti that has not had a public, official, tested release with a stable version number. This is the case when you downloaded a nightly build or compiled Luanti from source code on the master branch.

### The short format

Long story short: If there's a `-dev` in the version number, it's a developer version.

The easiest way to mark a particular version as a "developer version" is to take the stable version number we expect to see in future and, then we append a `-dev` to it:

```
<FUTURE_VERSION>-dev
```

- `<FUTURE_VERSION>` is the stable version number we expect to release in future.
- `-dev` is literally the text `-dev` which stands for, you guessed it, "development".

For example, version 5.0.0-dev refers to a "developer version" that we expect to become (after changes, possibly) version 5.0.0 eventually.

But note this short format can be ambiguous. It could refer to ANY "developer version" that exists between version 5.0.0 and 0.4.17.1 (the previous "stable" version), and is not specific. Your version 5.0.0-dev could be entirely different from the version 5.0.0-dev of your friend.

### The full format

The full developer version number is a bit longer and is written in the window title:

```
<FUTURE_VERSION>-dev-<COMMIT><DIRTY>
```

- `<COMMIT>` are the first 8 characters of a hash of the Git commit that Luanti was compiled with, if the source code was cloned using Git.
  - In simpler terms: This is a string that can uniquely identify what point in development the build is from. Note this is not necessarily in increasing order.
- `<DIRTY>` is literally `-dirty` when your Luanti is said to be "dirty". It becomes dirty when you have made changes in the source tree that aren't committed to Git, acting as a note that additional changes may have been applied to this build. If you didn't touch anything this should be empty.

For example, the version number "5.0.0-dev-a18c310a" means that this is a developer version of Luanti with the Git commit hash "a18c310a" and we believe this state of Luanti will (likely after many more changes) eventually become version 5.0.0.

An example for a "dirty" version would be "5.0.0-dev-a18c310a-dirty".

If you want to report bugs, please always give the full version number which you see in the window title.
