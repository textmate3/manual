# Opening Files {#opening-files}

TextMate supports the normal ways of opening documents, that is, dragging them to the TextMate application icon or opening document types set to open with TextMate.

But because opening documents is such a frequent action, and Finder is not always the most efficient interface for this, there is a plethora of other ways to open documents which will be described in this chapter.

Last section will explain how to open binary files, such as gzipped documents or binary property lists.

## Folders as Projects {#folders-as-projects}

When working with multiple related documents, things will be a lot easier if you start by opening the containing folder.

In this manual we commonly refer to such enclosing folder as the projet folder, or simply a project. Next chapter has more info about working with projects.

The project folder can be opened via the usual means, e.g. *File → Open…* (`⌘O`), executing `mate .` in your terminal (to open current folder as a project), dragging a folder to the TextMate application icon, etc.

However, you may find that your project folders are not getting added to the *File → Open Recent* submenu. Instead you can use *File → Open Recent Project…* (`⇧⌘O`).

This will show you a list of recent folders that has been opened in TextMate where you can quickly select what to open by using the normal navigation keys or start typing to filter the list. The dialog supports multi-selection (either by shift-clicking items or holding shift when pressing arrow up/down).

Should the list contain unwanted items, these can be removed by either pressing delete (`⌫`) or clicking the remove button.

The dialog also has a section for favorites. You can navigate between sections using *Window → Select Next Tab* (`⌘}`) and *Window → Select Previous Tab* (`⌘{`), or you can use the mouse to click the desired section label.

The favorites section show the contents of `~/Library/Application Support/TextMate/Favorites` where it looks for symbolic links to your most used documents and folders.

