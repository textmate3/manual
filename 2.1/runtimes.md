# Runtimes

Bundle commands are mostly written in Ruby. A few are in Python. TextMate 2.1 runs them on a Ruby and a Python it chooses, so a command behaves the same on every machine, and so that the bundles can be tested against one version rather than against whatever a machine happens to have.

## The versions

Each release pins one Ruby and one Python: `4.0.9` and `3.14.9` as this is written. A newer patch arrives with an update to the application, not on its own. The pins are exact rather than a series so that nothing moves under the bundles without a release saying so.

## How a runtime arrives

There are three bundles related to in-app runtimes.

**Runtimes** includes the protocol every resolver answers in.

**Runtimes Ruby** includes [`rv`](https://github.com/spinel-coop/rv), a Ruby version manager made by [Spinel](https://spinel.coop). In TextMate 2.1 `rv` is required because the bundle support library and most of the bundles are written in Ruby.

**Runtimes Python** includes [`uv`](https://github.com/astral-sh/uv), a Python manager made by [Astral](https://github.com/astral-sh/). In TextMate 2.1, `uv` is optional, because only a few bundles run Python and `uv` is larger than everything else TextMate ships put together.

At launch the application asks each installed `Runtimes` bundle for its pinned version. The bundle's resolver looks wherever its manager looks:

- if you have `rv` or `uv` already installed (so that whatever TextMate 2.1 installs also shows up in your own list)
- then the copy inside the bundle

If the version is on the machine it is used. If not, the manager installs a prebuilt copy, into its usual place: `~/.local/share/rv/rubies/` for Ruby and `~/.local/share/uv/python/` for Python. This download/install usually only takes a few seconds because it's prebuilt.

If you're using TextMate 2.1 while offline, then the app will use whichever newest Ruby 4 and Python 3 that `rv` and `uv` know about. The app will also show a notice that this is happening. With no Ruby installed at all, Ruby commands will not run. TextMate gives a notice about this too.

## What the banner above the editor says

When the launch did something about a runtime on its own, every document window shows a banner above the editor until you dismiss it. When you dismiss it in one windows, it's dismissed in all open windows. The banner will say one of three things, depening on your system's situation:

| The banner says                                         | What happened                                                                                                                       |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| TextMate installed Ruby 4.0.9 for bundle commands, at … | The version was not on the machine and the manager installed it                                                                     |
| Bundle commands run on a Ruby other than 4.0.9. …       | The version could not be installed, offline most likely, and another one stands in. The second sentence is the manager's own reason |
| Ruby bundle commands will not run. …                    | Nothing could be found or installed, or the Runtimes Ruby bundle is not installed                                                   |

The same for Python. The banner appears once per launch and is gone for good once dismissed. The next launch asks again, so a fallback that lasts shows up again until it is fixed, and an install, which happens once, is seen once.

## Python is lazily installed

The `Runtimes Python` bundle is not installed by default. Without it installed, whenever you first run a command written in Python, the command will fail with its own standard error.

Then TextMate will offer to install the bundle that provides Python. You then affirm and TextMate will enable the bunle and install the pinned version of Python too. The command and any others written in Python will now run.

If you say no to then bundle installation notice, then nothing is installed, the command won't run, and you won't be asked again until the next launch.

## Your own Ruby or Python

`TM_RUBY` and `TM_PYTHON` can be set in the app's Settings winow. They can also be set in a `.tm_properties` file. A bundle can also specify the interpreter that it runs on.

TextMate will use any of them, with three exceptions. Each exception is said in the command's standard error:

- The system's Ruby or Python is rejected. The app's pinned version of the runtime is used instead.
- A path that is not an executable file is rejected. The app's pinned version of the runtime is used instead.
- A Ruby older than 4.0 can be used. The app will display a warning that a failure may be because of the wrong Ruby version not because of the bundle itself.

The pinned runtime/version goes first on the `PATH` that every command is given. If you haven't set `TM_RUBY` and `TM_PYTHON`, then each is set to the app's pinned version, so `#!/usr/bin/env ruby` and `#!/usr/bin/env python3` in a command finds it.

## The runtime/language managers

If you have a newer version of `rv` or `uv` already installed, then the runtime resolver will use that instead of the older version in the bundle.

That way any version of Python or Ruby installed by TextMate will also show up in your normal list of installed versions. Python with `uv python list` and Ruby with `rv ruby list`.

If you have an older version of `rv` or `uv` already installed, then the runtime resolver will install the newer version from the bundle instead.

The versions of `rv` and `uv` included in the runtime bundles get updated by the bundle catalog without needing an update release to the TextMate app.
