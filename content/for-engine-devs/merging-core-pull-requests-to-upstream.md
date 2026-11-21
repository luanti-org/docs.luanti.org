---
title: Merging core pull requests to upstream
aliases:
  - /Merging_core_pull_requests_to_upstream
  - /merging-core-pull-requests-to-upstream
  - /engine-dev-process/merging-core-pull-requests-to-upstream
---

This page contains **technical guidelines** for core developers when deciding whether to merge a pull request.

For determining who is allowed to do what, see [Organization](/for-engine-devs/organization).

For guidelines and rules on Git and Github, see [Git Guidelines](/for-engine-devs/git-guidelines).

Also see:

- [https://github.com/luanti-org/luanti/blob/master/.github/CONTRIBUTING.md](https://github.com/luanti-org/luanti/blob/master/.github/CONTRIBUTING.md)
- [https://github.com/luanti-org/luanti/blob/master/doc/direction.md](https://github.com/luanti-org/luanti/blob/master/doc/direction.md)

## Requirements

There are five major requirements that each pull request must fulfill in order to be mergeable to upstream Luanti.

1.  It should follow a roadmap in some way, to make sure it fits the whole picture of the project. Different roadmaps and project guidance is managed in [https://github.com/luanti-org/luanti/blob/master/doc/direction.md](https://github.com/luanti-org/luanti/blob/master/doc/direction.md). A core dev can decide to review and merge something that doesn't follow direction.md if they consider it to be beneficial to the project.
2.  It must work in the first place. Compile it and test it in game, or write mod code that uses it.
3.  The code style must be correct. [Code style guidelines](/for-engine-devs/code-style-guidelines)
4.  The internal interfaces of the code must be good, and it should be reasonably optimized, depending on how often the code is called.
5.  The protocols and formats that it uses must be well designed, including the required compatibility in the part in question. On-disk formats are extremely important to get right. Modding API concerns are split between this and req2. This is about knowing the engine's design along with its pitfalls.

...which can be checked by:

1.  Anyone.
2.  Anyone, or at least any modder.
3.  Those with a bit of general C++ and Lua knowledge.
4.  Those with a lot of general C++ and Lua knowledge.
5.  Those who have studied the file formats, the protocol and the Lua API, and know how version compatibility can be reliably done.

## How can this help?

I (celeron55) am interested in seeing whether we could end up in a situation where pull requests would get comments like "req 1, 2 and 3 checked.", after which someone could do the rest and ultimately anyone could merge it based on these simple comments, after each of the five requirements have been checked.