Such links can be created from the [file browser](projects.html#file-browser)’s action menu or manually in a terminal.

It is possible to add the contents of a folder to your favorites, instead of the folder itself.

For example if you keep all your projects under `~/projects` then you can create a symbolic link in a terminal with `[DIR]` as prefix like this:

```
ln -s ~/projects "$HOME/Library/Application Support/TextMate/Favorites/[DIR] Projects"
```

After this, you should see a list of your projects when selecting Favorites.

## Open Quickly {#open-quickly}

The easiest and most powerful way to open and switch between documents is by using *File → Open Quickly…* (`⌘T`).

The list of documents in this window is sorted according to last use, with the most recently used (excluding current document) at the top, meaning that pressing return (`↩`) is a useful shortcut for “switch back to previous document”.

### Filter String {#filter-string}

The list can be filtered by entering a filter string. This string will try to match abbreviations as well as subsets, and will rank matches according to how “good” a match they are. For example a filter string of `otv` will both match `OakTabBarView.mm` and `OakTextView.mm`, but since the latter has each of its capitals matched, this one is considered the better match. Likewise a filter string of `text` matches both `test-fixtures` and `text-view`, but for the latter, it fully matches a single subset and is even matching from the beginning of the name, so this one is the better match.

By default the filter string is applied to the full path, for example `srcotv` will match `/path/to/src/OakTextView.mm`, but you can include slashes (`/`) in the filter string to make this more explicit.

If the filter string contains an asterisk (`*`) then it is treated as a [glob string](references.html#glob-string) instead. For example `*.md` will match all documents with a `.md` extension.

If the filter string contains a colon (`:`) and what comes after the colon is a valid [selection string](references.html#selection-string) then when selecting a file, TextMate will also jump to the location described by this selection string.

For example `otv:32` will jump to line 32 of the selected document.

When you open the Open Quickly dialog then TextMate will check if your search clipboard contains a string of the form `«file»:«line»`. If it does, this is used as the default filter string.

The use case for this is when you see output in your terminal of the form:

```
src/OakTextView.mm:325: error: use of undeclared identifier 'foo'
```

If you want to jump to this error in TextMate, select the filename including line number and invoke *Edit → Find → Use Selection for Find* (`⌘E`) (an action found in majority of macOS applications). Now switch to TextMate and select *File → Open Quickly…* (`⌘T`) followed by return (`↩`).

### Keyboard Shortcuts {#keyboard-shortcuts}

Pressing option-return (`⌥↩`) to open a document will make TextMate close all other (non-sticky) documents first.

Holding down shift when using arrow keys will allow you to select multiple documents.

Switching between sources (All, Open Files, and Uncommitted Changes) can be done using the actions in the Windows menu for switching tabs.

That is `⌘1`-`⌘n` to select the n’th source, or `⌘{` and `⌘}` to selection previous/next source.

While the dialog is showing, it is possible to use *File Browser → Enclosing Folder* (`⌘↑`) to get a listing for the parent folder.

### Customization {#customization}

The following keys can be set via the [properties system]():

* `followSymbolicLinks`: By default symbolic links to folders are not followed. If this key is set to `true` then you will see the contents of these folders.

In addition to the general [filtering settings]() you can set these keys to specifically target the Open Quickly dialog:

* `includeInFileChooser`
* `excludeInFileChooser`
* `includeFilesInFileChooser`
* `excludeFilesInFileChooser`
* `excludeDirectoriesInFileChooser`

## Related Files {#related-files}

In some languages it is common to have a header and implementation file sharing the same base name, for example `foo.c` and `foo.h`.

TextMate allows you to cycle through files with the same base name using *Navigation → Go to Related File* (`⌥⌘↑`).

In languages that does not work with headers, you may want to repurpose this feature for switching between a controller and its view, or an implementation file and its test file.

This can be done by setting the `relatedFilePath` [properties key]() to the file that is related to the current one.

For example in [Go](https://golang.org/) the convention is to append `_test` to the basename for test files, which means for regular files (`*.go`) we want to add `_test`, and for test files (`*_test.go`), we want to remove it.

This can be achieved with the following setting:

```
[ *.go ]
relatedFilePath = "${TM_FILEPATH/(_test)?\.go$/${1:?:_test}.go/}"
```

This setting targets both test and non-test files, then we set `relatedFilePath` to the current file path with a substitution that optionally matches the `_test` part, and if it was matched, we replace it with nothing, otherwise we insert it.

A more explicit setting could be the following:

```
[ *.go~*_test.go ]
relatedFilePath = "${TM_FILEPATH/\.go$/_test.go/}"

[ *_test.go ]
relatedFilePath = "${TM_FILEPATH/_test\.go$/.go/}"
```

The first glob uses `~` to exclude test files from matching.

## `mate` {#mate}

TextMate comes with its own `mate` shell command, which can be installed from the Terminal page in Preferences.

After installation you can run `mate -h` in a terminal which should show the version number and the following usage instructions:

```
mate 2.12 (2016-12-12)
Usage: mate [-wl<selection>t<filetype>m<name>rehv] [-u<identifier> | file ...]
       mate [-c<mark>] -s<mark>:<value> -l<line> [-u<identifier> | file ...]
       mate -c<mark> [-l<line>] [-u<identifier> | file ...]

Options:
 -w, --[no-]wait                 Wait for file to be closed by TextMate.
 -l, --line <selection>          Setup <selection> after loading file.
 -t, --type <filetype>           Treat file as having <filetype>.
 -m, --name <name>               The display name shown in TextMate.
 -r, --[no-]recent               Add file to Open Recent menu.
 -u, --uuid <identifier>         Reference already open document with
                                 <identifier>.
 -e, --[no-]escapes              Set this to preserve ANSI escapes from stdin.
 -s, --set-mark <mark>[:<value>] Set a mark with optional <value> (requires --line).
 -c, --clear-mark <mark>         Clear a mark (clears all marks without --line).
 -h, --help                      Show this information.
 -v, --version                   Print version information.

Files opened via mate are added to the recent menu unless
the file starts with a period, --wait or --no-recent is
specified, or the file is in the system’s temporary directory.

By default mate will wait for files to be closed if the command name
has a "_wait" suffix (e.g. via a symbolic link) or when used as a
filter like in this examples:

    ls *.tex|mate|sh    -w implied
    mate -|cat -n       -w implied (read from stdin)

The -l/--line option requires a selection in the following format:

    selection    = <range> ('&' <range>)*
    range        = <pos> | <normal_range> | <column_range>
    pos          = <line> (':' <column>)? ('+' <offset>)?
    normal_range = <pos> '-' <pos>
    column_range = <pos> 'x' <pos>
    line         = [1-9][0-9]*
    column       = [1-9][0-9]*
    offset       = [1-9][0-9]*
```

### Editing With `sudo` {#editing-with-sudo}

The `mate` shell command supports being called via `sudo` and will pass the authorization to TextMate.

So if you run `mate /etc/hosts` and save, TextMate will ask you for admin credentials.

But if you instead run `sudo mate /etc/hosts` you will be asked for password in the terminal, and TextMate will be able to save your file without prompting you for admin credentials.

Which you prefer is a matter of personal preference, the main advantage of `sudo` is that (depending on your settings) the authorization is cached for your current user, so after entering your password, you can continue to use `sudo` to edit files without needing to re-type your password, where TextMate will associate the authorization with the current document, and will drop it once the document is closed.

### Setting and Clearing Marks {#setting-and-clearing-marks}

In addition to opening files, the `mate` command allows you to set and clear gutter marks.

Such marks can be used to decorate lines with warning or error icons, indicate that a line has been changed since last commit, the mark can be a color square showing what color is defined in the line’s CSS, etc.

To set a mark you call `mate --set-mark <mark>[:<value>]` where `<mark>` should be a path to the image you want to use for the mark.

If the mark’s basename ends with `Template` then it is treated as a monochrome image that is rendered in the proper theme color.

If you provide `<value>` then this text is shown to the user when clicking the mark.

We recommend using the PDF format for scalable images.

As an example, if you have `Robot Template.pdf` in your bundle’s support folder then you can set a mark on line 180 with associated text using the following line of shell code:

```
"$TM_MATE" --line="180" --set-mark="$TM_BUNDLE_SUPPORT/Robot Template.pdf:Danger, Will Robinson!"
```

In this example the mark will be set for the current document, this is because if we do not specify any file path then `mate` will read the value of the `TM_DOCUMENT_UUID` environment variable and use that as the default value for `--uuid`.

To make that explicit, we could have added `--uuid="$TM_DOCUMENT_UUID"`.

If your `<mark>` does not start with a slash (`/`) then TextMate will expect it to be the name of one of the built-in images. Currently these four built-in images exist: `error`, `warning`, `note`, and `search`.

To clear a mark you use `--clear-mark` with the mark you wish to clear, for example:

```
"$TM_MATE" --clear-mark="$TM_BUNDLE_SUPPORT/Robot Template.pdf"
```

In the above there is no document or line reference, so all marks of this type will be cleared for all lines in all documents. Specify document and/or line to limit what gets cleared.

If the mark given to `--clear-mark` has a trailing slash (`/`) then all marks with this prefix gets cleared, for example if you have multiple marks in your bundle’s support folder and wish to clear them all in one go then use:

```
"$TM_MATE" --clear-mark="$TM_BUNDLE_SUPPORT/"
```

Lastly when you wish to update marks, it is recommended to call `mate` with both `--clear-mark` and `--set-mark`, as that avoids potential flicker when setting a mark for a line that already has one.

### Calling `mate` from Commands {#calling-mate-from-commands}

If you are writing a TextMate command then the `TM_MATE` environment variable is setup to point to `mate`, so always use this variable instead of `mate` directly.

For example if you want to put an error icon on line 180 of the current file then it can be done using something like the following:

```
"$TM_MATE" --set-mark="error:Deprecated syntax." --line=180
```

### Shell Variables {#shell-variables}

After having installed `mate` you may want to setup a few shell variables described in the following subsections.

#### `EDITOR` {#editor}

The `EDITOR` variable is used by most shell commands that require an external editor. Since these commands expect a synchronous editor, you should include the `-w` flag when setting this variable. For example (`~/.profile`):

```
export EDITOR='mate -w'
```

Currently `crontab` does not treat `EDITOR` as a “shell string” making it not possible to provide arguments to `mate`. If you need to use TextMate to edit your cron then you can create a symbolic link to `mate` with a `_wait`. For example:

```
ln -s mate ~/bin/mate_wait   # run this once to create the link
export EDITOR='mate_wait'    # use in your ~/.profile or similar
```

#### `GIT_EDITOR` {#git_editor}

When you commit to a [Git](http://git.or.cz/) repository you may find that your caret is not at the first line.

This is because Git reuses the same file for each new commit message and TextMate reads caret position from the file’s extended attributes, so if the file was saved with the caret placed in the middle of the file, the next time you write a commit message, the caret will be restored to this position.

Recent versions of TextMate has disabled creation of extended attributes for Git commit messages, but it will still read them, so if you have old repositories and you do not want to manually remove the extended attributes for `.git/COMMIT_EDITMSG` then you can instead set the Git editor to `mate -wl1`. This instructs TextMate to open with the caret at line 1 rather than where it was last placed.

To set it like this for Git, you can set the `GIT_EDITOR` variable or Git’s `core.editor` configuration variable.

#### `TEXEDIT` {#texedit}

When TeX gives an error message with a file reference, you can enter `e` to edit the file (and correct the error).

For this to work with `mate`, set the `TEXEDIT` variable like this:

```
export TEXEDIT='mate -w -l %d "%s"'
```

#### `LESSEDIT` {#lessedit}

The `less` pager supports editing the file being viewed by pressing `v`. To setup TextMate to be used with less, you need to setup the `LESSEDIT` variable:

```
export LESSEDIT='mate -l %lm %f'
```

## `rmate` {#rmate}

If you are using ssh then you may find it useful to be able to edit remote files in TextMate.

This can be achieved using the `ramte` script, for more info see the [`rmate` GitHub page](https://github.com/textmate/rmate).

## URL Scheme {#url-scheme}

The `txmt` URL scheme allows you to open files in TextMate via hyperlinks found for example in HTML documents (anchors). These can refer to local files which can be useful when:

1. Using [commands](commands.html#commands) with [HTML output](html-output.html#html-output) that indicate errors/warnings with the current document, or refer to other documents in your project.
1. If you are generating a set of web-pages from simpler (text) files you can have these link to the original text files, so that when you are inspecting the generated result (in a browser) you can quickly edit the source of each page by following the `txmt:`-link.

The URL scheme is `txmt:` and currently has one command named `open`. This command takes up to three arguments:

`url` The (file) URL to open, defaults to current file, example: `url=file://~/.profile`. `line` The line on which the caret should be placed after opening the file, example: `line=11`. `column` The column on which the caret should be placed after opening the file, example: `column=3`.

So a full example of a `txmt:` URL could be ([click here to test](txmt://open/?url=file://~/.profile&line=11&column=2)):

```
txmt://open/?url=file://~/.profile&line=11&column=2
```

If your `txmt` links open in another application then it is because that application has claimed the URL scheme. There is currently no way for the user to change this, but if you go to the Terminal pane in TextMate’s preferences then TextMate will reclaim its URL scheme.

## Binary Files {#binary-files}

Some binary files have a textual representation, for example binary property lists, compressed or encrypted text files, compiled AppleScript code, etc.

You can create a TextMate command to decode such files during open. Though for the file types mentioned above, import commands already exist in the Avian bundle.

To create your own import command, create a regular command in the bundle editor with input set to document and output set to replace the document, and then set the “content match” to a regular expression that will match your file type’s signature.

For example a PNG file should always start with the following 8 hexadecimal bytes: `89 50 4E 47 0D 0A 1A 0A` so a “content match” regular expression to identify PNG files could be: `\A\x{89}PNG\r\n\x{1A}\n`.

Here `\A` anchors the match to the beginning of the file and some of the hexadecimal values are represented using their ASCII characters (`PNG`) or escape sequence (`\r\n`).
