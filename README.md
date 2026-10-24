# TextMate manual

The manual for every version of TextMate, one directory per documented version, matching the published URL scheme the way the Ruby documentation and Bootstrap do it.

| Directory | Covers         | Nature                                                        |
| --------- | -------------- | ------------------------------------------------------------- |
| `1/`      | TextMate 1.x   | Preservation. Extracted from the archived macromates.com HTML |
| `2.0/`    | TextMate 2.0.x | Preservation. The manual as the 2.0 era left it               |

Each future release adds its own directory by copying the previous version's forward and editing it. Published paths mirror the directories: `/manual/1.0/`, `/manual/2.0/`, and an unversioned `/manual/` that redirects to the newest stable version so old links never rot.

## Current state of the tree

`1/` holds the TextMate 1.x manual as archived from macromates.com: compiled HTML pages and their images, preserved exactly as served. The pages never had a markdown source, and their stylesheet links point at the original site, so they read best as content rather than render faithfully offline. Extracting them into a source format is future work.

`2.0/` holds the TextMate 2.0 manual as last deployed, compiled HTML containing prose that was edited after the markdown sources were last touched. `2.0/markdown/` holds that prose recovered from the HTML, chapter by chapter, so the markdown says what the deployed manual says. Headings keep their anchors as a trailing `{#id}`, the header attribute kramdown and Pandoc read, since fifteen years of links point at them. The HTML stays as the record of what was deployed.

This repository is content only. Website tooling, when it exists, lives with the website.
