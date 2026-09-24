# Contributing to Gitling

Thanks for your interest in improving Gitling. Bug reports, fixes, features, and translations are
all welcome.

> **Important:** all work happens on the **`develop`** branch. Branch off `develop` and open your
> pull request against `develop`, not `master`. `master` only receives merges from `develop` when
> a release is cut. Because `master` is the repo's default branch, GitHub will pick it
> automatically when you fork or open a PR, so double-check the base branch.

## Reporting bugs and requesting features

Open an issue using the [bug report](https://github.com/maneeshacooray/Gitling/issues/new?template=bug_report.md)
or [feature request](https://github.com/maneeshacooray/Gitling/issues/new?template=feature_request.md)
template. For bugs, the most useful things to include are:

- Steps to reproduce
- Your Android version and device
- The Gitling version (shown in Settings)
- The error message or stack trace, if there was one

Please search existing issues first to avoid duplicates.

## Development setup

- **Android Studio** (a recent stable release) with the Android SDK for API 37.
- **JDK 21.** The Gradle toolchain is pinned to 21 to match F-Droid's reproducible-build
  environment. If your default JDK is a different version, point `JAVA_HOME` at a JDK 21 install
  rather than changing the toolchain setting.

Build a debug APK and run the unit tests (this is what CI runs):

```bash
./gradlew --no-daemon clean assembleDebug testDebug
```

Run a single test class:

```bash
./gradlew testDebug --tests "me.sheimi.sgit.database.models.RepoTest"
```

The app supports Android 6.0 (API 23) and up, so keep older API levels in mind. Several past bugs
came from Java APIs that exist on newer Android versions but not older ones.

## Finding your way around the code

The app is partway through a migration from a legacy Java/XML codebase to Kotlin and Jetpack
Compose, and both live side by side:

- `app/src/main/java/me/sheimi/` is the legacy layer: Activities, Fragments, the `Repo` model,
  and one task class per git operation (`PullTask`, `RebaseTask`, and so on). This is where the
  JGit calls happen.
- `app/src/main/java/com/manichord/mgit/` is the newer layer: Compose screens, theming, and
  ViewModels that wrap the legacy model and tasks.

New UI should be written in Compose. [`docs/agents/architecture.md`](docs/agents/architecture.md)
explains how the two layers fit together, including the `FragmentHost` bridge and how the commit
graph is rendered. It's worth reading before making larger changes.

## Making a pull request

1. Fork the repo and create a branch from **`develop`** (not `master`), for example
   `fix/crash-on-empty-repo` or `feat/rename-file`:

   ```bash
   git remote add upstream https://github.com/maneeshacooray/Gitling.git   # once
   git fetch upstream
   git checkout -b fix/my-fix upstream/develop
   ```
2. Make your change. Keep each PR focused on one fix or feature.
3. Build and run the unit tests (see above).
4. Test it on a real device or emulator, not just a successful compile.
5. Open the PR against **`develop`**, not `master`. On GitHub's "Open a pull request" page, change
   the **base** dropdown from `master` to `develop`. Then fill in the PR template.

A few details:

- **Screenshots are optional but appreciated.** If your change affects anything visible, a
  screenshot or screen recording of it working makes review much quicker.
- **Commit messages** follow a [Conventional Commits](https://www.conventionalcommits.org/) style,
  such as `fix: ...`, `feat(ui): ...`, or `docs: ...`. Explain *why* in the commit body when
  the change isn't obvious.
- **Don't bump the version** or edit `CHANGELOG.md`, the What's New entries, or the fastlane
  changelogs. The maintainer handles these when cutting a release.
- If your PR fixes an issue, reference it in the description (for example, `Closes #12`).

## Translations

Gitling's UI strings live in `app/src/main/res/values/` (English). Translations go in the matching
`values-<language>/` folder, for example `values-de/` for German. Store listing text for F-Droid
and other stores lives in `fastlane/metadata/android/<locale>/`.

To add or improve a translation, copy the English string files you're translating into the right
folder, translate the values (not the `name` attributes), and open a PR as described above.

## License

Gitling is licensed under the [GPLv3](LICENSE). By contributing, you agree that your
contributions are licensed under the same terms.
