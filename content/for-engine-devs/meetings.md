---
title: Meetings
aliases:
  - /Meetings
  - /meetings
  - /engine-dev-process/meetings
---

# Meetings

Core developer meetings are held when needed. They take place in the #luanti-dev [IRC](/about/irc) channel.

This Wiki page contains the meeting plans for upcoming meetings, and meeting notes for previous meetings.

**Only core team should edit this page.** You can raise points to be discussed in an upcoming meeting by contacting a core dev, or messaging on IRC. Please avoid derailing meetings by posting unrelated things whilst discussion about other things is happening.

## Upcoming Meetings

**Remember to post meetings in the [GitHub discussions](https://github.com/orgs/luanti-org/discussions) in advance**

## 2026-02-15

Add your points here. Most important comes first.

**Organization Discussion**
- Consider a general policy for experimental features (see also [past discussion](https://github.com/luanti-org/luanti/issues/15548))

**PR discussion/reviews**
- [Matrix and rotation API](https://github.com/luanti-org/luanti/pull/16870)
- [Node param2 bitfield](https://github.com/luanti-org/luanti/pull/16839) -\> Does it look well to be done in this way?

Also consider:
- ["One Approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Apr+is%3Aopen+label%3A%22One+approval+%E2%9C%85+%E2%97%BB%EF%B8%8F%22) and decide on whether to merge, request changes or close.

## Past Meetings

## 2026-01-31 (FOSDEM)

* Pizza
* Governance, organization, development

## 2026-01-04

Add your points here. Most important comes first.

**Organization Discussion**
- Feature freeze and release 18 January to avoid stress to whom goes to FOSDEM (Zughy)
- release an RC build

**PR discussion/reviews**
- decide which feature PRs to still get into 5.15.0 (if any)

Also consider:
- ["One Approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Apr+is%3Aopen+label%3A%22One+approval+%E2%9C%85+%E2%97%BB%EF%B8%8F%22) and decide on whether to merge, request changes or close.

## 2025-10-05
**Organization Discussion**
- Release Candidate/5.14? (Zughy)
  - LT 5.14 has been released

## 2025-09-21

**Organization Discussion**
- FF (Zughy)
  - Luanti is now in Feature Freeze

## 2025-09-07

**Organization Discussion**
- FF now and RC on September 21 (Zughy)
  - Not happening, maybe next meeting 

## 2025-07-27

**Organization Discussion**

- Release 5.13 and skip RC since we're two weeks late (Zughy)

**PR discussion/reviews**

- discuss PRs and issues in 5.13 milestone

## 2025-06-29

**Organization Discussion**

- It's the estimated FF + 5.13 RC (Zughy)
  - Not happening

**PR discussion/reviews**

- https://github.com/luanti-org/luanti/issues/14135 for 5.13?
  - Merged

## 2025-06-15

**PR discussion/reviews**

- Is it important that passwords are not stored excessively long in plain text in memory?
  If unimportant [#14196](https://github.com/luanti-org/luanti/pull/14196) should be closed
  and [#14129](https://github.com/luanti-org/luanti/pull/14129) should continue with passwords stored in plain text.
- The original author of [#13811](https://github.com/luanti-org/luanti/pull/13811) introduced a `class BitField` -> see [here](https://github.com/luanti-org/luanti/pull/13811/files#diff-2be35250aa71dc4852f4955c67e13cabe131c20b7373cc92385484b391aa8f62).
  If we like this class, we could create a separate PR for it and review and merge it before #13811, so that the latter will be smaller and easier to review.

## 2025-05-16

-No meeting notes have been taken-

## 2025-04-20

**Organization Discussion**

- Feature freeze time (Zughy)
  - Time to sort out a few feature PRs and then we'll go into feature freeze

## 2025-04-06

**Organization Discussion**

- Feature freeze time: I suggest doing it now and err on the side of caution, keeping the feature PRs in the milestone and releasing a RC in the next meeting (Zughy)
  - Let's do it in the next meeting
- Release schedule shift to accommodate game jam schedule (proposed: keep May release, lightweight release in July, continue 3-month from there)
  - Undecided

## 2025-01-19

**Organization Discussion**

- Feature freeze time (Zughy)
  - Feature freeze has started. Still undecided about what to do with SDL2
- PLEASE finish the [renaming process](https://github.com/luanti-org/luanti/issues/15322) before FOSDEM (Zughy)
  - Luanti organization has been created on GitHub, the rest is in the works. For more details see the [IRC log](https://irc.luanti.org/minetest-dev/2025-01-19#i_6236220)

## 2024-12-22

**Organization Discussion**

- `<y5nw> celeron55: may I propose moving the discussion about me to the 2024-10-13 meeting? (mainly due to personal reasons)`
  - (context: being a core dev) past date, but appears to not have been discussed yet. Answer still uncertain

**PR discussion/reviews**

- https://github.com/luanti-org/luanti/pulls?q=is%3Apr+is%3Aopen+scancodes

## 2024-12-08

**Organization Discussion**

- Finish renaming Minetest -> Luanti ([issue](https://github.com/luanti-org/luanti/issues/15322), Zughy)
  - Not finished, but steps forwards were made

## 2024-11-10

**Organization Discussion**

- release time
  - 5.10 is out

**PR discussion/reviews**

- How to fix the collision bugs?
  - reverted PR #15029 with #15400

## 2024-10-27

**Organization Discussion**

- renaming progress
  - Halfway through
- feature freeze progress
  - RC probably happening tomorrow. Will release in two weeks

## 2024-10-13

Meeting: [https://irc.luanti.org/minetest-dev/2024-10-13#i_6208356](https://irc.luanti.org/minetest-dev/2024-10-13#i_6208356)

**Organization Discussion**

- rename time! (Zughy)
  - Proceed to announce on the blog/forums, then rename iteratively
  - Blog post: grorp and Zughy are on it
- feature freeze time (Zughy)
  - Freeze started. Issues discussed quickly.

## 2024-09-15

**Organization Discussion**

- 5.9.1: [https://github.com/luanti-org/luanti/milestone/27](https://github.com/luanti-org/luanti/milestone/27)
- [Add gameid aliases](https://github.com/luanti-org/luanti/issues/14543) (herowl)
- (low priority) Future development cycle idea by pgimeno. Almost permanent feature freeze. (again, Krock)
  - Idea submission: [https://irc.luanti.org/minetest-dev/2024-07-17#i_6185892](https://irc.luanti.org/minetest-dev/2024-07-17#i_6185892)
  - Meeting discussion 1: [https://irc.luanti.org/minetest-dev/2024-08-04#i_6189904](https://irc.luanti.org/minetest-dev/2024-08-04#i_6189904)
  - TODO: Set up a poll
- Thoughts on the latest proposed core dev (Zughy)
  - Context?

## 2024-09-01

**Organization Discussion**

- 5.9.1: [https://github.com/luanti-org/luanti/milestone/27](https://github.com/luanti-org/luanti/milestone/27)
  - Currently 10 PRs ready (merged) that could be included. Not all milestone issues will be fixed in time.
- [Separate repo for requesting and discussing new features](https://github.com/luanti-org/luanti/issues/11568). See [here and next page](https://irc.luanti.org/minetest-dev/2024-09-01#i_6197652) for further info (Zughy)
  - A few core devs tend to be opposed to this idea. For a definitive result, a poll might be appropriate.

## 2024-08-18

- 5.9.1: [https://github.com/luanti-org/luanti/milestone/27](https://github.com/luanti-org/luanti/milestone/27)
  - Went though one by one

## 2024-08-04

**Organization Discussion**

- Finalize 5.9 (Zughy)
  - SDL character lookup issue:
  - `SDL_GetScancodeFromKey(SDLK_SLASH)` idea from _y5nw_ --> [https://github.com/luanti-org/luanti/pull/14894](https://github.com/luanti-org/luanti/pull/14894) (hacky workaround, updated)
  - See also: meeting discussion
- Future development cycle idea by pgimeno. Almost permanent feature freeze. [https://irc.luanti.org/minetest-dev/2024-07-17#i_6185892](https://irc.luanti.org/minetest-dev/2024-07-17#i_6185892) (Krock)
  - [https://irc.luanti.org/minetest-dev/2024-08-04#i_6189904](https://irc.luanti.org/minetest-dev/2024-08-04#i_6189904)
- [https://github.com/luanti-org/luanti/pull/13987](https://github.com/luanti-org/luanti/pull/13987) (observers)
  - Krock will check code paths to determine the risks.

## 2024-07-21

**Organization Discussion**

- 5.9

**PR discussion/reviews**

- [https://github.com/luanti-org/luanti/pull/14749#issuecomment-2222747966](https://github.com/luanti-org/luanti/pull/14749#issuecomment-2222747966) opinions needed
  - Krock: "sfan5's comment seems like a good workaround. I could open a PR for that if it's what we'd want"

## 2024-04-28

**Organization Discussion**

- Feature freeze when? May 12, release June 1? (Zughy)
  - Agreed on the start of June. Also, general consensus about releasing more often

## 2024-03-03

**PR discussion/reviews**

- [3d line rendering](https://github.com/luanti-org/luanti/pull/13020#issuecomment-1944150506): thoughts on grorp's primitive suggestion (Zughy)
  - [https://irc.luanti.org/minetest-dev/2024-03-03#i_6156739](https://irc.luanti.org/minetest-dev/2024-03-03#i_6156739)
  - general agreement to grorp's comment
  - low priority for PR author as of now
  - Idea: attach to object bones for more versatility (future PR)
- "One Approval" PR discussion (requested by sfan5)
  - 11391: appgurueu is on it
  - 14225 (bulk_get_node): let sfence provide reasons for not closing the PR
  - 14319 (nil puncher): answered
  - 14347 (pointing range): sfan5 is on it, Desour maybe too
  - 14369 (preserve metatables): appgurueu is on it
- [Camera API (draft)](https://github.com/luanti-org/luanti/pull/14325)
  - Questions & inputs: [https://irc.luanti.org/minetest-dev/2024-03-03#i_6156971](https://irc.luanti.org/minetest-dev/2024-03-03#i_6156971)

## 2024-02-18

**Organization Discussion**

- Consider converting unfinished / abandoned PRs to draft rather than closing them. Closing signals "no interest" / rejection, whereas draft signals "not ready" and might be more appropriate for "adoption needed" PRs or similar. (appgurueu)
  - Close + "Adoption needed" labels (as always) are better.
- Do we want approvals to be revoked when new commits are pushed? (There's a GH repo setting for this, celeron would have to enable it.) (appgurueu)
  - Wiki update WIP
- When do we want 6.0 to happen? After 5.9, after 5.10, even later? (appgurueu)
  - Not worth it at the moment, too few breakages on the list (doc/breakages.md) (Krock, sfan5)
  - Reference: [https://irc.luanti.org/minetest-dev/2024-02-18#i_6153824](https://irc.luanti.org/minetest-dev/2024-02-18#i_6153824)

**PR discussion/reviews**

- [Simplify bug report form](https://github.com/luanti-org/luanti/pull/14154): stale, we should take a decision (Zughy)
  - { Desour, sfan5 } for a simple text template, { rubenwardy, Krock } against
- [bulk_get_node](https://github.com/luanti-org/luanti/pull/14225) Worth or not? (Krock)
  - appgurueu might look into it again to judge the reliability of the performance tests
  - node get/set speedup PR: [https://github.com/luanti-org/luanti/pull/14384](https://github.com/luanti-org/luanti/pull/14384)

## 2024-01-21

**Organization Discussion**

- PRs are on the rise and FOSDEM is coming. If it keeps going like this, it'll probably be PR hell all over again. What can we do to avoid both core devs burnouts and contributors being ignored/waiting for ages? Is the roadmap working? (Zughy)
- Suggestion about core devs supporting PRs having to self-assign such PRs, see [here](https://irc.luanti.org/minetest-dev/2024-01-15#i_6146007). No general "we" should be accepted (Zughy)
  - Added the assignment guideline to [Git Guidelines](/Git_Guidelines#Issue_and_Pull-Request_Management). (Krock)
  - Try to assign yourself to PRs that you support to ensure they're not forgotten - even more so on such that are not on the roadmap.
  - The guidelines are currently a mess: [https://irc.luanti.org/minetest-dev/2024-01-21#i_6147476](https://irc.luanti.org/minetest-dev/2024-01-21#i_6147476) - need cleaning up into doc/

**PR discussion/reviews**

- What to do about [Add drawtype sunken and covered](https://github.com/luanti-org/luanti/pull/14103), see my comment (Zughy)
  - Left a summary of the discussion (Krock)
- When can we have [Reformat the code irr#248](https://github.com/minetest/irrlicht/pull/248), and irrlicht merge? (DS)
  - Only irr#276 would be nice to have merged before import
  - Added a milestone for Feb 4 to keep track of the waiting PRs.
- [Irrlicht build fix](https://github.com/minetest/irrlicht/pull/281) - I would be grateful about some opinions. (Krock)
  - PR updated with sfan5's suggestion.
- [Inventory stack swap fixes](https://github.com/luanti-org/luanti/pull/12595) - asking who would have time to review it (Krock)
  - Potential reviewers informed.

## 2023-12-10

**Organization Discussion**

- 5.8.0 release
  - See milestone for open tasks

## 2023-11-12

**Organization Discussion**

- 5.8.0 release
  - The [Formspec Android fix](https://github.com/luanti-org/luanti/pull/13872) should be merged beforehand (Krock)
    - grorp also looked into it, without any progress
    - Waiting on progress from any contributor
- Final answer about internal discussion #71 (Zughy)
  - Posted a poll at the end of the discussion, waiting for feedback

**PR discussion/reviews**

- Can anyone please check the log in [CJK characters looks been filtered in chat_message while the GUI could display them collectively](https://github.com/luanti-org/luanti/issues/13813#issuecomment-1730594420) so to understand what to do with the issue? (Zughy)
  - Krock took care of it, ball in OP's court
- [HUD/Formspec replacement PR](https://github.com/luanti-org/luanti/pull/12926) status update? (json (IRC) via Krock)

## 2023-10-15

Mainly a revisit of all milestone points without specific PR reviews.

**Organization Discussion**

- Feature freeze!
  - Revisit all milestone points (Krock)
    - After de-bundling, only minor or obscure issues are left over. Good enough for a 5.8.0 release (Krock)
  - Why did #13885 become a draft? (Krock)
    - grorp is not present for clarifying

## 2023-10-01

**Organization Discussion**

- Feature freeze? (Zughy)
  - Suggestion (see milestone): keep MTG to have enough time to de-bundle in 5.9.0 (Krock)
    - Voted for +2 weeks time to get this done.
    - Caveat: rubenwardy and grorp will not be available much in the next few weeks
  - Feature freeze on Oct 15, release on Oct 29
  - [LTO optimization](https://github.com/luanti-org/luanti/issues/13192) is not a blocker. Can be moved to 5.9.0 if needed.
- [Settings UI tracker](https://github.com/luanti-org/luanti/issues/13476) - Plenty of merged PRs, how much is still required for 5.8.0? (Krock)
  - Non-blocking issues for an eventual release.

**PR discussion/reviews**

- [Dual wield](https://github.com/luanti-org/luanti/pull/11016) - Feedback brainstorming (Krock)
  - Responded.

Concept approval for:

- [Liquid system](https://github.com/luanti-org/luanti/pull/13764) - Concept yes/no? (Krock)
  - Concept approved. Waiting for performance test results.

## 2023-09-17

**Organization Discussion**

- Feature freeze? (Zughy)
  - Milestone set to Oct 15 for feature freeze on Oct 1 (possibly a dev meeting day) (Krock)
  - Reserved additional time to assign milestones and to solve the MTG_follow-up issue [https://github.com/luanti-org/luanti/issues/13800](https://github.com/luanti-org/luanti/issues/13800)

**PR discussion/reviews**

- [Import Irrlicht](https://github.com/luanti-org/luanti/pull/13642) (wsor4035)
  - Generally agreed but needs addressing Desour's requirements before merge
  - It will have to be merged eventually (sfan5)
- [Code style guidelines for Java code](https://github.com/luanti-org/luanti/pull/13700) There are already guidelines for C++ and Lua code, but not (Android-specific) Java code. It can be as simple as, "Use Android Studio's linter/formatter." (srifqi)
  - Volunteers are welcome to extend this page, e.g. srifqi: [/Android_code_style_guidelines](/Android_code_style_guidelines)
- A follow up for [Issue 13583](https://github.com/luanti-org/luanti/issues/13583): Is there a draft already that I missed? This can be great for documenting supported OS/env. (srifqi)
  - The suggested document is currently WIP
  - Be clear how old toolchains we support, e.g. by supported Ubuntu LTS versions to use newer libraries or C++ revisions (Krock)
- [Dual Wielding](https://github.com/luanti-org/luanti/pull/11016) has been waiting for a feedback since 6 months. Since we're busy, I'd suggest to prioritize MTG removal and postpone this feature to 5.9, so that core devs can review it without any pressure (Zughy)
  - No progress and the issues could yet not be solved. Removed milestone, marked as draft.

Concept approval for:

- [Refactored the liquid system, and added an optional feature for liquids to flow to the next slope](https://github.com/luanti-org/luanti/pull/13764)
  - Comment written to clarify the situation (Krock)
- [Fix liquid falling if in "float" group](https://github.com/luanti-org/luanti/pull/13789)
  - Concept approved by Krock

## 2023-06-25

**Organization Discussion**

- Start considering a hypothetical new name, possibly privately. See [Here](https://github.com/luanti-org/luanti/issues/13510) (Zughy)
  - Will be discussed internally

**PR discussion/reviews**

- Less than two months to [5.8](https://github.com/luanti-org/luanti/milestone/22): we should consider what to do with a few issues/PRs in the milestone to avoid a last month burst (Zughy)
  - Not yet urgent. PRs and issues will be resolved when people have time for it.

Also discussed:

- [De-bundle Minetest Game](https://github.com/luanti-org/luanti/issues/13550)
  - Question about "featured" marks to make it more visible to newcomers

## 2023-05-28

**Organization Discussion**

- welcome srifqi
- 5.7.1 release (justified by [Android crashes](https://irc.luanti.org/minetest-dev/2023-05-23#i_6086412))

**PR discussion/reviews**

- Discuss and concept approve or close ["Roadmap: needs approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22Roadmap%3A+Needs+approval%22)
  - [make falling node checks check for protection](https://github.com/luanti-org/luanti/pull/12966) TL;DR: Some server made ladders falling, and protection doesn't protect from fall through on_punch by other players. Possible solutions include making the registered on_punch/on_dig/on_placenode function overwrite-able, or only checking for protection in on_punch. What to do? (DS)
    - Removed roadmap approval needed label. (DS)

## 2023-05-14

**Organization Discussion**

- close all the Jude's PRs that haven't been adopted (Zughy)
  - done

**PR discussion/reviews**

- ["Dual Wielding"](https://github.com/luanti-org/luanti/pull/11016) I think OP is waiting for a core dev feedback, see their last comment (Zughy)
  - ???

## 2023-04-30

Notes inherited from the meeting 2023-04-16

**Organization Discussion**

- now that 5.7 has been released, we should write down a roadmap for 5.8 onwards (Zughy)
  - No special action taken. Might need picking up later again.

**PR discussion/reviews**

- ["Move code style guidelines into repo"](https://github.com/luanti-org/luanti/pull/12749) Do we want this? (I'm myself neutral now that code blocks are no longer broken on the wiki.) (DS)
  - Generally okay but updating it might be cumbersome. Guidelines should apply to all Minetest (group) projects (Krock)
  - Closed by author (DS).
- ["Conditional textures for nodes, bulk texture changing"](https://github.com/luanti-org/luanti/issues/13417) Support for this? Or for the alternative linked in the original post? Or none? (Zughy)
  - Yet another feature request. No interest shown. Marked as low priority.
- ["Add utility script to update/generate \*.tr files"](https://github.com/luanti-org/luanti/pull/13464) roadmap approval or rejection?
  - Answer posted in [https://github.com/luanti-org/luanti/pull/13437](https://github.com/luanti-org/luanti/pull/13437)
- [Irrlicht as a submodule](https://github.com/luanti-org/luanti/pull/13215) - how to fix Android compatibility? Ideas are welcome.
  - Fix the Android script later? Waiting on numberZero when they have time (Krock)

## 2023-04-02

**Organization Discussion**

- Where should we post next meetings notice? (Zughy)
  - rubenwardy moved them to [https://github.com/orgs/minetest/discussions](https://github.com/orgs/minetest/discussions)
  - People with read access: MTE + MTG + triagers
- Complaints about connected glass changes - changes the meaning of glasslike_framed_optional and there's no way for texture packs to control it.
  - Reverted for now. Let's keep it as a subject for 5.8.0-dev
- 5.7.0 release
  - Aiming for 9 April or earlier. Not enough developer activity in this meeting.

## 2023-03-19

**Organization Discussion**

- Current roadmap evaluation and draft of the next one (Zughy)
  - Still need focus on UI/UX improvements, entities were not touched at all, de-hardcoding shouldn't be a point

## 2023-03-05

**Organization Discussion**

- Feature freeze? (Zughy)
  - Yes, now (sfan5, x2048).
- Dedicate the upcoming version to Jude/TurkeyMcMac (Zughy, original proposal by jp)
  - sfan5 will do that when updating the credits
- We're choking on PRs again: shouldn't MT advertise itself more so to potentially find new core devs and/or recruit existing contributors? (Zughy)
  - There is not much that can be done about it, wait for new devs.

**PR discussion/reviews**

- ["SSCSM execution"](https://github.com/luanti-org/luanti/pull/13046) (TurkeyMcMac)
  - Should the PR be reviewed in parts? If so, it can be reviewed at this point.
  - It would be good to divide up the work of implementing the various SSCSM APIs, if other people have time to work on this. See the TODO list in the PR description. To see how to implement SSCSM APIs, you can look at src/script/lua_api/l_csm.cpp and src/script/lua_api/l_csm_nodemeta.cpp.
  - Meeting note: x2048 might have a look at it. It should be merged in small chunks, preferably with unittests (Krock)
- ["Reorder client initialization"](https://github.com/luanti-org/luanti/pull/12189#issuecomment-1130461922) needs sfan's take to unlock the status of the PR (Zughy)
  - sfan5 unblocked it; will be reviewed when time comes.
- [MariaDB](https://github.com/luanti-org/luanti/pull/13266) Yes or No (Krock: Yes) - who could test this easily? (Krock)
  - obsolete entry
- ["Dual Wielding"](https://github.com/luanti-org/luanti/pull/11016) How should we go ahead with this? (sfan5)
  - Yes: item definition flag (sfan5 answered)
- [Add world-independent storage directory for mods](https://github.com/luanti-org/luanti/pull/12315) - approved, but there's concerns about how multiple instances of Minetest running will cause issues for modders. Is this acceptable? What should modders do in these situations? Is the design fundamentally flawed (rubenwardy)
  - Ideas: callback-based updates for sane locking (sfan5)

## 2023-01-08

**Organization Discussion**

- ["Host the generated Lua API documentation under api.luanti.org"](https://github.com/luanti-org/luanti/issues/13072) (Zughy)
  - Assigned to rubenwardy to move it to GitHub
- Feature freeze starting on January 22? I suggest to keep every feature currently listed in the milestone (Zughy)
  - focus on the Privacy Policy (sample available)
  - postponed the main menu (settings) rework to 5.8.0
  - minetest.get_player_window_information reviews requested
  - Dual Wield should be reviewed because it's what the players want

**PR discussion/reviews**

- ["Static binary builds for Linux on common architectures"](https://github.com/luanti-org/luanti/issues/13037): yes, no? (Zughy)
  - AppImages are coming and Docker is an universal solution. Additional static binaries are not necessary.
- ["Allow zoom even with overwritten default FOV"](https://github.com/luanti-org/luanti/pull/12953): There seems to be some disagreement on this in the comments. Is the change acceptable? (TurkeyMcMac)
  - Handled (Krock)
- ["Add node texture variants"](https://github.com/luanti-org/luanti/pull/12928): What item properties should be allowed to vary by variant? (TurkeyMcMac)
  - Currently supported: "tiles", "overlay_tiles", and "special_tiles"
  - Others that could be supported:
    - "inventory_image" and "wield_image": May be unnecessary as there's a separate PR for settings these using item metadata.
    - "mesh": Could be complicated to implement, I haven't looked into it.
  - Should all properties vary based on the same bit field ("param2_variant") or is this bad API design? (A single bit field is not a functional limitation. It is still possible to let properties effectively vary independently by making a variant for each possible combination.)
  - \-> Feedbacks given for the concept. Might be discussed again in the next meeting.

## 2022-12-04

**Organization Discussion**

- 5.7 release when? (Zughy)
  - somewhen in January is good (rubenwardy, sfan5)
- we should decide on the [new roadmap](https://github.com/luanti-org/luanti/issues/12746) (sfan5)
  - Commented an analysis of the previous roadmap and suggestions on how to increase "success rate" (Krock)

**PR discussion/reviews**

- ["Dual wielding"](https://github.com/luanti-org/luanti/pull/11016): can someone please take care of this PR if the author doesn't come back? It's a huge feature for modders (Zughy)
  - Author came back. Check the progress as usual.

## 2022-10-23

**PR discussion/reviews**

- [Add vector() constructor](https://github.com/luanti-org/luanti/pull/12772) - It seems this proposal is controversial. Its fate should be decided. (TurkeyMcMac)

## 2022-10-09

**PR discussion/reviews**

- [Add support for attached facedir/4dir nodes (comment)](https://github.com/luanti-org/luanti/pull/11432#issuecomment-1263562675) - Thoughts on adding this as part of the PR? (TurkeyMcMac)
- [Add callback on_mapblocks_changed](https://github.com/luanti-org/luanti/pull/12739) - Decide whether this is the right approach. We could go for something with node granularity, but it would probably be more expensive. (TurkeyMcMac)
- [Implement some VoxelManip functionality safely using LuaJIT's FFI library](https://github.com/luanti-org/luanti/pull/12611) - Decide whether the added complexity is worth it. (TurkeyMcMac)
- [Use .md extension for markdown files](https://github.com/luanti-org/luanti/pull/12449) - Please decide once and for all (Zughy)
- Shader brightness issue

## 2022-09-25

**PR discussion/reviews**

- [Set visibility of players/hide them from others](https://github.com/luanti-org/luanti/pull/9863) has been rebased twice, it's from 2020. Can anyone have a look at it? Or put it in the 5.7.0 milestone, I don't know (Zughy)
- [New physics modifiers](https://github.com/luanti-org/luanti/pull/11465) concept discussion (Wuzzy)
- The same goes for [Show relative screenshot filename on chat when applicable](https://github.com/luanti-org/luanti/pull/9776) (Zughy)

## 2022-09-11

- [Item pickup](https://github.com/luanti-org/luanti/pull/7712) general concept discussion
- [Item movement unit tests](https://github.com/luanti-org/luanti/pull/11885) required for bugfix PR [#12595](https://github.com/luanti-org/luanti/pull/12595)

## 2022-08-28

**Organization Discussion**

- Add some bugs to the 5.6.1 milestone? (Proposal: each core dev pick one and adds it)
- Two years have passed since the [first roadmap brainstorm](https://github.com/luanti-org/luanti/issues/10461): create another one to then follow after 5.7? Also to gather feedback

**PR discussion/reviews**

- [Use .md extension for markdown files](https://github.com/luanti-org/luanti/pull/12449) - yes or no?
- [Doc of set_sun/-moon: Absolute orientation and difference in scale](https://github.com/luanti-org/luanti/pull/12145) \- document difference in size or slightly change the scale of both the celestial bodies?

## 2022-08-14

**PR discussion/reviews**

- [https://github.com/luanti-org/luanti/issues?q=is%3Aopen+is%3Aissue+label%3AQuestion](https://github.com/luanti-org/luanti/issues?q=is%3Aopen+is%3Aissue+label%3AQuestion)
- [Add minetest.get_player_window_information()](https://github.com/luanti-org/luanti/pull/12367) - any requirements for merge?
- [Settings redesign](https://github.com/luanti-org/luanti/pull/12480) - how to resolve issues with focus?

## 2022-07-31

**Organization Discussion**

- Who writes the release changelog?
- When does the release happen? presumably today

**PR discussion/reviews**

- Work on merging the last PRs left in [the milestone](https://github.com/luanti-org/luanti/milestone/19)

## 2022-07-17

**PR discussion/reviews**

- review [5.6.0 Milestone](https://github.com/luanti-org/luanti/milestone/19)

Also consider:

- ["One Approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22One+approval%22) and decide on whether to merge, request changes or close.
  - in particular decide whether we want them for 5.6

## 2022-07-03

**Organization Discussion**

- Feature freeze date for 5.6.0
  - Starting on June 17. Close to 6 months release cycle.

**PR discussion/reviews**

- [Add minetest.get_player_window_information()](https://github.com/luanti-org/luanti/pull/12367)
  - How to resist fingerprinting? -> minor concern, rounding is possible
  - Move to player ObjectRef? -> player-specific function. not necessarily.
  - Should this be prioritized for 5.6? ->
- [Show dep errors in Select Mods modal](https://github.com/luanti-org/luanti/pull/12284) - pls someone review
  - review shall be done
- [Settings redesign](https://github.com/luanti-org/luanti/pull/12480) - concept approval, prioritization, requirements for submission
  - "yes but not in 5.6"
- [Bouncy improvements](https://github.com/luanti-org/luanti/pull/11939) - makes trampolines fun again.
  - review shall be done
- [repeat_place_time](https://github.com/luanti-org/luanti/issues/12493) - what do to about this?
  - Krock: write a bounds check or removal PR

Roadmapped

- [FOV mouse sensitivity](https://github.com/luanti-org/luanti/pull/12483)
- [http client fetch](https://github.com/luanti-org/luanti/pull/12463)
  - not needed
- [fix acceleration](https://github.com/luanti-org/luanti/pull/12353) / [fix gravity calculation](https://github.com/luanti-org/luanti/pull/11855)
  - approved
- [allow server to use media from texture_path](https://github.com/luanti-org/luanti/pull/10850)
  - flawed, but possibly helpful
- [Only count entity as "on ground" if they are colliding on the Y axis](https://github.com/luanti-org/luanti/pull/10587)
  - Krock + sfan5: rejected.
- [userdata command line option](https://github.com/luanti-org/luanti/pull/9424)
  - OK, but needs changes
- [Add keep_newlines argument to wrap_text](https://github.com/luanti-org/luanti/pull/7728)
  - OK, might need fixing
- [Add an item pick up callback (2)](https://github.com/luanti-org/luanti/pull/7712)
  - seems useful.

## 2022-06-05

**Organization Discussion**

- make a 5.5.2 release due to security fixes?
  - yes

**PR discussion/reviews**

- [Make minetest.write_json use empty list instead of "null" for empty tables](https://github.com/luanti-org/luanti/pull/12168) - is this approach good?
- [Raise default windowed resolution to 1280x720 (720p)](https://github.com/luanti-org/luanti/pull/12355) - yes or no?
  - no

## 2022-05-22

**PR discussion/reviews**

- [#11545](https://github.com/luanti-org/luanti/pull/11545) <- someone should review this --sfan5
  - x2048
- [#12367](https://github.com/luanti-org/luanti/pull/12367) some questions here
  - mostly answered before meeting
- [https://github.com/luanti-org/luanti/issues/12353](https://github.com/luanti-org/luanti/issues/12353) -- Fix acceleration by appgurueu

## 2022-05-08

**Organization Discussion**

- Gauge interest in 5.5.1 release
  - 3 in favor, 1 neutral
- Prioritize things for 5.6?
  - planned date: July, PRs will be added to milestone

**PR discussion/reviews**

- [#11251 (comment)](https://github.com/luanti-org/luanti/issues/12251#issuecomment-1115477945) - backwards compat hinders refactors, drop compat for old irrmt revisions this time?
  - _<+rubenwardy> anyway. I think the conclusion is that breaking it is fine, with some concerns over ease-of-use during dev_

Also consider:

- ["One Approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22One+approval%22) and decide on whether to merge, request changes or close.
- ["Roadmap: needs approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22Roadmap%3A+Needs+approval%22)
- oldest PRs

## 2022-04-24

**Organization Discussion**

- What to do with closing out old PRs? Should we start assessing old PRs under the roadmap / assigning people?
  - Yes, start marking PRs as supported by core dev / roadmap / needs approval. [https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+-label%3ARoadmap+-label%3ABugfix+-label%3A%22Supported+by+core+dev%22+sort%3Acreated-asc+](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+-label%3ARoadmap+-label%3ABugfix+-label%3A%22Supported+by+core+dev%22+sort%3Acreated-asc+)
- Release in June?
  - Potentially, will discuss 5.6.0 goals in next meeting

**Issues/planning**

- See proposal in [Optional dependencies being overridden/turned into hard dependencies](https://github.com/luanti-org/luanti/issues/8601)
  - Accepted
- [Calling set_detach() + set_properties() + set_pos() on the same step fails to teleport the player half of the time, especially online](https://github.com/luanti-org/luanti/issues/12092)
  - Marked as confirmed, to be investigated

**PR discussion/reviews**

- Close [Shader improvements: Saturation Optimization](https://github.com/luanti-org/luanti/pull/11854)?
  - Closed.

Roadmap: needs approval

- [Single functions to get/set every celestial vault element](https://github.com/luanti-org/luanti/pull/12181)
  - Approved.
- [Make debug.g/setmetatable respect the "\_\_metatable_debug" metatable field](https://github.com/luanti-org/luanti/pull/12118)
  - Postponed for further inputs.
- [WIP: Do not stat /etc/localtime all the time](https://github.com/luanti-org/luanti/pull/12080)
  - Needs actions or will be closed.
- [Add mods list formspec display for /mods cmd](https://github.com/luanti-org/luanti/pull/11946)
  - Approved but needs a better implementation.

High-level/Concept approval for:

- [Add support for translating content title and descriptions](https://github.com/luanti-org/luanti/pull/12208)
  - Inputs provided.
- [Add login/register dialog to separate login/register](https://github.com/luanti-org/luanti/pull/12185)
  - Inputs provided.

Other:

- New spatial index library for entity lookup performance improvements [12040](https://github.com/luanti-org/luanti/pull/12040)
  - Library needs checking, first PR should also replace existing spatial lib
- [Add bulk ABMs by TurkeyMcMac](https://github.com/luanti-org/luanti/issues/12128)
  - Unsure of the question, skipped

## 2022-04-10

**Organization Discussion**

- More regular meetings
  - agreed on fortnightly meetings, starting 2022-04-10 and 2022-04-24
- Review roadmap rules. Remove 1 week limit, use meetings instead
  - raise limit to 1 month, attempt review sooner during regular meetings

**PR discussion/reviews**

Also have a look at "One Approval" PRs and decide on whether to merge, request changes or close.

- [SQLite media cache](https://github.com/luanti-org/luanti/pull/11567)
  - Closed for lack of benchmarks. Opinion is split on whether sqlite is a good approach
- [proller's breaking world](https://github.com/luanti-org/luanti/pull/11843)
  - Closed due to scale of changes vs prioritization. Agreement made that a hard network break should not be done
- [Add API function minetest.activate_objects_in_area](https://github.com/luanti-org/luanti/pull/11630)
  - worth more consideration, posted comment in PR
- [CSM settings](https://github.com/luanti-org/luanti/pull/12131)
  - Supported by core dev
- [Add text tile modifier](https://github.com/luanti-org/luanti/issues/12084)
  - Closed as draft and bad approach
- [Restore and enhance bouncy behavior by pecksin](https://github.com/luanti-org/luanti/issues/11939)
  - Supported by core dev
- [Dual Wielding](https://github.com/luanti-org/luanti/issues/11016)
  - concept accepted, no need for an arbitrary amount of wields
- [A light_source should not override sunlight](https://github.com/luanti-org/luanti/issues/11933)
  - Krock brought up an implementation concern
- [Add rotation support for wallmounted nodes in 'ceiling' or 'floor' mode](https://github.com/luanti-org/luanti/pull/11073)
  - concept accepted

**MTG discussion/reviews**

- [Don't block fluid (water, lava) flow by flowers, mushrooms, grass and saplings](https://github.com/luanti-org/minetest_game/pull/2942)
  - Does this break builds? Does this count as a feature? **\->** Closed as too close to a feature

## 2021-09-04

Add your points here. Most important comes first.

**Organization Discussion**

**PR discussion/reviews**

- Android update for November
  - WIP. Waiting for rubenwardy. Not very urgent yet.
- Irrlicht milestone
  - Await hecks' OpenGL rewrite until beginning of October.
  - .. otherwise remove shadow map main menu setting and disable within C++
- ItemStack metadata network optimizations - [https://github.com/luanti-org/luanti/pull/11014](https://github.com/luanti-org/luanti/pull/11014)
  - Closed.
- Automatic selection of mods - Adopt or Won't add - [https://github.com/luanti-org/luanti/pull/11274](https://github.com/luanti-org/luanti/pull/11274)
  - Closed.
- Death item duplication and callback fix - [https://github.com/luanti-org/luanti/pull/11445](https://github.com/luanti-org/luanti/pull/11445)
- Joystick controls - [https://github.com/luanti-org/luanti/pull/11262](https://github.com/luanti-org/luanti/pull/11262)
- New movement tweak (trivial) - [https://github.com/luanti-org/luanti/pull/11367](https://github.com/luanti-org/luanti/pull/11367)
  - To be discussed later.
- Perhaps provide an alternative fix for [https://github.com/luanti-org/luanti/issues/9725](https://github.com/luanti-org/luanti/issues/9725)
- TODO reviews
  - Dynamic Media V2 [https://github.com/luanti-org/luanti/issues/11550](https://github.com/luanti-org/luanti/issues/11550)
  - Animated particle spawners [https://github.com/luanti-org/luanti/issues/11545](https://github.com/luanti-org/luanti/issues/11545)

Also have a look at "One Approval" PRs and decide on whether to merge, request changes or close.

**MTG discussion/reviews**

## 2021-01-23

**Organization Discussion**

- SDL2 integration into Irrlicht

  - Ensure that adding an "original Irrlicht" compatibility mode is somehow possible if necessary (ex. Android). (mentioned by sfan5)
  - Development on a separate branch to merge when it's usable
  - SDL2 as Irrlicht device. WIP (nerzhul) [https://github.com/luanti-org/luanti/pull/10780](https://github.com/luanti-org/luanti/pull/10780)
  - SDL2 for text input. WIP (numzero)

- Plans for 5.4.0 and feature freeze
  - **Feature freeze on 30 Jan 2021**

**PR discussion/reviews**

- [https://github.com/luanti-org/luanti/issues/9352](https://github.com/luanti-org/luanti/issues/9352) get_player_information issue

  - No solution yet.

- [https://github.com/luanti-org/luanti/pull/10693](https://github.com/luanti-org/luanti/pull/10693) Builtin translate

  - Needs re-review. Soon ready for merge prior 5.4.0 freeze

- [https://github.com/luanti-org/luanti/pull/10636](https://github.com/luanti-org/luanti/pull/10636) Android, ready for review.

  - Gradle error is fixed in master (rebase)
  - rubenwardy will review/test

- [https://github.com/luanti-org/luanti/pull/10083](https://github.com/luanti-org/luanti/pull/10083) Inventory slots style

  - Waiting for merge or another approval (v-rob)

- [https://github.com/luanti-org/luanti/issues/10555](https://github.com/luanti-org/luanti/issues/10555) Game settings bug

  - Fix Settings for once and all in 5.4.0: [https://github.com/luanti-org/luanti/pull/10819](https://github.com/luanti-org/luanti/pull/10819)

- [https://github.com/luanti-org/luanti/pull/10265](https://github.com/luanti-org/luanti/pull/10265) Formspec image features
  - Considered for 5.4.0 prior freeze, but not priority

**MTG discussion/reviews**

- [https://github.com/luanti-org/minetest_game/pull/2803](https://github.com/luanti-org/minetest_game/pull/2803) Texture compression issue
  - Based on issue [https://github.com/luanti-org/minetest_game/issues/2801](https://github.com/luanti-org/minetest_game/issues/2801)
  - imagefilters.cpp imageCleanTransparent() might need fixing to get white-filled opaque leaves after adding colors to the texture
  - Affected textures are whitelisted, but ExeVirus review that again

## 2020-12-19

**Organization Discussion**

- 5.3.1 or 5.4.0 release date? There have been multiple big security fixes.
  - Perhaps January, depending on how quickly the milestone issues can be resolved
  - Texture alpha warnings issue: sfan5 needs time to work on it
  - game settings fallback issue: needs decision whether to fix overrideDefaults() or the whole Settings structure ([https://github.com/luanti-org/luanti/issues/10555](https://github.com/luanti-org/luanti/issues/10555))

**PR discussion/reviews**

Various concepts to judge:

- [https://github.com/luanti-org/luanti/pull/10384](https://github.com/luanti-org/luanti/pull/10384) image transparency
- [https://github.com/luanti-org/luanti/pull/10430](https://github.com/luanti-org/luanti/pull/10430) mapgen constant
- [https://github.com/luanti-org/luanti/pull/10516](https://github.com/luanti-org/luanti/pull/10516) base64 (2)

**MTG discussion/reviews**

## 2020-10-03

Logs: [http://irc.luanti.org/minetest-dev/2020-10-03#i_5738345](http://irc.luanti.org/minetest-dev/2020-10-03#i_5738345)

**Organization Discussion**

- 5.4.0 release plan -> December? [https://github.com/luanti-org/luanti/milestone/17](https://github.com/luanti-org/luanti/milestone/17)
  - \-> Rough approximated date for release (after 6 months)
- Roadmap - everyone list 3 goals for development (ie: Improve UI), this can then be condensed into a roadmap of goals.
  - \-> [https://github.com/luanti-org/luanti/issues/10461](https://github.com/luanti-org/luanti/issues/10461)
- Add trusted contributors as issue triagers. [https://github.com/orgs/minetest/teams/engine/discussions/25](https://github.com/orgs/minetest/teams/engine/discussions/25)
  - Suggested users: Wuzzy, Calinou. Wuzzy was already offered core dev, but refused.
  - They would help ensure issue quality (asking for information, editing, closing duplicated) and organize issues (labelling, prioritizing)
  - This doesn't prevent core developers from doing the above.
  - celeron55 is willing to trial this, paramat is against this.
  - \-> Krock, rubenwardy: Demo run with a few chosen people. Rules will be written down during this phase to ensure everything's going well. Needs discussion with paramat.

**PR discussion/reviews**

- Decide on a "use_texture_alpha" solution: [https://github.com/luanti-org/luanti/issues/10342](https://github.com/luanti-org/luanti/issues/10342)
  - \-> sfan5 will take care of it
- Adopt or modify anticheat fix: [https://github.com/luanti-org/luanti/pull/10340](https://github.com/luanti-org/luanti/pull/10340)
- Require debug priv to view gameplay-relevant debug info (2nd try), add wireframe priv [https://github.com/luanti-org/luanti/pull/9315](https://github.com/luanti-org/luanti/pull/9315)
  - Requested by GreenXenith
  - \-> Will be reviewed...

**MTG discussion/reviews**

## 2020-08-01

**Organization Discussion**

- Theme of 5.4.0 (ie: what to focus on)
- New core developers
  - New member invited
- Minimal/Devtest policy: [https://github.com/luanti-org/luanti/issues/9645](https://github.com/luanti-org/luanti/issues/9645)
  - Wrote comment
- minetest-mods.github.io needs a library update - could there be any problems? volunteers?
  - postpone, not so relevant

**PR discussion/reviews**

- HUD minimap: [https://github.com/luanti-org/luanti/pull/9079](https://github.com/luanti-org/luanti/pull/9079)
- HUD compass: [https://github.com/luanti-org/luanti/pull/9312](https://github.com/luanti-org/luanti/pull/9312)
- Relative ~coordinates: [https://github.com/luanti-org/luanti/pull/9588](https://github.com/luanti-org/luanti/pull/9588)
- LuaEntitySAO API: [https://github.com/luanti-org/luanti/pull/9717](https://github.com/luanti-org/luanti/pull/9717)
  - Massive PR, but stalls. Needs clarification what to do
  - Incompatible with the recent on_step moveresult changes
  - Krock: Some API functions might be superfluous (not generic enough), check later again

**MTG discussion/reviews**

- Wall extension (trivial): [https://github.com/luanti-org/minetest_game/pull/2237](https://github.com/luanti-org/minetest_game/pull/2237)
- Craftguide: [https://github.com/luanti-org/minetest_game/pull/2396](https://github.com/luanti-org/minetest_game/pull/2396)

## 2020-06-06

**Organization Discussion**

- Allow or deny UTF-8 characters in the source code? [relevant PR](https://github.com/luanti-org/luanti/pull/9828)
  - Many different encodings. Non-ASCII characters should be avoided [http://irc.luanti.org/minetest-dev/2020-06-06#i_5698515](http://irc.luanti.org/minetest-dev/2020-06-06#i_5698515)
- Translations and minetest.conf.example need updating. Volunteers?
  - Reminder. Will be done when feature freeze starts.
- Feature freeze date
  - Bone rotation issues need to be fixed (or reverted)
  - Freeze in 1 week: 13 Jun (2 weeks for bugfixes after)

**PR discussion/reviews**

- [POC: Introduce SerializeStream helper class](https://github.com/luanti-org/luanti/pull/9914)
  - Split input/output stream and polish for a real PR.

**Suspected issues**

- [network desync](https://github.com/luanti-org/luanti/issues/9592)
  - sfan5: [http://irc.luanti.org/minetest-dev/2020-06-06#i_5698555](http://irc.luanti.org/minetest-dev/2020-06-06#i_5698555) not important for release, networking limits
- Wireframe glitch (graphics related)
  - Driver issue. Wait for AMD to fix their stuff

## 2020-03-07

Most important first.

**Organization Discussion**

- Release date for 5.2.0: March 14, if possible
- Open issues: [https://github.com/luanti-org/luanti/issues?q=is%3Aissue+is%3Aopen+label%3ABlocker](https://github.com/luanti-org/luanti/issues?q=is%3Aissue+is%3Aopen+label%3ABlocker)
  - Sky fix: [https://github.com/luanti-org/luanti/pull/9472](https://github.com/luanti-org/luanti/pull/9472)
  - Entity shader fix: (none yet)

**PR discussion/reviews**

- Delayed for 5.3.0: Android XXL PR [https://github.com/luanti-org/luanti/pull/9066](https://github.com/luanti-org/luanti/pull/9066)
  - Fix dependencies in [https://github.com/luanti-org/luanti_android_deps_binaries](https://github.com/luanti-org/luanti_android_deps_binaries)
  - Startup crash needs fixing
  - Build script works (sfan5)

**MTG discussion/reviews**

- Probably 5.3.0 or later: Craft guide: [https://github.com/luanti-org/minetest_game/pull/2396](https://github.com/luanti-org/minetest_game/pull/2396)
  - Should this be added in 5.2 or not?

## 2020-01-11

Arranged by: Krock

Most important first.

**Organization Discussion**

- Looking for new Core Developer members
- What's missing for 5.1.1?

**PR discussion/reviews**

- Formspec text cursor feature [https://github.com/luanti-org/luanti/pull/8665](https://github.com/luanti-org/luanti/pull/8665)
- Scroll container [https://github.com/luanti-org/luanti/pull/9101](https://github.com/luanti-org/luanti/pull/9101)
- Images in tabs [https://github.com/luanti-org/luanti/pull/8621](https://github.com/luanti-org/luanti/pull/8621)

**MTG discussion/reviews**

- Craftguide from pauloue [https://github.com/luanti-org/minetest_game/pull/2396](https://github.com/luanti-org/minetest_game/pull/2396)

## 2019-08-10

Arranged by: Krock

**PR discussion/reviews**

- Custom particle generators for particle spawners [https://github.com/luanti-org/luanti/pull/6688](https://github.com/luanti-org/luanti/pull/6688)
  - Should this be closed?
- Fix debug.txt growing bigger and bigger [https://github.com/luanti-org/luanti/pull/8382](https://github.com/luanti-org/luanti/pull/8382)
  - What to change? Or reject & close?
- Punchwear [https://github.com/luanti-org/luanti/pull/6804](https://github.com/luanti-org/luanti/pull/6804)
  - Who wants to adopt it? The feature was requested many times.
- Add support for set_formspec_prepend in main menu [https://github.com/luanti-org/luanti/pull/8611](https://github.com/luanti-org/luanti/pull/8611)
- Hide chat when console is open [https://github.com/luanti-org/luanti/pull/8656](https://github.com/luanti-org/luanti/pull/8656)

## 2019-06-29

Arranged by: rubenwardy

Logs: [http://irc.luanti.org/minetest-dev/2019-06-29#i_5561978](http://irc.luanti.org/minetest-dev/2019-06-29#i_5561978)

**Organization discussion**

- Is 5.0.2 needed for the newlines in setting name bug? ([https://github.com/luanti-org/luanti/pull/8590](https://github.com/luanti-org/luanti/pull/8590))
  - Yet no clear decision
- What is needed to have good mobs in Minetest (- Game)?
  - I suggest roping in mob devs like tenplus1 and stujones1
  - No blood included. Belongs to another mod
- Combine pathfinder issues into a single issue
  - [https://github.com/luanti-org/luanti/issues/8642](https://github.com/luanti-org/luanti/issues/8642)
- Possible close issues/PRs: [https://github.com/luanti-org/luanti/pulls?q=is%3Apr+is%3Aopen+sort%3Aupdated-desc+label%3A%22Possible+close%22](https://github.com/luanti-org/luanti/pulls?q=is%3Apr+is%3Aopen+sort%3Aupdated-desc+label%3A%22Possible+close%22)
- Does 1 disapproval block a PR?
  - SmallJoker seems to think it does

**Proposed order of merging formspec PRs**

The idea is to merge the most important PR first (top of list) to not drown in conflicts: **[https://github.com/luanti-org/luanti/projects/6](https://github.com/luanti-org/luanti/projects/6)**

- style\[\] [https://github.com/luanti-org/luanti/pull/8383](https://github.com/luanti-org/luanti/pull/8383)
- scroll_container\[\] [https://github.com/luanti-org/luanti/pull/8591](https://github.com/luanti-org/luanti/pull/8591)
- list spacing [https://github.com/luanti-org/luanti/pull/7971](https://github.com/luanti-org/luanti/pull/7971) (perhaps integrate into style\[\]?)
- Unarelith's refactors

## 2018-July-August

Arranged by: Krock

**Organization discussion**

- Will there be a 0.4.18 release?
  - [http://irc.luanti.org/minetest-dev/2018-07-14#i_5358759](http://irc.luanti.org/minetest-dev/2018-07-14#i_5358759) - sounds like a definitive "no"?
- Specify a rough 5.0.0 release date (month, year)
  - Freeze for multiple weeks for testing & fixing
- Forums: Clarify which rules (amongst license) apply for WIP modpacks/mods/games
  - [https://forum.luanti.org/viewtopic.php?t=20630](https://forum.luanti.org/viewtopic.php?t=20630)

**PR discussion/reviews**

- Colorize command parameters and privilege names ([https://github.com/luanti-org/luanti/pull/7019](https://github.com/luanti-org/luanti/pull/7019))
  - Improves readability - does it need \`string.format\` for a possibly prettier code?
- Protocol cleanups [https://github.com/luanti-org/luanti/pull/7348](https://github.com/luanti-org/luanti/pull/7348)
  - Review needed (compat break)

## 2018-06-??

Arranged by: Krock

**Organization discussion**

- Will there be a 0.4.18 release?
- Bug-fixing the bugfix release
  - [https://github.com/luanti-org/luanti/commit/014a1a08](https://github.com/luanti-org/luanti/commit/014a1a08) fix for [https://github.com/luanti-org/luanti/issues/6092](https://github.com/luanti-org/luanti/issues/6092)
  - [https://github.com/luanti-org/luanti/commit/a1598e1b](https://github.com/luanti-org/luanti/commit/a1598e1b) (c_internal.cpp only) fix for [https://github.com/luanti-org/luanti/issues/7419](https://github.com/luanti-org/luanti/issues/7419)
- Proposal: change master version scheme to: 5.0.0

**PR discussion/reviews**

- Colorize command parameters and privilege names ([https://github.com/luanti-org/luanti/pull/7019](https://github.com/luanti-org/luanti/pull/7019))
  - Improves readability - does it need \`string.format\` for a possibly prettier code?
- Lua parameter reading [https://github.com/luanti-org/luanti/pull/7410](https://github.com/luanti-org/luanti/pull/7410)
  - Good design concept? Comments?
- Protocol cleanups [https://github.com/luanti-org/luanti/pull/7348](https://github.com/luanti-org/luanti/pull/7348)
  - Review needed (compat break)
- CSM keypress - is this advantage acceptable? [https://github.com/luanti-org/luanti/pull/7008](https://github.com/luanti-org/luanti/pull/7008)
  - Needs flavors to be added: [http://irc.luanti.org/minetest-dev/2018-06-11#i_5329651](http://irc.luanti.org/minetest-dev/2018-06-11#i_5329651)

## 2018-05-19

Arranged by: Krock

Proposed time: 18:00 UTC

**Organization discussion**

- 0.4.17 - backport missing fixes and set date for freeze
  - ~Android-specific fix for [https://pastebin.com/j4mjniaq](https://pastebin.com/j4mjniaq) must be added too (during freeze?)~
  - Needs buildbot fixes (404 packages)
  - Backport [https://github.com/luanti-org/luanti/commit/4bb41a19d](https://github.com/luanti-org/luanti/commit/4bb41a19d) ([http://irc.luanti.org/minetest-dev/2018-05-19#i_5310266](http://irc.luanti.org/minetest-dev/2018-05-19#i_5310266))
- 0.4.17 - set release date

**PR discussion/reviews**

- Colorize command parameters and privilege names ([https://github.com/luanti-org/luanti/pull/7019](https://github.com/luanti-org/luanti/pull/7019))
  - Improves readability - does it need \`string.format\` for a possibly prettier code?
- Fix missing ignore textures ([https://github.com/luanti-org/luanti/pull/7326](https://github.com/luanti-org/luanti/pull/7326))
  - Needs more dev opinions & reviewing

## 2018-05-05

Arranged by: Krock

It's now almost a month since the last meeting - it time for the next one. Proposed time: 18:00 UTC

**Organization discussion**

- When do we have time to release 0.4.17?
  - Still unclear freeze and release dates. 1 MTE needs backporting first
- Whether to leave the issue number link in a commit message when merging on Github
  - MTE: Keep the numbers if they're inserted automatically (high PR count, helpful for the web interface blame or fast jumps in commits)
  - MTG: The numbers may be removed if wanted (small amount of PRs)
  - [http://irc.luanti.org/minetest-dev/2018-05-05#i_5298887](http://irc.luanti.org/minetest-dev/2018-05-05#i_5298887)

**PR discussion/reviews**

- Colorize command parameters and privilege names ([https://github.com/luanti-org/luanti/pull/7019](https://github.com/luanti-org/luanti/pull/7019))
  - Improves readability - does it need \`string.format\` for a possibly prettier code?
- Abort when trying to set a not registered node ([https://github.com/luanti-org/luanti/pull/7011](https://github.com/luanti-org/luanti/pull/7011))
  - Needs more dev opinions & reviewing

## 2018-04-07

Arranged by: Krock

**Organization discussion**

- Questions related to 0.4.17, 0.4.18 or future development?
- 0.4.17 release should be moved to 21/22 April for backports ([http://irc.luanti.org/minetest-dev/2018-04-07#i_5272834](http://irc.luanti.org/minetest-dev/2018-04-07#i_5272834))
  - Feature/backport freeze after 14 April to have enough time for testing

**PR discussion/review**

- Android joystick ([https://github.com/luanti-org/luanti/pull/7126](https://github.com/luanti-org/luanti/pull/7126))
- Formspecs: Add a `<use_color_alpha>` parameter to the box\[\] element ([https://github.com/luanti-org/luanti/pull/7116](https://github.com/luanti-org/luanti/pull/7116))
  - Causes errors in-game for older clients but doesn't affect playability - merge or not?
- Colorize command parameters and privilege names ([https://github.com/luanti-org/luanti/pull/7019](https://github.com/luanti-org/luanti/pull/7019))
  - Improves readability - does it need \`string.format\` for a possibly prettier code?

**MTG discussion/reviews**

- Tools: re-balance ([https://github.com/luanti-org/minetest_game/issues/1681](https://github.com/luanti-org/minetest_game/issues/1681))
  - Needs either a levelling system and/or special abilities for different materials to add variety
  - paramat suggests removing bronze, which is incorrectly a stronger tool than steel ([http://irc.luanti.org/minetest-dev/2018-04-07#i_5272959](http://irc.luanti.org/minetest-dev/2018-04-07#i_5272959))
  - Future mese concept: [http://irc.luanti.org/minetest-dev/2018-04-07#i_5273013](http://irc.luanti.org/minetest-dev/2018-04-07#i_5273013)

## 2018-03-31

Arranged by: Krock

**PR discussion/reviews**

- Introduce clang-tidy to the CI ([https://github.com/luanti-org/luanti/pull/6295](https://github.com/luanti-org/luanti/pull/6295))
- Android joystick ([https://github.com/luanti-org/luanti/pull/7126](https://github.com/luanti-org/luanti/pull/7126))
  - Minor change requests, discussion about joystick placement

**0.4.17 release schedule**

- Definitive release date
  - Announced. 15 April 2018, see [https://forum.luanti.org/viewtopic.php?f=18&t=19905](https://forum.luanti.org/viewtopic.php?f=18&t=19905)
- Any new, important bugfixes to backport?
  - Added the \`core.rotate_node\` bugfix, otherwise probably not. It's pretty much stable.
- Volunteer for cherry-picking the ToDo backport commits ([https://github.com/luanti-org/luanti/pull/6746](https://github.com/luanti-org/luanti/pull/6746))
  - `<sfan5>` probably me ([http://irc.luanti.org/minetest-dev/2018-03-31#i_5264276](http://irc.luanti.org/minetest-dev/2018-03-31#i_5264276))

## 2018-03-10

Arranged by: Krock

**Organization discussion**

- New Doxygen rule
  - Log: [http://irc.luanti.org/minetest-dev/2018-03-10#i_5249578](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249578)
  - Opposed: paramat, sfan5, SmallJoker, Shara
  - Comments in the functions directly are more welcome
- Group issues in projects?
  - Log: [http://irc.luanti.org/minetest-dev/2018-03-10#i_5249600](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249600)
  - Purpose is unclear and questionable due missing information, needs to be discussed later again

**PR discussion/reviews**

- Remove texture pack caching ([https://github.com/luanti-org/luanti/pull/6660](https://github.com/luanti-org/luanti/pull/6660))
- Don’t create CSM script env ([https://github.com/luanti-org/luanti/pull/6951](https://github.com/luanti-org/luanti/pull/6951))
  - Log: [http://irc.luanti.org/minetest-dev/2018-03-10#i_5249625](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249625)
  - Needs improvement, questionable solution due request of server-provided CSM ([https://github.com/luanti-org/luanti/issues/5393](https://github.com/luanti-org/luanti/issues/5393))
- “Bugfix” for 0.4-backported rotate_node calls ([https://github.com/luanti-org/luanti/pull/6900](https://github.com/luanti-org/luanti/pull/6900))
  - SmallJoker will test again and possibly merge after success
- Android C++11 build fix ([https://github.com/luanti-org/luanti/pull/6796](https://github.com/luanti-org/luanti/pull/6796))
  - Log: [http://irc.luanti.org/minetest-dev/2018-03-10#i_5249684](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249684)
  - sfan5 and nerzhul will try to test it soon. It does not break the regular setup, so a merge is to expect soon
  - Confirmed working by Wayward_One ([http://irc.luanti.org/minetest-dev/2018-03-10#i_5249720](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249720))
- Autogenerated settings header ([https://github.com/luanti-org/luanti/pull/6728](https://github.com/luanti-org/luanti/pull/6728))
  - Log: [http://irc.luanti.org/minetest-dev/2018-03-10#i_5249703](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249703) (stub)
  - Needs attention

**0.4.17 release schedule**

- Definitive release date
  - absent nerzhul: “release 0.4.17 ASAP” ([http://irc.luanti.org/minetest-dev/2018-03-10#i_5249422](http://irc.luanti.org/minetest-dev/2018-03-10#i_5249422))
- Are there any new, important bugfixes to backport?
- Volunteer for cherry-picking the 6 TODO (+ ^ from above) commits ([https://github.com/luanti-org/luanti/pull/6746](https://github.com/luanti-org/luanti/pull/6746))

## 2017-05-20

```
 Minetest developer meeting 2017-05-20

Last chance to merge/improve the remaining PR before feature freeze:

Old business:
 * Additional hooks for inventory manipulations callbacks (#4035)
   * Krock was working on it (#5647), but had an issue with a mysterious crash bug.  Progress?
 * Timed move (#1489)
   * Merge/close?
   * nerzhul: close for me
   * nore: Merge
   * Krock: Merge
 * Split block position in SQLite3 database (and use R*Tree) (#1845)
   * Rebased, performance tests to do soon(tm).
   * nerzhul: performance is a little bit slower, but it's ridiculous compared to mapblock
    deserialization part (no bench to show it atm, but i did it 2 years ago on my fork, and SQLite was improved since this date)
 * Add list_predict formspec element (#1988)
   * sofar worked on a rebase a while ago.  Close or merge a modern version?
 * New screenshots for website (web#78)  https://github.com/luanti-org/luanti.github.io/issues/78
   * ShadowNinja: "good" screenshots are fairly subjective and it doesn't look like we're actually
    getting anywhere near a decision in the issue thread, therefore I propose assigning someone to choose and install new screenshots.
   * > Calinou will handle it


New business:
 * First, release blockers:
 * https://github.com/luanti-org/luanti/pull/5767 Don't add damage flash while punch texture modifier is active #5767
   * > Bugfix, punted to next meeting.
 * https://github.com/luanti-org/luanti/issues/5782 [CSM] Disable preview mod before release #5782
   * CSM disabled by default anyways since it's experimental
   * > Fixed by #5554
 * https://github.com/luanti-org/luanti/issues/5728 Block instadig after tool switch #5728
   * > Fixed by #5785
 * nerzhul:
 * https://github.com/luanti-org/luanti/pull/5732 (CSM nodedefs/itemdefs read)
 * maybe close https://github.com/luanti-org/luanti/pull/5654
 * Single/Multiplayer tab merge https://github.com/luanti-org/luanti/pull/5627 (user UI experience)
   * > Merge with minor changes
 * Create world better experience: https://github.com/luanti-org/luanti/pull/5589
 * Permit to enable/disable mods on CSM  https://github.com/luanti-org/luanti/pull/5554 (very important)
   * ShadowNina: CSM is experimental, so I don't think any CSM issues should be blockers
     * nerzhul: here the problem if a user enable client side modding his experience will be affected by this preview mod, dedicated to modders)
   * > nerzhul will merge
 * https://github.com/luanti-org/luanti/pull/5589 (start world after creation)
 * LMDB backend (https://github.com/luanti-org/luanti/pull/4206)
   * nerzhul: Close? We already have 5 backends to maintain and we already cover all the use cases (client usage, performance usage, reliability usage)



```

## 2017-05-13

```
 Minetest developer meeting 2017-05-13

Old business:
 * Additional hooks for inventory manipulations callbacks (#4035)
   * Krock was working on it (#5647), but had an issue with a mysterious crash bug.
   * > No progress
 * Timed move (#1489)
   * Merge/close?
   * > nerzhul: close for me
   * > nore: Merge
   * > Krock: Merge
 * Split block position in SQLite3 database (and use R*Tree) (#1845)
   * Rebased, performance tests to do soon(tm).
   * nerzhul: performance is a little bit slower, but it's ridiculous compared to mapblock deserialization part (no bench to show it atm, but i did it 2 years ago on my fork, and SQLite was improved since this date)
 * Add list_predict formspec element (#1988)
   * sofar worked on a rebase a while ago.  Close or merge a modern version?


New business:
 * New screenshots for website (web#78)  https://github.com/luanti-org/luanti.github.io/issues/78
   * ShadowNinja: "good" screenshots are fairly subjective and it doesn't look like we're actually getting anywhere near a decision in the issue thread, therefore I propose assigning someone to choose and install new screenshots.
 * Fix 'alpha' property for liquid nodes (#5494)
   * > paramat testing.
 * CSM: Fix undocumented API calls (#5756)



nerzhul: please look at recent PR for this meeting. Feature Freeze is in 8 days
Here is a list to look at:
 * https://github.com/luanti-org/luanti/pull/5746 (cleanup content_mapblock)
 * https://github.com/luanti-org/luanti/pull/5732 (CSM nodedefs/itemdefs read)
 * maybe close https://github.com/luanti-org/luanti/pull/5654
 * Single/Multiplayer tab merge https://github.com/luanti-org/luanti/pull/5627 (user UI experience)
 * Create world better experience: https://github.com/luanti-org/luanti/pull/5589
 * Permit to enable/disable mods on CSM  https://github.com/luanti-org/luanti/pull/5554 (very important)


```

## 2017-05-06

```
Minetest developer meeting 2017-05-06

Old business:
    "Add configurable key bindings" (#1439)
        Rebased by nore, needs tweaks and merge.
    Additional hooks for inventory manipulations callbacks (#4035)
        Krock is working on it, progress?
    Timed move (#1489)
        Merge/close?
    nerzhul: close for me
    Use a settings object for the main settings (#1949)
        Rebased, should be merged soon.
    Split block position in SQLite3 database (and use R*Tree) (#1845)
        Rebased, performance tests to do soon(tm).
        nerzhul: performance is a little bit slower, but it's ridiculous compared to mapblock deserialization part
      (no bench to show it atm, but i did it 2 years ago on my fork, and SQLite was improved since this date)
    Add list_predict formspec element (#1988)
        sofar worked on a rebase a while ago.  Close or merge a modern version?
    Use object property "stepheight" for player stepheight too. (#2056)
        Agreed to merge, but nobody yet assigned.
    Make players respect makes_footstep_sound in the object properties (#2852)
        Agreed to merge, nobody assigned.

New business:
    Add support for multiple listen addresses (#2604)
        Big patch, but can't really be split up much more since everything's interdependent.
    Make the player collisionbox settable (#2738)
    Clean up numeric.h and split FacePositionCache from it (#3256)
        Up-to-date and looks more-or-less ready to merge, just needs formal approval.
    <nerzhul>:
        https://github.com/luanti-org/luanti/pull/5361 Background color on textarea/field/pwdfield)
        https://github.com/luanti-org/luanti/pull/5355: vertical bar in text areas
        Possible close: https://github.com/luanti-org/luanti/pull/5281 and https://github.com/luanti-org/luanti/pull/5279
        Documentation update: https://github.com/luanti-org/luanti/pull/4968 close or not close ?

For future meeting:
    If finished: https://github.com/luanti-org/luanti/pull/5544 (on_item_use CSM)


```

## 2017-04-29

Arranged by: paramat

ShadowNinja is unavailable today, paramat will be hosting in his stead.

**Old business**: (Things mentioned at previous meetings that haven't yet been resolved)

- "Add configurable key bindings" (#1439)
  - Rebased by nore, needs tweaks and merge.
- Additional hooks for inventory manipulations callbacks (#4035)
  - Krock is working on it, progress?
- Minetest subgame meeting
  - paramat held one at 1900Z
- Timed move (#1489)
  - Merge/close?
- Use a settings object for the main settings (#1949)
  - Rebased, should be merged soon.
- Split block position in SQLite3 database (and use R\*Tree) (#1845)
  - Rebased, performance tests to do soon(tm).
  - nerzhul: performance is a little bit slower, but it's ridiculous compared to mapblock deserialization part (no bench to show it atm, but i did it 2 years ago on my fork, and SQLite was improved since this date)
- Add list_predict formspec element (#1988)
  - sofar worked on a rebase a while ago. Close or merge a modern version?

**New business:** (mainly oldest or least recently updated PRs that haven't been handled yet)

- Added formspec element 'KeyEventBox' for char-based input by ninnghazad (#1737)
  - Closed: better done with client-side mods.
- Use object property "stepheight" for player stepheight too. (#2056)
  - Agreed to merge, but nobody assigned.
- Add mapgen settings to create world dialog by srifqi (#2561)
  - Merge, but needs changes.

## 2017-04-22

```
Minetest developer meeting 2017-04-22

Old business:
    "getTime refactoring" (#1188) not yet merged (nore)
    "Delayed shutdown" merged by nerzhul.
    "Disable Android LevelDB" rebased by nore and merged by nerzhul.
    "Add configurable key bindings" (#1439) rebased by nore, needs tweaks and merge.
    Additional hooks for inventory manipulations callbacks (#4035)
        Needs rebase and cleanup, volunteers?
    Rename .txt to .md (#5451)?

New business:  (PRs obtained by sorting by age on GitHub)
    Schedule Minetest subgame meeting?
    Coverity: https://scan.coverity.com/projects/12441
        Need to triage issues, many legitimate issues to fix.
    Timed move (#1489)
    Added formspec element 'KeyEventBox' for char-based input (#1737)
    Add ability to escape commands with a back-slash (#1768)
        <ShadowNinja> I may want to tweak this so that it doesn't strip the slash when you type "\foo" (or maybe not?
     It should at least work for the client-side dot-commands too)
    Split block position in SQLite3 database (and use R*Tree) (#1845)
    Use a settings object for the main settings (#1949)
        <ShadowNinja> I also unified Initialize and InitializeAsync, which was necessary to implement this.
        I think the unification is worth merging even without the settings object change.  This conflicts with just about
        the entire ScriptAPI, so I don't want to rebase it too often :-)
    Add listpredict to the formspec (#1988)
    Add non-global mod namespaces (#2039)
```

## Template

Add your points here. Most important comes first.

**Organization Discussion**

- ...

**PR discussion/reviews**

- ...

Also consider:

- ["One Approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22One+approval%22) and decide on whether to merge, request changes or close.
- ["Roadmap: needs approval" PRs](https://github.com/luanti-org/luanti/pulls?q=is%3Aopen+is%3Apr+label%3A%22Roadmap%3A+Needs+approval%22)
