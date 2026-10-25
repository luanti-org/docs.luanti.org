# Luanti Documentation

Hosted by GitHub Pages, generated with [Hugo](https://gohugo.io/).

Theme: [hugo-book](https://themes.gohugo.io/themes/hugo-book/)

See [/content](/content/) for content.

See [About](content/about/_index.md) for more information

## Contributing

See [Contributing to Docs](content/about/contributing-to-docs.md) before opening a pull request.

- [Local installation](content/about/contributing-to-docs.md#local-development)
- [Spell checking](content/about/contributing-to-docs.md#spell-checker)

## package.json

`package.json` dependencies and scripts are described here for readability:

- scripts
  - `build`: Builds the site to ensure changes are valid
  - `build:ci`: Builds the site but fails if any warnings are detected. Used in our CI pipeline.
  - `format`: Fixes formatting of the codebase
  - `start`: Builds and serves the site at http://localhost:1313 with Hugo.
  - `test:a11y`: Builds and serves the site, then uses Playwright and axe to test accessibility
  - `test:a11y:tests`: Not meant for independent use, only as part of `test:a11y`
  - `test:links`: Tests validity of all links in the site. WIP, ref [#177](https://github.com/luanti-org/docs.luanti.org/issues/177)
  - `test:spelling`: Reports all apparent spelling errors. WIP, ref [#83](https://github.com/luanti-org/docs.luanti.org/issues/83) for details.
  - `test:spelling-only`: Same as `test:spelling`, but only for the given files, e.g. `npm run test:spelling-only -- content/about/faq.md`. Used in our CI pipeline for pull requests.
  - `test:spelling-only-uncommitted`: Same as `test:spelling`, but only for files changed or added, but not committed yet.
    May not work on Windows, since npm runs scripts in `cmd.exe` there. Workaround: `npm config set script-shell "C:\\Program Files\\Git\\bin\\bash.exe"` (requires [Git for Windows](https://gitforwindows.org/)), or use WSL.
  - `test:spelling-only-staged`: Same as `test:spelling`, but only for files staged with `git add`
  - `test:spelling-only-last-commit`: Same as `test:spelling`, but only for files changed in the last commit
- dependencies
  - `hugo-extended`: Static site generator that turns Markdown and shortcodes into HTML
- devDependencies
  - [`@axe-core/playwright`](https://npmjs.com/package/@axe-core/playwright): A11y tester bindings for Playwright
  - [`@playwright/test`](https://npmjs.com/package/@playwright/test): Browser automation and test library
  - [`cspell`](https://npmjs.com/package/cspell): Spell-checker
  - linkinator: Link-checker (WIP)
  - prettier: Formatter (exact version pinned per [official recommendation](https://prettier.io/docs/next/install#summary))
  - [`sort-package-json`](https://npmjs.com/package/sort-package-json): Sorts package.json files for consistency
  - [`start-server-and-test`](https://npmjs.com/package/start-server-and-test): Allows easy setup and teardown of complex tests, like our a11y tests
