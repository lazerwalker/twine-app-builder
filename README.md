# Twine Wrapper

![Build and Test](https://github.com/lazerwalker/twine-electron-test/actions/workflows/main.yml/badge.svg)

This is a project to take your existing HTML5 game and automatically generate a desktop version for Windows and macOS. It works with any game whose output is HTML/JavaScript/CSS.

The primary intention of this tool is to take games made in browser-focused tools like [Twine](https://twinery.org) (both 1 and 2), [Bitsy](http://www.bitsy.org/), and [PuzzleScript](https://www.puzzlescript.net/) and produce desktop builds suitable for distribution on platforms such as Steam or Itch, but you may find other uses for it as well!

To use this, you will need basic familiarity with git and GitHub. No other technical expertise is needed beyond whatever you need to make your game!

## How to Use

There are more detailed instructions below, but here's what the high-level flow looks like for using this project:

1. You fork this git repo, and add your browser game to it (an `index.html` page and maybe some extra files like images or audio)
2. When you commit those changes to git and push them to GitHub, GitHub will automatically take your game files and bundle them up into downloadable desktop binaries
3. The "Releases" section of your project's GitHub (https://github.com/username/repo/releases) will now contain downloadable Windows and Mac versions of your game
4. Any time you add a new git tag to your repo, this process will repeat and new binaries will be auto-generated!

## Getting Started

1. Fork this repo. Assuming you're reading this README on GitHub, click the "Fork" button in the top-right of this repo
1. Move your game files into your forked repo. Put anything you'll need into the `src` folder. This must include an `index.html` file, which will be loaded in a custom web browser whenever players open your game, but might also include other resources like images or audio.
1. In your new repo, there will be a file in the `.github/workflows` subfolder called `main.yml`. Down around line 89, in the "Build the app" section, change the `APP_NAME` variable from "My Twine Game" to whatever you want your app to be called.
1. If you have a custom app icon you'd like to use, put that as `icon.png` in the root of the repo. It will be automatically resized as long as it is square and at least 1024x1024.
1. Commit those changes to your git repo
1. Create a new git tag by running `git tag [tag-name]` with whatever you would like the new version number for your build to be. Your version number must start with the character "v". This is intended to be used with numeric version numbers (e.g. `v1` or `v2.10.3`), but other than the 'v' restriction you can use whatever versioning scheme you would like.
1. Push both your changes and your new git tag (via `git push --tags`) to GitHub
1. Wait a few minutes! You can go to the "Actions" tab in your GitHub repo to see build progress.
1. When the build is done, the "Release" tab in your repo will contain download links.

As you make changes to your game, repeat the last few steps. Every new git commit that you make and push up to GitHub will result in a new build of your game.

## Advanced Features

### Customization

This project provides a sensible set of default options in the generated app, but you might want to customize it! If you want to add deeper integration with OS-level features, or make configuration changes to things such as the menu bar, you can modify the Electron app template being used to generate the app. This will require knowledge of JavaScript and Electron.

When GitHub Actions builds your app, it fetches a simple wrapper Electron app located at https://github.com/lazerwalker/electron-wrapper-template. You can fork that template, make any changes you want, and then update your GitHub Actions workflow to point to your own fork in the ["repository"]() key of the "Check out Electron app template" step.

### Windows signing

Signing your Windows app removes the "untrusted publisher" warning message that Windows may show upon running your game. If you're primarily publishing on Steam, this may not be necessary, as this warning does not show up for games launched via the Steam launcher. Signing your app for Windows requires purchasing a developer certificate.

**Warning: This has not been tested and may not work as-is**

1. Go through the process of [creating a certificate for package signing](https://docs.microsoft.com/en-us/windows/msix/package/create-certificate-package-signing?WT.mt_id=spatial-0000-emwalkerspatial-8466-emwalker).
1. Once you've done this and have a valid PFX file, base64 encode it. You can do this in PowerShell by using the command `certutil -encode infile outfile`.
1. Open up your GitHub repo's Action Secrets (Settings -> Secrets), and create two "Repository secrets". `CERTIFICATE_WINDOWS_PFX` should contain the base64-encoded contents of your PFX file, and `WINDOWS_PFX_PASSWORD` should contain the password.

### macOS Signing and Notarization

Signing and notarizing your macOS app will avoid warnings that your app is unsigned, which may require users to go into their security settings to allow it to run. At some point, Apple may _require_ all apps to be notarized, but that has not yet happened as of the writing of this README. Signing and notarizong your app requires a paid Apple developer account.

**Warning: This has not been tested and may not work as-is**

To notarize your app, set up two repository secrets (from your fork's repo page, Settings -> Secrets -> New repository secret) called `APPLE_ID` and `APPLE_ID_PASSWORD` containing the username and password (respectively) to an Apple ID that has the ability to notarize apps. You may want to create a dedicated free Apple developer user that has been granted access to your paid account instead of storing your personal Apple ID credentials.

Proper codesigning (instead of merely notarization) should be supported as well, but more work needs to be done here.

## How does this project work?

Under the hood, project relies on two core pieces of technology: [GitHub Actions](https://github.com/features/actions) and [Electron](https://www.electronjs.org/).

- GitHub Actions is a free service integrated with GitHub that can run little bits of code on cloud servers whenever you do things like push new code to GitHub.
- Electron is an open-source tool that lets you build desktop apps using browser technologies like HTML, JavaScript, and CSS

I maintain a GitHub repo that contains a minimal scaffolding project built on Electron. When new code is pushed in your repo, a GitHub Action runs that grabs your HTML files, injects them into that scaffolding project, and builds the project for you using Electron tools.

## "Why don't you support [insert feature here]?"

Open a GitHub Issue in this repo!

This project is an experiment, so I've intentionally kept the initial release very minimal. If people are actually using this, I'd love to expand on it! Let me know if you're using this and there's something you're dying to see added, or if there's some missing feature preventing you from using this, so I can prioritize improvements! Some specific things I'm currently thinking about:

- More customization options that don't require forking the Electron template
- Linux build support
- iOS and Android build support
- Integration with store platforms (e.g. Itch.io, Steam) to auto-upload new builds via GH Actions
- Supporting automatic updates (i.e. push out new versions of your game without requiring players to download new binaries)

## License

This project is licensed under the MIT license. See the `LICENSE` file in this repo for more info.
