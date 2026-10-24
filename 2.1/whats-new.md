# What is new in 2.1

The headline is that TextMate 2.1 is TextMate 2.0 carried forward by a new project, built for the current macOS, with the pieces around the editor made new. The editor itself, the text view, the grammars, the scopes, is the one you know.

## Its own identity

TextMate 2.1 is `com.textmate3.TextMate`, with its own preferences, its own Application Support directory and its own caches. It can be installed beside TextMate 2 and the two never touch. The first launch imports your preferences, your installed bundles and your session from TextMate 2, once, and leaves TextMate 2 as it was.

## Runtimes that arrive on their own

Bundle commands run on a Ruby the release pins, 4.0.9, and on a Python the release pins, 3.14.9, found on your machine or installed for you through the runtime managers the Runtimes bundles carry, rv for Ruby and uv for Python. The system's copies are never used. When the application installs a runtime at launch, stands in with another version, or cannot find one, a strip above the text in every document window says so, until you dismiss it. [Runtimes](runtimes.md) has the whole arrangement.

## The About window

Every page of the About window is native: the release notes, the legal page, the recent changes to your installed bundles, and the contributions page listing every commit to the application's repository by everyone who made one, with the author linked to their GitHub profile and each commit to GitHub.

## Preferences

Every pane is new: Files, Projects, Bundles, Variables, Updates and Terminal. The Bundles pane is a sortable table of the catalog with a category bar and search, a check box per bundle that installs or uninstalls, and a spinner while an install is on its way. The Updates pane offers the update channel.

## Update channels

Every update is on one of three channels. Release reaches everyone. Beta is the nightlies, for anyone who wants to live on the edge. Alpha is for the development team. The Updates pane picks the channel.

## The bundle editor edits forms

Grammars, themes, preferences and macros are edited as forms rather than as property list text. A grammar shows its rules with the issues a validator finds, a theme shows the page, the gutter and every scoped entry with a live preview over a sample in a grammar you choose, a preferences item shows every setting the application reads grouped as the application groups them, and a macro shows its steps in play order with reorder and remove.

## Signed, notarized, and updated

Releases are Developer ID signed and notarized, and update through Sparkle from a feed the application verifies. Bundles install from a catalog whose index and every archive are signed with a key compiled into the application, so a modified bundle fails to install rather than installing.

## The privileged helper

Writing a file the application may not, one owned by root for instance, goes through a helper the system registers the first time it is needed, through the mechanism macOS provides for it, rather than through an installer run at launch.

## Smaller things worth knowing

- Choosers remember a pick as soon as it is made, so the abbreviation memory survives a crash and does not grow without limit while the application runs.
- Find in Project on a selection that holds a folder and something inside it searches the folder, not its parent.
- A bundle command whose shebang names the system Ruby by its old framework path runs on the same Ruby every other Ruby command does.
- A `TM_RUBY` naming a Ruby that is not there no longer breaks every Ruby command. The application refuses it, says why, and runs the command on the Ruby it pins.
