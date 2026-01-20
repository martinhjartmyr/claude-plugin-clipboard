---
description: Read content from the system clipboard
allowed-tools: Bash(pbpaste:*), Bash(xclip:*), Bash(wl-paste:*), Bash(powershell.exe:*), Bash(command:*), Bash(printf:*), Bash(cat:*)
---

# Paste from Clipboard

Read the current content from the system clipboard and display it.

## Instructions

Detect the platform and read the clipboard using the appropriate command:

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
  echo "No clipboard command found"
  exit 1
fi
```

After reading, display the clipboard content to the user. If the user provided additional instructions via $ARGUMENTS, apply them to the clipboard content (e.g., "paste and format as JSON").
