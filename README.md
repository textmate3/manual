# TextMate manual

The manual for every version of TextMate, one directory per documented version, matching the published URL scheme the way the Ruby documentation and Bootstrap do it.

| Directory | Covers         | Nature                                                        |
| --------- | -------------- | ------------------------------------------------------------- |
| `1/`      | TextMate 1.x   | Preservation. Extracted from the archived macromates.com HTML |
| `2.0/`    | TextMate 2.0.x | Preservation. The manual as the 2.0 era left it               |

Each future release adds its own directory by copying the previous version's forward and editing it. Published paths mirror the directories: `/manual/1/`, `/manual/2.0/`, and an unversioned `/manual/` that redirects to the newest stable version so old links never rot.

## Current state of the tree

The 2.0 content still lives at the repository root (`pages/`, `bin/`, `Makefile`) and moves into `2.0/` only after the stranded prose is recovered. `deployed-html-2016/` holds the manual as last deployed, compiled HTML containing prose that was edited after the markdown sources were last touched. It is the reference the recovery works from and rebuilds are checked against, so nothing regenerates or writes into it. Running `make` before that recovery would overwrite the markdown with older content, so the recovery comes first.

`1/` holds the TextMate 1.x manual as archived from macromates.com: compiled HTML pages and their images, preserved exactly as served. The pages never had a markdown source, and their stylesheet links point at the original site, so they read best as content rather than render faithfully offline. Extracting them into a source format is future work.

Shared tooling stays at the repository root and serves every version directory.
