# Requirements

## The system

TextMate 2.1 requires **macOS 27 or later**, on **Apple silicon**. There is no build for Intel Macs, and there is no build for an older macOS.

The floor is the current system on purpose. The application is built against the current SDK with no guards for older systems, so it can use what the system provides without reasoning about which version has what. The minimum moves to each new macOS as it ships.

## Hardware

An Apple silicon Mac. The application, its command line tools and the runtime managers it carries are all built for arm64 alone.

## What you do not need to install

TextMate's bundles run their commands on Ruby, and some of them on Python. You do not install either.

- **Ruby** arrives through the Runtimes Ruby bundle, which every copy of TextMate carries. On first launch the application asks it for the exact Ruby version the release pins, finds it if it is already on the machine, and installs it if not. See [Runtimes](runtimes.md).
- **Python** arrives the same way through the Runtimes Python bundle, which is optional. A command that needs Python offers to install the bundle the first time it runs.
- **The system's Ruby and Python** are never used, even when you point TextMate at them. macOS ships a Ruby and a Python too old for the bundles, and the application refuses them by name and says so.

## Network

The first launch, and the first run of a Python command, may download a runtime, a few tens of megabytes. After that the runtimes are on disk and nothing needs the network. Offline, with a runtime not yet installed, TextMate stands in with the newest one it can find on the machine and tells you so in a strip above the text.

Bundles install and update from the TextMate 3 catalog over the network, and the application checks for its own updates the same way. The Bundles and Updates panes of Preferences control both.

## Where things live

| What | Where |
| --- | --- |
| Preferences | `~/Library/Preferences/com.textmate3.TextMate.plist` |
| Bundles, sessions, settings | `~/Library/Application Support/TextMate 3/` |
| Caches | `~/Library/Caches/com.textmate3.TextMate/` |
| Rubies the application installed | `~/.local/share/rv/rubies/` |
| Pythons the application installed | `~/.local/share/uv/python/` |

TextMate 2 keeps its own preferences and its own Application Support directory, and 2.1 leaves them exactly as they are. See [Breaking changes](breaking-changes.md) for what the first launch imports from them.
