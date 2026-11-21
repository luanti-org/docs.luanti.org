---
title: Git Guidelines
aliases:
  - /Git_Guidelines
  - /git-guidelines
  - /engine-dev-process/git-guidelines
---

This page is mostly directed to core team members with commit or triage access to upstream repositories.

For determining who is allowed to do what, see [Organization](/for-engine-devs/organization).

For guidelines about overall pull request quality, see [Merging core pull requests to upstream](/Merging_core_pull_requests_to_upstream).

## Rules

### Upstream branch rules

Feature freezes take effect on the master branch.

If desired a separate development branch ("dev") can be made into which pull requests are merged during the freeze. It is then rebased onto master once the freeze is over.
However, this is **usually skipped** when nobody feels like merging features during a feature freeze.

The `luanti` repository contain the stable-5 branch, which has to be updated to the latest stable series version at each release.

### Upstream pull requests

- Don't mix multiple unrelated things in one commit. The same applies to code style cleanup.
- People considering merging pull requests are not required to look anything up anywhere else than the pull request and its comments.
  If there is something blocking the merging of a pull request, the reason must be added as a comment to the pull request.
  This goes both ways: If you check a pull request to be mergeable, use the review feature to approve it.

### Upstream commit rules

- You can push something to upstream \[1\] only if two members of the core team \[2\] agree on it. (See also [Organization](/for-engine-devs/organization))
  - Two for-votes are required for code to be mergeable upstream. Any against vote has to be resolved in a meeting before merge.
  - For PRs: The second reviewer should invalidate the first review when major changes happened to the PR in the meantime.
  - There are two exceptions to this rule as detailed in the following section.
- Commit messages must start with a capital letter and must be in the present tense. (look at the commit log)
- Do not modify history (i.e. force push) older than 10 minutes.
- Use rebase, not merge, to get linear history. \[3\]
- Do not rush anything, unless our users' data is about to be corrupted otherwise.

## Rule 1 in practice

Tell people openly what you do, and if someone finds a problem in what you do, allow resolving to take its time.

One exception to this rule is if you have a **small patch**, fixing some compiler error or other trivial mistake.
Notify about fixing it on #luanti-dev, wait for 5-15 minutes and push it.
To save time, you should notify when _finding_ the problem, not when _having it fixed_. If someone asks something about it, delay pushing and link the patch \[4\] or tell whatever else people want to know.

Another exception is for low-risk PRs under the "**one approval rule**" (use the corresponding label).
One approval (including your own) is enough when all of these match:
- The change doesn't touch the disk and network formats
- The change doesn't touch the main server-side Lua API
- You are able to argue the change is not likely to cause issues (i.e. it's simple)
- The CI build passes on multiple platforms

Note that rule 1 is only applied to the [luanti-org/luanti](https://github.com/luanti-org/luanti) and [luanti-org/minetest_game](https://github.com/luanti-org/minetest_game) repositories.
For the other repos apply some common sense: Check who last worked on it or who wrote most of the code (if applicable), consider consulting them for changes especially if they're large.
If nobody has cared about a repo for a long time you don't have to worry either.

#### Notes

\[1\] Upstream is at [https://github.com/luanti-org/luanti](https://github.com/luanti-org/luanti)

\[2\] The team: [https://github.com/orgs/luanti-org/people](https://github.com/orgs/luanti-org/people)

\[3\] On Github, press the "Rebase and merge" button. Of course you can rebase a remote branch in a local repository for more in-depth tools. There's also the ancient workflow of appending .patch to the pull request URL, getting into your project directory and doing `git am <patch>`. Similarly for single commits.

\[4\] Patches can be linked using a pastebin or by using GitHub (pull request or not).

## Issue and Pull-Request Management

- If an issue is a duplicate, post "duplicate of #ISSUE_NUM", label as [Duplicate](https://github.com/luanti-org/luanti/labels/Duplicate), and close the issue. Newer issues should be considered duplicates of older issues, unless the newer issue has more useful conversation. Information from the duplicate issue can also be edited into the open issue.
- If a pull request or an issue does not get a response from its author within one month when requiring more details, it is closed.
  - Use ["Action / change needed"](https://github.com/luanti-org/luanti/labels/Action%20%2F%20change%20needed) and ["User feedback needed"](https://github.com/luanti-org/luanti/labels/User%20feedback%20needed)
- Core devs who reviewed a PR once should stay with the PR for additional review rounds. Loss of interest (thus unsubscribing) should be signalled properly.
  - This is best communicated by assigning yourself to the PR using the GitHub feature.
  - PR assignments show who's taking care of the PR; leaving the option to @ them to progress.
- [WIP](https://github.com/luanti-org/luanti/labels/WIP) / draft pull-requests that are not updated within 6 months should be closed.
- The [Possible Close](https://github.com/luanti-org/luanti/labels/Possible%20Close) label can be used to warn authors of impending closure.

### Triagers

- Triagers are members of the project that are not core developers, but have the ability to manage issues - see above.
  - Examples include labelling issues, asking for necessary information, and managing boards to help with long-term planning.
- They may close issues or PRs, but cannot approve them (the act of approving an issue implies there is a dev willing to review a contribution).
- They should err on the side of caution - if they don't understand the issue, they should wait for feedback.
- They should consider ways to improve project management further.
