# Runtimes

Bundle commands are written in Ruby, and some in Python. TextMate 2.1 runs them on a Ruby and a Python it chooses, so a command behaves the same on every machine, and so the bundles can be tested against one version rather than against whatever a machine happens to have.

## The versions

Each release names one Ruby and one Python, exactly: 4.0.9 and 3.14.9 as this is written. A newer patch arrives with an update to the application, not on its own. The pins are exact rather than a series so that nothing moves under the bundles without a release saying so.

## How a runtime arrives

Three bundles carry the arrangement. **Runtimes** carries the protocol every resolver answers in. **Runtimes Ruby** carries rv, the Ruby version manager from Spinel, and is mandatory, since every bundle's support library is Ruby. **Runtimes Python** carries uv, the Python manager from Astral, and is optional, because only a minority of bundles run Python and uv is larger than everything else TextMate ships put together.

At launch the application asks each installed Runtimes bundle for its pinned version. The bundle's resolver looks wherever its manager looks: your own rv or uv first, if you have one, so what TextMate installs shows up in your own list, and the copy inside the bundle last. If the version is on the machine it is used. If not, the manager installs a prebuilt copy, in seconds, into its usual place: `~/.local/share/rv/rubies/` for Ruby and `~/.local/share/uv/python/` for Python.

Offline, with the version not yet installed, the newest Ruby 4 or Python 3 the manager already has stands in, and TextMate says so. With nothing at all, Ruby commands do not run, and TextMate says that too.

## What the strip above the text says

When the launch did something about a runtime on its own, every document window shows a strip above the text until you dismiss it, which dismisses it everywhere at once. The three things it says:

| The strip says | What happened |
| --- | --- |
| TextMate installed Ruby 4.0.9 for bundle commands, at … | The version was not on the machine and the manager installed it |
| Bundle commands run on a Ruby other than 4.0.9. … | The version could not be installed, offline most likely, and another one stands in. The second sentence is the manager's own reason |
| Ruby bundle commands will not run. … | Nothing could be found or installed, or the Runtimes Ruby bundle is not installed |

The same for Python. The strip appears once per launch and is gone for good once dismissed. The next launch asks again, so a fallback that lasts shows up again until it is fixed, and an install, which happens once, is seen once.

## Python is asked for when first needed

The Runtimes Python bundle is not installed by default. The first Python command you run fails, says so on its own standard error, and TextMate offers to install the bundle that provides Python. Say yes and the bundle installs, the pinned Python with it, and the command runs from then on. Say no and you are not asked again until the next launch.

## Your own Ruby or Python

`TM_RUBY` and `TM_PYTHON`, set in Preferences, in a `.tm_properties` file, or in a bundle, name the interpreter a command runs on, and TextMate honors them with three exceptions, each of which is said on the command's standard error:

- The system's Ruby or Python is refused. The pinned runtime is used instead.
- A path that is not an executable file is refused the same way.
- A Ruby older than 4.0 is honored, with a warning that a failure may be the version rather than the bundle.

The pinned runtime goes first on the `PATH` every command gets and is named in `TM_RUBY` and `TM_PYTHON` when you have not set them, so `#!/usr/bin/env ruby` and `#!/usr/bin/env python3` in a command find it.

## The managers themselves

The resolver prefers a manager you installed yourself over the copy in the bundle, so a Python TextMate installs appears in your own `uv python list`, and a Ruby in `rv ruby list`. If your own copy is older than the version TextMate asks for, the bundle's copy installs it instead, into the same place, where yours finds it from then on.

The copies the bundles carry are updated through the bundle catalog, without an update to the application.
