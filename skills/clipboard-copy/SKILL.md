---
description: Copy content to clipboard when the user wants to paste something elsewhere. Triggers on phrases like "copy to clipboard", "put in clipboard", "copy that", "I want to paste this", or requests to generate content for pasting.
allowed-tools:
  - Bash(printf:*)
  - Bash(pbcopy:*)
  - Bash(xclip:*)
  - Bash(wl-copy:*)
  - Bash(clip.exe:*)
  - Bash(cat:*)
  - Bash(command:*)
---

# Clipboard Copy Skill

This skill enables copying content to the system clipboard when the user's intent suggests they want to paste content elsewhere.

## When to Activate

Use this skill when the user:

- Explicitly asks to "copy to clipboard" or "put in clipboard"
- Says "copy that" after generating content
- Mentions wanting to "paste" something
- Asks to "generate X and copy it" (e.g., "generate a commit message and copy it")
- Requests content "for the clipboard"

## Cross-Platform Clipboard Commands

Detect and use the appropriate clipboard command:

### macOS

```bash
printf '%s' "content" | pbcopy
```

### Linux (Wayland)

```bash
printf '%s' "content" | wl-copy
```

### Linux (X11)

```bash
printf '%s' "content" | xclip -selection clipboard
```

### WSL

```bash
printf '%s' "content" | clip.exe
```

## Platform Detection Pattern

```bash
if command -v pbcopy &>/dev/null; then
  printf '%s' "$content" | pbcopy
elif command -v wl-copy &>/dev/null; then
  printf '%s' "$content" | wl-copy
elif command -v xclip &>/dev/null; then
  printf '%s' "$content" | xclip -selection clipboard
elif command -v clip.exe &>/dev/null; then
  printf '%s' "$content" | clip.exe
else
  echo "Error: No clipboard command available"
  exit 1
fi
```

## Handling Special Characters

Always use `printf '%s'` instead of `echo` to handle:

- Backslashes
- Leading dashes
- Escape sequences

## Handling Multi-line Content

For content with newlines, use a heredoc with quoted delimiter to prevent variable expansion:

```bash
cat <<'EOF' | pbcopy
line 1
line 2
special chars: $var \n "quotes"
EOF
```

## After Copying

Confirm to the user with a brief message like "Copied to clipboard." Do not repeat the full content back.
