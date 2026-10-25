# Searching {#searching}

The primary search actions which you should learn the key equivalents for are:

- _Edit → Find → Find Next_ (`⌘G`)
- _Edit → Find → Find Previous_ (`⇧⌘G`)
- _Edit → Find → Show History_ (`⌃⌥⌘F`)
- _Edit → Find → Use Selection for Find_ (`⌘E`)

Another action is _Bundles → Text → Selecting → Extend Selection to Include Next_ (`⌃W`), more detail about this action can be found in the [search clipboard](searching.html#search-clipboard) section below.

TextMate’s search dialog can be brought up by selecting _Edit → Find → Find…_ (`⌘F`). Once this dialog is showing the _Edit → Find → Find All_ (`⌥⌘F`) menu item will trigger the `Find All` button in the search dialog and show you all results from the current document. When the search dialog is not showing, the same action will instead select all matches in the current document.

If you wish to search for tab or newline characters then you can press option-tab or option-newline to insert these in the search or replace fields. The fields will automatically grow with their content.

The sigma button (`Σ`) in the search dialog can be used to count the number of results in the current document. Though the same information can be obtained via `Find All` so this action is likely to be removed in a future version of TextMate.

## Folder Search {#folder-search}

You can select _Edit ⇢ Find → Find in Project…_ (`⇧⌘F`) to bring up the search dialog with the `In` pop-up defaulting to the current project folder. You can also click the magnifying glass in the file browser to open a search dialog pointing to the file browser’s [effective folder](projects.html#effective-folder), or switch focus to the file browser before pressing `⇧⌘F`.

Should you wish to extend the search to a parent folder then you can use _Go → Enclosing Folder_ (`⌘↑`) or interact with the `In` pop-up menu.

When the `In` pop-up is showing a folder you can select `Find All` to perform a search across the files in the selected folder.

If you wish to limit the search then you can edit the `matching` [glob string](references.html#glob-string). For example to only search HTML files you can make it `*.html` or if you wish to exclude all files in the `vendor` folder, make it `!vendor/**`.

See [filtering files](settings.html#filtering-files) for more options.

If you wish the search to follow symbolic links then you should enable this via the gear pop-up menu to the right of the `matching` text field.

You can abort a running search by clicking the stop button that appear to the left of the status text, closing the window, or pressing `⌘.`.

## Search Results {#search-results}

After a folder search you will see the results grouped by file. There are a few useful actions related to these results which we’ll go over in this section.

Using command `⌘1`-`⌘n` will select the first result in the n’th file. It will also move focus to the results list, allowing you to use arrow up and down to move through the results.

Using _Edit → Find → Find Next_ (`⌘G`) will select the first result, and subsequent uses will advance through the results. Likewise _Edit → Find → Find Previous_ (`⇧⌘G`) will initially select the last result and move backwards through the results.

If you close the search dialog and use _Edit → Find → Find Next_ (`⌘G`) in a document with no more results, TextMate will open the next file which have results and select the first result in this file.

From the gear menu in the find dialog you can select _Collapse Results_ (`⌥⌘1`). This is useful when you are only interested in which files contain matches and not the matches themselves.

Also in the gear menu are actions for copying the results to the clipboard. You can copy just the matching parts or the entire line with the match, either with or without file name. The first option, copying matching parts, is useful for [regular expression](references.html#regular-expression) searches, for example a project search for `href=".*?"` followed by _Copy Matching Parts_ will provide you with a list of all links across all files. You can paste that into a new document and use _Bundles → Text → Sorting → Sort Documents & Remove Duplicates_ (F5) to remove duplicates.

## Replacing {#replacing}

When you search for regular expressions the replacement string is interpreted as a [format string](references.html#format-string) where you can use captures from the search. Here is a [blog post about using captures and nested replacements in TextMate](http://blog.macromates.com/2012/nested-replacements/).

As mentioned in the blog post, TextMate offers you replacement previews. These are automatically shown when focus is in the `Replace` text field.

For results that you do not wish to have replaced, uncheck the check box next to the result. By holding down option (`⌥`) when clicking a check box, all results for the corresponding file will be excluded or included.

## Search Clipboard {#search-clipboard}

Similar to how you can copy text to the clipboard with _Edit → Copy_ (`⌘C`) and paste it later with _Edit → Paste_ (`⌘V`) you can use _Edit → Find → Use Selection for Find_ (`⌘E`) to place the current selection on the (globally shared) search clipboard and use _Edit → Find → Find Next_ (`⌘G`) or _Edit → Find → Find Previous_ (`⇧⌘G`) to find the next or previous occurrence of the search string.

The content of the search clipboard is also used when opening the find dialog using _Edit → Find → Find…_ (`⌘F`).

TextMate maintains a history of your previous search strings and you can select _Edit → Find → Show History_ (`⌃⌥⌘F`) to see previous search strings. If you close the list with return (`↩`) then TextMate will search for the next occurrence of the selected string, whereas closing the list with escape (`⎋`) will change the current string on the search clipboard but not perform a search.

In addition to placing selection on the search clipboard, you can also use _Edit → Find → Use Selection for Replace_ (`⇧⌘E`). This is useful if you want to perform a replacement and bypass the search dialog. The steps would be:

1. Select the string you wish to replace and press `⌘E`.
1. Overtype the selection with the replacement, select it, and press `⇧⌘E`.

After this you can use _Edit → Find → Replace All_ (`⌃⌘G`), make a selection and use _Edit → Find → Replace All in Selection_ (`⌃⇧⌘G`), or possibly _Edit → Find → Replace and Find_ (`⌥⌘G`)

If you wish to replace only a few instances near the caret, it might be easier to select the first match and then use _Bundles → Text → Selecting → Extend Selection to Include Next_ (`⌃W`). This action will make the next occurrence of the currently selected string part of the selection, causing multiple occurrences to be selected. Use it to select all the occurrences you wish to replace, and then perform your replacement, as the changes you do to the first occurrence will be mirrored to all the other occurrences.

With no selection the action is _Edit → Select → Word_ (`⌃W`) which means that for single word replacements you only need to place the caret on the word to be replaced and then press `⌃W` until all the occurrences are selected.

## Search Options {#search-options}

Four options affect how the search works, these can be toggled either via the _Edit_ menu or in the find dialog.

The options are as follows:

| Name               | Key Equivalent | Explanation                                                                                                                                 |
| ------------------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Ignore Case        | `⌥⌘C`          | Perform a case insensitive search.                                                                                                          |
| Regular Expression | `⌥⌘R`          | Treat the search string as a _regular expression_.                                                                                          |
| Ignore Whitespace  |                | Whitespace in the document and search string is ignored. This makes it easy to search for multi-line content regardless of its indentation. |
| Wrap Around        | `⌥⌘A`          | Start over from the top of the document when no match is found.                                                                             |

## Incremental Search {#incremental-search}

Selecting _Edit → Find → Incremental Search_ (`⌃S`) will open a search field at the bottom of the window where a search string can be entered, and TextMate will show what it matches in the document. The search is case-insensitive and ignores whitespace.

While the search field is showing you can use the following key equivalents:

| Key Equivalent | Action             |
| -------------- | ------------------ |
| `⌃S` `⇥`       | Find next          |
| `⌃⇧S` `⇧⇥`     | Find previous      |
| `↩` `⎋`        | Close search field |
