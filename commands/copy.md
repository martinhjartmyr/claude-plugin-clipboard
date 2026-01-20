---
description: Copy content to the system clipboard
allowed-tools: Bash(printf:*), Bash(pbcopy:*), Bash(xclip:*), Bash(wl-copy:*), Bash(clip.exe:*), Bash(cat:*), Bash(command:*)
---

# Copy to Clipboard

Copy the following content to the system clipboard: $ARGUMENTS

## Instructions

Detect the platform and copy the content using the appropriate command:

```bash
if command -v pbcopy &>/dev/null; then
  printf '%s' "CONTENT" | pbcopy
elif command -v wl-copy &>/dev/null; then
  printf '%s' "CONTENT" | wl-copy
elif command -v xclip &>/dev/null; then
  printf '%s' "CONTENT" | xclip -selection clipboard
elif command -v clip.exe &>/dev/null; then
  printf '%s' "CONTENT" | clip.exe
else
  echo "No clipboard command found"
  exit 1
fi
```

For multi-line content, use a heredoc with the detected clipboard command:

```bash
cat <<'EOF' | <clipboard-command>
content here
EOF
```

Where `<clipboard-command>` is the appropriate command from the platform detection above (e.g., `pbcopy` on macOS).

After copying, respond with "Copied to clipboard." Do not repeat the content.
