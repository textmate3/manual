# Commands {#commands}

- Commands with non-zero exit code treated as error
- Current working directory

## Search Path {#search-path}

### Required Commands {#required-commands}

```
requiredCommands = (
    command = 'clang-format';
    variable = 'TM_CLANG_FORMAT';
    locations = ( '/usr/local/bin/clang-format' );
    moreInfoURL = 'http://clang.llvm.org/docs/ClangFormat.html';
);
```

## Drag Commands {#drag-commands}

- Dropping multiple files

## Writing Commands {#writing-commands}

- Use `TM_«tool»` and use it without quotes if there is a chance the tool might be multiple tools.

### Executor Commands {#executor-commands}

The command can read the document content via stdin, that will be the current state of the document.

Do you wish for the command to never save the document? There are a few options, but it depends on your goal.

For example, if you wish the command to fire after a save operation (how it is presently setup, although by overloading the `⌘S` key, which we discourage) then you can set the semantic class to ‘callback.document.did-save’ then it is automatically called after each save, and can read the document via the `TM_FILEPATH` environment variable (note that if you do this, you should make sure that “Save” is set to “Nothing” to avoid a loop).

If you wish to run the command decoupled from any potential save, then you should read the document from stdin. We do have some ruby support code that most of the run commands use, it will save an untitled document to a temporary file and setup `TM_FILEPATH` accordingly, meaning that the command can be assert that `TM_FILEPATH` always points to an up-to-date file.

A simple example of this is:

```
#!/usr/bin/env ruby18 -wKU
require "#{ENV['TM_SUPPORT_PATH']}/lib/tm/save_current_document"

TextMate.save_if_untitled('js')
# use ENV['TM_FILEPATH']
```

The argument to `TextMate.save_if_untitled` is the extension to use for the temporary file (and can be left out).

When using `TextMate.save_if_untitled` the command should set “Save” to “
