# Requirements

## The system

TextMate 2.1 requires **macOS 27 or later** and an **Apple silicon** chip. Intel Macs and older versions of macOS are not supported.

## Hardware

An Apple silicon Mac (M series or A series). The application, the command line tools, and the runtime managers it includes are only built for arm64.

## What you don't need to install

TextMate's bundles mostly run their commands in Ruby. A few run in Python. You don't install either.

- **Ruby** is installed through the default included `Runtimes Ruby` bundle.

On first launch the application gets the `Runtimes Ruby` bundle gets the pinned Ruby version from the app. The bundle finds it if it is already on the machine or downloads/installs it if it's not already installed. See [Runtimes](runtimes.md).

- **Python** is installed the same way through the optional `Runtimes Python` bundle. A command that needs Python offers to install the bundle the first time it runs.

- **The system's Ruby and Python** are never used, even if you point TextMate at them. macOS ships a Ruby and a Python too old for the bundles. So, the application rejects them entirely and tells you that it did and why it did.

## Network

The first time TextMate 2.1 is launched, it may download a runtime, if your system doesn't have the expected version.

The first time a Python command is run, it may also download a runtime, if your system doesn't have the expected version of Python. The download is a few dozen megabytes.

After that the runtimes are on disk and nothing needs the network. Offline, with a runtime not yet installed, TextMate will use the newest one it can find on the machine and tells you so in a banner above the editor.

Bundles install and update from the TextMate 3 catalog over the network. The application checks for its own updates the same way. Both can be turned off in `Preferences`.

## Where things live

| What                              | Where                                                |
| --------------------------------- | ---------------------------------------------------- |
| Preferences                       | `~/Library/Preferences/com.textmate3.TextMate.plist` |
| Bundles, sessions, settings       | `~/Library/Application Support/TextMate 3/`          |
| Caches                            | `~/Library/Caches/com.textmate3.TextMate/`           |
| Rubies the application installed  | `~/.local/share/rv/rubies/`                          |
| Pythons the application installed | `~/.local/share/uv/python/`                          |

TextMate 2.0 keeps its own preferences and its own Application Support directory. TextMate 2.1 leaves them exactly as they are. See [Breaking changes](breaking-changes.md) for what the first launch imports from them.
