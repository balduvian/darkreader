<h2 align="center">Contributions</h2>

You can contribute to and help Dark Reader's active open development in many ways. We provide this document in order to make it as east as possible to contribute. Read on below to learn how and thank you in advance. All contributors must adhere to Dark Reader's [code of conduct](https://github.com/darkreader/darkreader/blob/main/CODE_OF_CONDUCT.md).

## Sponsor

<a href="https://opencollective.com/darkreader/donate" target="_blank" rel="noreferrer noopener"> <img src="https://opencollective.com/darkreader/donate/button@2x.png?color=blue" width=300 /></a>

Sponsor the development of Dark Reader.

## Translation

[Improve or suggest](https://github.com/darkreader/darkreader/tree/main/src/_locales) a translation. See the list of [language codes](https://developer.chrome.com/webstore/i18n#localeTable) that we can support.

## Disabling Dark Reader on your site

Website pages can request Dark Reader to disable itself by embedding a "Dark Reader lock". The "lock" is a `<meta>` tag with `name` attribute set to `darkreader-lock` which is a child of `<head>` tag in the document.

### Disabling Dark Reader statically

Add `<meta name="darkreader-lock">` within your HTML document in `<head>` like so:
```html
<head>
    <meta name="darkreader-lock">
</head>
```

### Disabling Dark Reader dynamically

Add the "lock" dynamically like so (assuming browser already parsed enough of the document to create a `head` attribute):
```js
const lock = document.createElement('meta');
lock.name = 'darkreader-lock';
document.head.appendChild(lock);
```

## Adding a website that is already dark

If a website is **already dark** and meets the following requirements:

- The entire website, including all subpages, is dark by default, regardless of the system's preferred color scheme.
- The URL is the actual website address. No redirects of any kind are allowed.
- The website is complete and finished. Any website in the design or development phase or any other incomplete status is not permitted. These statuses can include any placeholder web pages or verbiage about coming soon, the website being under construction, the website having moved, etc.

Then you can **add it to the [dark-sites.config](https://github.com/darkreader/darkreader/blob/main/src/config/dark-sites.config) file**.

**Please maintain the alphabetical order of the websites listed in this file.**

## Fixing incorrect inversions

If any **element** on a web page is **not inverted or styled correctly**, you can fix it by specifying the appropriate [**CSS selector**](https://developer.mozilla.org/docs/Web/CSS/CSS_Selectors). Use the [**dynamic-theme-fixes.config**](https://github.com/darkreader/darkreader/blob/main/src/config/dynamic-theme-fixes.config) file for Dynamic Theme mode and the [**inversion-fixes.config**](https://github.com/darkreader/darkreader/blob/main/src/config/inversion-fixes.config) file for Filter and Filter+ modes.

**Please maintain the alphabetical order of the websites listed, use short selectors, and preserve the code style in these files.**

You can learn how to create a fix for the appropriate mode below.

## How to use the Dev Tools

The Dev Tools help you **fix minor issues** on a web page. These can include a dark icon on a dark background, removing a bright background, adding a white background to a transparent image, etc.

In **Dynamic mode**, if the page looks partially dark and bright, it is considered a bug.

In **Filter mode**, it is a common practice to invert elements on the page that are already dark.

- Open **Chrome Dev Tools** (`F12`) in Chrome or "Inspector" (`Ctrl+Shift+C`) in Firefox.
- Click on **element picker** (top-left corner). It is enabled automatically in Firefox.
- Pick an incorrectly inverted element.
- Choose a **[selector](https://developer.mozilla.org/docs/Web/CSS/CSS_Selectors)** for that element or all similar elements (for example, if it has `class="icon small"`, the selector may look like `.icon`).
- Click **Dark Reader icon**.
- Click **Open developer tools** (at the bottom of the Dark Reader popup).
- Edit or add a block containing the URL and selectors to invert.
- Click **Apply**.
- Check how it looks both in **Light** and **Dark** mode.
- If the **fix works** open **[dynamic-theme-fixes.config](https://github.com/darkreader/darkreader/blob/main/src/config/dynamic-theme-fixes.config) file** or **[inversion-fixes.config](https://github.com/darkreader/darkreader/blob/main/src/config/inversion-fixes.config) file**.
- Click **Edit** (login to GitHub first).
- **Insert your fix** there. Preserve **alphabetic order** by URL.
- Provide a **short description** of what you have done.
- Click **Propose file change**.
- Review your changes. Click **Create pull request**.
- GitHub actions will run tests to make sure it has the right code style.
- If you see a **red cross**, click **Details** to see what is wrong and edit the existing Pull Request.
- When you see a **green checkmark** then everything is fine.
- A Dark Reader developer will **review** and merge your changes, making them available.

```CSS
dynamic-theme-fixes.config
================================

example.com

INVERT
.icon

CSS
.wrong-element-colors {
    background-color: ${white} !important;
    color: ${black} !important;
}

IGNORE INLINE STYLE
.color-picker

IGNORE IMAGE ANALYSIS
.logo
```

| Rule | Description | Notes / Examples |
|---|---|---|
| **INVERT** | Inverts specified elements. | **Dynamic Mode**: INVERT only for dark images that are invisible on dark backgrounds. |
| **CSS** | Adds custom CSS to a web page. | `!important` keyword should be specified for each CSS property to prevent overrides by other stylesheets.<br>**Dynamic mode** supports `${COLOR}` template, where `COLOR` is a color value before the inversion. <br>*Example*: `${white}` will become `${black}` in dark mode. |
| **IGNORE&nbsp;INLINE&nbsp;STYLE** | Prevents inline style analysis of matched elements. | *Example*: `<p style="color: red">` element's style attribute will not be changed. |
| **IGNORE&nbsp;IMAGE&nbsp;ANALYSIS** | Prevents background images from being analyzed for matched selectors. |  |

## Adding a new color scheme

If you can add a new _popular_ or _unique_ but usable predefined color scheme to Dark Reader, you can add it to the `src/config/color-schemes.drconf` file. Please use the following steps to add a new color scheme:

- Open **[color-schemes.drconf](https://github.com/darkreader/darkreader/blob/main/src/config/color-schemes.drconf) file**.
- Click **Edit** (login to GitHub first).
- **Insert your fix** there. Preserve **alphabetic order** by Color scheme name.
- Provide a **short description** of what you have done.
- Click **Propose file change**.
- Review your changes. Click **Create pull request**.
- GitHub actions will run tests to make sure it has the right code style.
- If you see a **red cross**, click **Details** to see what is wrong and edit the existing Pull Request.
- When you see a **green checkmark** then everything is fine.
- A Dark Reader developer will **review** and merge your changes, making them available.

## Dynamic variables

When making a fix for background or text colors, instead of using hardcoded colors (like `#fff`, `#000`, `black` or `white`), please use CSS variables that are generated based on the user settings:

```CSS
dynamic-theme-fixes.config
================================
example.com

CSS
.logo {
    background-color: var(--darkreader-neutral-background) !important;
}
.footer > p {
    color: var(--darkreader-neutral-text) !important;
}

```

Here is a full table of available CSS variables:

| Variable | Description | Use |
|---|---|---|
| **`--darkreader-neutral-background`** | Neutral background color that <br>corresponds to the user's settings. | Mostly used for elements that have <br>the wrong background color. |
| **`--darkreader-neutral-text`** | Neutral text color that <br>corresponds to the user's settings. | Used for elements with the wrong text color. |
| **`--darkreader-selection-background`** | The background color setting <br>defined by the user. | The user's Background Color setting. |
| **`--darkreader-selection-text`** | The text color setting <br>defined by the user. | The user's Text Color setting. |

## Fixes for Filter and Filter+ mode

```CSS
inversion-fixes.config
================================

example.com

INVERT
.icon
.button
#player

NO INVERT
#player *

REMOVE BG
.bg-photo

CSS
.overlay {
    background: rgba(255, 255, 255, 0.5);
}
```

- Filter and Filter+ work by inverting the entire web page and reverting necessary elements (images, videos, etc.) listed in the `INVERT` section.
- If an inverted element contains images or other content that becomes incorrectly displayed, use the `NO INVERT` rule.
- `REMOVE BG` removes the background image from an element and forces a black background.

## Adding new features or fixing bugs

### Requesting a Feature

If you have an idea for a new Dark Reader feature, first check to make sure it has not already been requested in [GitHub issues](https://github.com/darkreader/darkreader/issues).
Before spending effort on making a change to implement the feature, please submit the feature request as an issue so that it can be discusses with active contributors and you are given approval to begin.

### Submitting a Bug Report 

If you find a bug in Dark Reader, first check to make sure that it has not already been reported asn an [issue on GitHub](https://github.com/darkreader/darkreader/issues). If not, please use the Bug Report template for your issue.
Provide steps to reproduce the bug and make sure that the bug can be reproduced in a new unmodified web browser profile with Dark Reader installed as the only extension.

### Creating a Pull Request

You can use any text editor or web IDE (for example, [Visual Studio Code](https://code.visualstudio.com/) or [WebStorm](https://www.jetbrains.com/webstorm/)) for editing the code.
After you have been given approval to implement a feature or fix a bug, follow these steps to create a pull request:

1. Create a fork of the repo on your personal GitHub
2. Add code that implements your fix
4. Launch the extension in your own browser to test your changes (See Local Setup section below)
3. Run local tests with `npm test` and verify that they all pass
4. Format your code with `npm run code-style` (see Code Style section below)
5. Make commits and push to your fork
6. Verify that CI tests pass on your fork (See CI section below)
7. Create a pull request on your fork targeting the `darkreader:main` branch

Make sure that in your pull request you mention the issue that it is related to. If you were communicating with a specific active contributor in the issue discussion before starting the pull request, please also mention that user. 

After you create your pull request, it may be approved and merged into the repo, have changes requested, or be rejected with explanation. Please monitor that status of your pull request so that you can iterate upon it if requested.
You are encouraged to continue to monitor the status of your pull request and original issue even after your pull request is merged until the next minor release.
In the case that other contributors make suggestions or find bugs in your code you should be prepared to iterate.

### Local Setup

To build and debug the extension **install the [Node.js](https://nodejs.org/)** LTS version.
Install development dependencies by running `npm install` in the project root folder.
Then execute `npm run debug`.

##### Chrome and Edge

- Open the `chrome://extensions` page.
- Disable the official Dark Reader version.
- Enable the **Developer mode**.
- Click **Load unpacked extension** button.
- Navigate to the project's `build/debug/chrome` folder.

##### Firefox

- Open the `about:addons` page.
- Disable the official Dark Reader version.
- Open the `about:debugging#addons` page.
- Click the **Load Temporary Add-on** button.
- Open the `build/debug/firefox/manifest.json` file.

If you execute `npm run debug:watch` instead of `npm run debug`, it will automatically recompile after making any code changes.

### Active Contributors

Dark Reader has a team of active contributors who manage approving issues and pull requests. Dark Reader uses [Github Discussions](https://github.com/darkreader/darkreader/discussions) for communicating with active contributors.

### Branch Organization

All development on Dark Reader is done directly on the branch `main`. All code submitted to the `main` branch must be non-breaking and with all tests passing. `main` should be in a state where it can always be released into the next minor version of Dark Reader. 

### Releases

Dark Reader releases use [semantic versioning](https://semver.org/). Minor releases occur roughly once a month. Each release comes with changes documented in the [CHANGELOG.md](https://github.com/darkreader/darkreader/blob/main/CHANGELOG.md). As of version 4.9, bug fixes, website improvements, and minor features are introduced as a patch increment.

### Technologies

Dark Reader uses [TypeScript](https://www.typescriptlang.org/) for the api, extension scripts, and extension ui. The extension popup ui uses [React](https://react.dev/) and [Less](https://lesscss.org/) for styling. It is build using [Node.js](https://nodejs.org/).

### Code Style

**Please preserve the code style** (for example, whitespaces, etc.). Code style is enforced automatically through [eslint](https://eslint.org/) it can be executed by running `npm run code-style`. For a full technical description of code style, see [.eslint.js](https://github.com/darkreader/darkreader/blob/main/.eslintrc.js) and [.eslintplugin.js](https://github.com/darkreader/darkreader/blob/main/.eslintplugin.js).  

### CI

This repo and thus any forks you create will have ontinuous integration tests through Github Actions that run on pull request or push. In addition to local testing with `npm test`, please make sure that all CI tests pass on your fork before proceeding with creating a pull request.
CI tests are configured in `.github/workflows`.

## Attribution

Part of this contributing document has been adapted from the [React](https://github.com/facebook/react/blob/main/CONTRIBUTING.md) and [ZSTD](https://github.com/facebook/zstd/blob/dev/CONTRIBUTING.md) repo contributing documents.