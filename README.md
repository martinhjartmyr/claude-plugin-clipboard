# Clipboard Plugin for Claude Code

Copy and read content from the system clipboard with cross-platform support.

## Features

- **Cross-platform**: macOS, Linux (X11/Wayland), WSL
- **No external dependencies**: Uses native clipboard commands
- **Handles special characters**: Backslashes, quotes, escape sequences
- **Multi-line support**: Properly handles content with newlines

## Usage

### Slash Commands

**Copy to clipboard:**

```
/clipboard:copy <content>
```

**Read from clipboard:**

```
/clipboard:paste
```

### Autonomous Skills

The plugin automatically activates when you use phrases like:

**For copying:**

- "copy that"
- "copy to clipboard"
- "generate a commit message and copy it"
- "put that in the clipboard"

**For reading:**

- "what's in my clipboard"
- "paste from clipboard"
- "use what I copied"
- "read clipboard contents"

Example workflows:

```
> Generate a commit message for my staged changes and copy it to clipboard
> What's in my clipboard?
> Paste from clipboard and format as JSON
```

## Supported Platforms

| Platform        | Copy                         | Paste                           |
| --------------- | ---------------------------- | ------------------------------- |
| macOS           | `pbcopy`                     | `pbpaste`                       |
| Linux (X11)     | `xclip -selection clipboard` | `xclip -selection clipboard -o` |
| Linux (Wayland) | `wl-copy`                    | `wl-paste`                      |
| WSL             | `clip.exe`                   | `powershell.exe Get-Clipboard`  |

## Security Note

When you install this plugin, Claude Code displays a warning:

> Claude Code may read, write, or execute files contained in this directory.

This is expected behavior. The plugin uses skills that execute system clipboard commands (`pbcopy`, `pbpaste`, `xclip`, `wl-copy`, `wl-paste`, `clip.exe`). These are standard utilities included with your operating system - the plugin does not install or download any additional software.

The plugin's permissions are limited to these clipboard utilities only.

## Installation

### From Marketplace

```bash
# Add the marketplace (first time only)
claude plugin marketplace add martinhjartmyr/mh-cc-plugins

# Install the plugin
claude plugin install clipboard@mh-cc-plugins
```

## License

MIT
