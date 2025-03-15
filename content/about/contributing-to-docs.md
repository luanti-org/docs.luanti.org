---
title: Contributing to Docs
aliases:
  - /about-this-site/guidelines
  - /about/guidelines
  - /about-this-site/local-development
  - /about/local-development
  - /about-this-site/making-changes
  - /about/making-changes
  - /about-this-site/rules
  - /about/rules
---

# Contributing to Docs

Luanti Documentation is written in Markdown and transformed into HTML by [Hugo](https://gohugo.io), a free and open-source static site generator. The source code is currently hosted on GitHub and deployed with GitHub Actions. The look and feel of the site is primarily driven by the Hugo Book (HB) theme, with changes applied as described in [HB's readme](https://github.com/alex-shpak/hugo-book#hugo-book-theme).

## Local Development

When cloning the repository you need to clone it recursively so that the theme submodule gets included:

```bash
git clone --recursive https://github.com/luanti-org/docs.luanti.org
```

If you have already cloned, you can fetch the submodules as such:

```bash
git submodule init
git submodule update --remote
```

This project uses [Hugo](https://gohugo.io/) to build the site and various Node packages to test it.

You can install Hugo locally as a [Node.js](https://nodejs.org) package for convenience. Node is also used for further testing scripts, like spell-checking and a11y. These scripts are described in [`package.json`](https://github.com/luanti-org/docs.luanti.org/blob/master/package.json) and [`readme.md`](https://github.com/luanti-org/docs.luanti.org/blob/master/README.md).

To install and run via Node:

```bash
npm install # Installs packages needed to build and test the site
npm start # Builds and serves the site on a localhost port for manual testing
```

If you prefer, you can manually install the relevant "extended" Hugo release from [Installation | Hugo](https://gohugo.io/installation/). Be sure to match the version present in `hugo.yaml`.

To run Hugo directly:

```bash
hugo server # This is the command internal to `npm start`
```

### Spell-checker

You can run the spell-checker using `npm run test:spelling`. You can also install the recommended "Code Spell Checker" VS Code extension to get inline annotations of apparent spelling mistakes. The extension might fail to call out all spelling mistakes: consider `npm run test:spelling` the correct response.

If you find a word that's spelled correctly but not recognized by the spell-checker, use the context menu's "Spelling > Add Words to CSpell Configuration" command or manually update `cspell.json` to include the word.

You can disable the spell-checker with Hugo comments:

{{% comment %}} Note to readers of the source: exclude the `/*` and `*/` to unescape the shortcode. {{% /comment %}}
{{% comment %}} cspell:disable {{% /comment %}}
{{% comment %}} cspell:enable {{% /comment %}}

```md
This text is spell-checked.

{{%/* comment */%}} cspell:disable {{%/* /comment */%}}

The text is not splel-checked ;)

{{%/* comment */%}} cspell:enable {{%/* /comment */%}}

Finally, this text is spell-checked again.
```

### Proposing changes

At the bottom of each page is an `edit this page` button. Clicking this will open up GitHub's web text editor where you can make text changes and commit. Alternatively you can use GitHub's VS Code interface from the repo page by clicking the `.` key.

Changes are reviewed via the common "fork and pull request" approach. If you're unfamiliar, this should serve as a quick guide. Feel free to reach out to #luanti-docs in IRC, Discord, or Matrix for further assistance :)

- Please fork the repo (the `edit this page` option at the bottom of each page will walk you through this)
- Web-based
  - Once the repo is forked, go to the repo page and click `.`: this will open VS Code in the browser
  - Checkout a branch (bottom left, second option in)
  - Make your edits
  - In the left vertical menu, select the source control option
  - Enter a message about your changes and click `commit and push`
- Local-based (terminal commands for example, use GUI client if you wish)
  - Clone the repo `git clone https://github.com/YOURUSERNAME/docs.luanti.org`
  - Make a branch `git checkout your_branch_name`
  - Make edits with the tools of your choosing
  - Add, commit, and push the changes: `git add -A && git commit -m "your commit message here" && git push`
- Make a PR from the branch back to the repo
- Wait for someone on the Luanti docs team to review and merge (we usually review PRs daily)

## Guidelines

### Add [Front Matter](https://gohugo.io/content-management/front-matter/)

Front matter is metadata at the top of every file, inside a block that starts and ends with three dashes.

```yaml
---
title: Your Page Title
aliases:
  - /old/page/path
---
```

Front matter should be in YAML.

The `title` attribute is required on every markdown file, and in the case of directory `_index.md` files, the `bookCollapseSection` attribute should be set to `true`.

If you move or rename a Markdown file, add an [`aliases`](https://gohugo.io/content-management/urls/#aliases) value for redirects to keep the old URL working.

### Formats

- Use American English spelling
- Write content in Markdown
- Use webp images instead of jpg, png, or others
- Add a [language](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks#syntax-highlighting) to code blocks

### Merge Policies

Please announce changes (in optional minutes) in the #luanti-docs IRC/Discord/Matrix channel

- Anything in /for-creators/api should be approved by one luanti-docs member that isn't yourself
- Anything core engine processes should be approved by one luanti-engine team member that isn't yourself minimum
- For all else: if you're a luanti-docs member, use your best judgement
- Trivial changes exempt from the first two points
