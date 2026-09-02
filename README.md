# TextMate manual

The manual for every version of TextMate, one directory per documented version, matching the published URL scheme the way the Ruby documentation and Bootstrap do it.

| Directory | Covers         | Nature                                                        |
| --------- | -------------- | ------------------------------------------------------------- |
| `1/`      | TextMate 1.x   | Preservation. Extracted from the archived macromates.com HTML |
| `2.0/`    | TextMate 2.0.x | Preservation. The manual as the 2.0 era left it               |

Each future release adds its own directory by copying the previous version's forward and editing it. Published paths mirror the directories: `/manual/1/`, `/manual/2.0/`, and an unversioned `/manual/` that redirects to the newest stable version so old links never rot.

## Current state of the tree

`2.0/` holds the manual as last deployed, compiled HTML containing prose that was edited after the markdown sources were last touched. Nothing regenerates or writes into it.

The markdown source and its tooling live at the repository root (`pages/`, `bin/`, `Makefile`) and are what future versions build from. The source lags `2.0/`: recovering the newer prose from the deployed HTML back into `pages/` comes before any other work here, and the recovery is proved by rebuilding and comparing against `2.0/`. Until then, `make` produces stale output.

`1/` holds the TextMate 1.x manual as archived from macromates.com: compiled HTML pages and their images, preserved exactly as served. The pages never had a markdown source, and their stylesheet links point at the original site, so they read best as content rather than render faithfully offline. Extracting them into a source format is future work.

Shared tooling stays at the repository root and serves every version directory.
