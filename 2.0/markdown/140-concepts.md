# Concepts {#concepts}

## Proxy Item {#proxy-item}

- SCM bundle
- Generic build action etc. (work in progress)

## Semantic Class {#semantic-class}

- Import/export commands, e.g. Binary Property List
- Will/did save callbacks, e.g. Make Script Executable

## Encoding {#encoding}

- Byte order mark / endian: UTF–8, UTF–16, UTF–32.
- `.tm_properties`: `attr.file.unknown-encoding`.
- Extended attributes: `com.apple.Encoding`.
- Line endings

TextMate will do a few tests against your file:

1. Does it have an UTF–8/16/32 BE/LE BOM? _Do not take this to mean that UTF–8 BOMs are fine!_
1. Does it have a `com.apple.TextEncoding` extended attribute?
1. Does it have an `encoding` setting via `.tm_properties`?
1. Is it valid ASCII/UTF–8?

If all of these tests fail, it will ask you to pick the proper encoding.
