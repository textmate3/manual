# Breaking changes

What TextMate 2.0 did that 2.1 does differently, and what to do about each.

## macOS 27 is the minimum

TextMate 2.0 runs on older systems and on Intel Macs. TextMate 2.1 requires macOS 27 and an Apple silicon Mac. On an older system or an Intel Mac, keep TextMate 2.0, which continues to run there.

## It is a different application to the system

TextMate 2.1 is `com.textmate3.TextMate`. Its preferences, its `Application Support` directory and its caches are its own, and nothing it does reaches TextMate 2.0's.

What this means:

- **Preferences, bundles and the session** are imported from TextMate 2 on the first launch, once. After that the two applications diverge. A bundle installed in one is not installed in the other, and a preference changed in one is not changed in the other.
- **`defaults write` commands** target `com.textmate3.TextMate`, not `com.macromates.TextMate`. Anything written for the old domain has no effect on 2.1.
- **Bundles you edited by hand** live under `~/Library/Application Support/TextMate 3/` for 2.1. TextMate 2.0's live where they always did.

## The system Ruby and Python are never used

TextMate 2.0 ran bundle commands on whatever Ruby `/usr/bin/ruby` was, and a `TM_RUBY` variable could point at anything. TextMate 2.1 runs bundle commands on the Ruby and Python versions pinned in the app, found or installed through `rv` and `uv`. TextMate 2.1 never the system copies outright.

- A `TM_RUBY` or `TM_PYTHON` set to `/usr/bin/ruby`, `/usr/bin/python3`, anything under the system frameworks, or anything inside Xcode is ignored. The command runs on the pinned runtime instead and the reason is written to the command's standard error.
- A `TM_RUBY` set to a Ruby that does not exist is refused the same way, with the same explanation. In 2.0 this broke every Ruby command with an error that pointed to the wrong file.
- A `TM_RUBY` set to a Ruby that exists but is older than 4.0 is used, as before, and the command's standard error says that a failure may be the version rather than the bundle.

If a bundle of your own depends on a particular Ruby, set it in `TM_RUBY` as before and it's used. As long as it is a real Ruby that is not the system's. See [Runtimes](runtimes.md).

## Shebangs naming the old system Ruby are rewritten

Some commands in older bundles started with a "shebang" (`#!...`) line pointing to old system Ruby, like `#!/System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/ruby`. Any command whose first line is shebang to an old/system Ruby now runs on the app's pinned version Ruby. This is the same way that 2.0 already worked for `#!/usr/bin/env ruby` and `#!/usr/bin/ruby`.

## Python commands need the Runtimes Python bundle

Several bundles are written in and run on Python. In 2.0 a Python command ran on whatever `python` the shell found, which on a current macOS is nothing at all.

In 2.1 a Python command needs the `Runtimes Python` bundle, which is optional and not installed by default. The first Python command you run offers to install it, once per launch. Say yes and the command runs on the pinned Python from then on.

## Cached files are new formats

The bundle index cache and the encoding frequency tables are property lists in the caches folder rather than the packed binary files 2.0 kept. Nothing to do. TextMate 2's own cached files in the shared caches folder are left alone.
