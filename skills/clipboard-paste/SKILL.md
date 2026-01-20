---
description: Read content from clipboard when the user wants to access what they've copied. Triggers on phrases like "paste from clipboard", "what's in my clipboard", "read clipboard", "get clipboard contents", or "use what I copied".
allowed-tools:
  - Bash(pbpaste:*)
  - Bash(xclip:*)
  - Bash(wl-paste:*)
  - Bash(powershell.exe:*)
  - Bash(command:*)
  - Bash(printf:*)
  - Bash(cat:*)
---

# Clipboard Paste Skill

This skill enables reading content from the system clipboard when the user's intent suggests they want to access previously copied content.

## When to Activate

Use this skill when the user:

- Asks "what's in my clipboard" or "show clipboard contents"
- Says "paste from clipboard" or "read clipboard"
- Mentions "use what I copied" or "the thing I copied"
- Asks to process, analyze, or work with clipboard content
- Says "paste that here" referring to external content

## Cross-Platform Clipboard Commands

Detect and use the appropriate clipboard command:

### macOS

```bash
pbpaste
```

### Linux (Wayland)

```bash
wl-paste
```

### Linux (X11)

```bash
xclip -selection clipboard -o
```

### WSL

```bash
powershell.exe -command "Get-Clipboard"
```

## Platform Detection Pattern

```bash
if command -v pbpaste &>/dev/null; then
  pbpaste
elif command -v wl-paste &>/dev/null; then
  wl-paste
elif command -v xclip &>/dev/null; then
  xclip -selection clipboard -o
elif command -v powershell.exe &>/dev/null; then
  powershell.exe -command "Get-Clipboard"
else
  echo "Error: No clipboard command available"
  exit 1
fi
```

## After Reading

Display the clipboard content to the user. If the user asked to process or transform the content (e.g., "paste and summarize", "what's in my clipboard and fix the formatting"), apply the requested operation to the content.
