# What is new in 2.1

The _tl;dr_ is that TextMate 2.1 is a fork of TextMate 2.0 that has updated for current macOS and Apple silicon (M-series or A-series chips).

It's a new project and people. It's still TextMate at the core, namely text editor. But with the pieces around the editor made new. The editor itself, grammars, scopes, themes… are all largely unchanged.

## Its own identity in macOS

TextMate 2.0 and lower are `com.macromates.TextMate`.
TextMate 2.1 is `com.textmate3.TextMate`.
Even though it's mostly the same app to you, it's a separate app to macOS' eyes.

TextMate 2.1 has its own preferences, `Application Support` directory and caches. TextMate 2.1 can be installed beside TextMate 2.0 and the two never touch. The first launch of TextMate 2.1 imports your preferences, installed bundles, and session from TextMate 2.0. But only once. It leaves TextMate 2.0 as it was.

## Runtimes that arrive on their own

The app pins specific versions of Ruby and Python that get used by bundle commands. Ruby: 4.0.9. Python: 3.14.9. TextMate tries to find any already intalled `rv` or `uv` as the language version managers, and for installed versions of Ruby and Python. If any of the installed versions of `rv`, `uv`, Ruby, or Python what the apps wants, the app will use yours. If not, the app will download and install the versions it needs. TextMate 2.1 never ever uses system Ruby or system Python. Both are either don't come with macOS anymore or are way to old to be usable. A banner above the editor will notify you of any version mismatche problems. [Runtimes](runtimes.md) has the whole arrangement.

## SwiftUI

Many parts of the non-editor parts of the app are rewritten in SwiftUI. About, Preferences, Bundle Editor, Commit Message, Runner Output, et al. The core text editor view is not changed.

## The About window

The About window is native SwiftUI not a WebKit view of an HTML file. Release notes, legal acknowledgements, recent changes to your installed bundles, and contributions listing every commit to the application's repository.

## Settings / Preferences

Preferences is now called Settings in macOS. The Settings window is all new SwiftUI, but all of the same tabs as before: Files, Projects, Bundles, Variables, Updates and Terminal. And a new one: Runtimes.

## Update channels

The app's software updates are available in three ways: alpha, beta, release.

Alpha is mostly meant for people doing active development. Beta is meant for anyone who wants to live on the edge. Release is for everyone. Release is proper official releases. You can set which one you want in the Updates tab in Settings.

## The bundle editor edits forms

Grammars, themes, preferences and macros are edited as forms rather than as property list text. A grammar shows its rules with the issues a validator finds. A theme shows the page, the gutter and every scoped entry with a live preview over a sample in a grammar you choose. A preferences item shows every setting the application reads grouped as the application groups them. A macro shows its steps in play order with reorder and remove.

## Signed, notarized, and updated

Releases are Developer ID signed and notarized. Software updates are built and managed with [Sparkle](https://sparkle-project.org). Updates via Sparkle from the `appcast.xml` feed are verified.

Bundles install from a catalog whose index and every archive are signed with a key compiled into the application. A modified bundle fails to install rather than installing.

## The privileged helper

Writing files that are not owned by the user or that the application has permissions to (for example, files owned by root), now goes through a helper. The helper is registered by the system the first time it is needed through the mechanism that macOS provides for it, instead of through an installer run at launch.

## Smaller things worth knowing

- Choosers remember a pick as soon as it is made, so the abbreviation memory survives a crash and does not grow without limit while the application runs.
- `Find in Project` on a selection that holds a folder and something inside it searches the folder, not its parent.
- A bundle command whose shebang names the system Ruby by its old framework path runs on the same Ruby every other Ruby command does.
- A `TM_RUBY` set to a Ruby that is not there no longer breaks every Ruby command. The application rejects it, alerts you, and runs the command on the Ruby version pinned in the app.
- ...
