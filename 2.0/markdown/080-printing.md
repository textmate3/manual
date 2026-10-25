# Printing {#printing}

## Printing Plain Text {#printing-plain-text}

If you wish to print text or produce a PDF from your document then you can select _File → Print…_ (`⌘P`) to bring up the system’s standard printing sheet.

Text will be syntax highlighted using the theme selected but since TextMate does not do background fills during printing, you should avoid selecting a theme that is made to draw light text on a dark background.

The current document font, with a point size of 11, is used for printing. You can adjust the scale factor if you wish to change the text size. To change the font itself you need to change the document font before bringing up the print dialog, this is done using the font panel available as _View → Font → Show Fonts_.

Margins can be increased by selecting `Manage Custom Sizes…` from the `Paper Size` pop-up button. Create a new size with the same dimensions as your normal paper and increase the values for the non-printable area.

If you miss some of the options mentioned above then you may need to select `Show Details`.

## Printing Processed Output {#printing-processed-output}

Some text formats, like Markdown and reStructuredText, are meant to be converted into other formats, often HTML. TextMate normally offers a preview action (bound to `⌃⌥⌘P`) which will show the styled text in an [HTML output](html-output.html#html-output) window.

After the output window appears, you can print the content using the standard printing sheet available by selecting _File → Print…_ (`⌘P`).

Output is using the print styles from the current [HTML output theme](html-output.html#html-output-theme).

This functionality is also available for generated PDF documents created by actions like _Bundles → LaTeX → Typeset & View_ (`⌘R`).
