---
name: wiki-setup
description: One-time setup — saves wiki root path, registers permissions, and symlinks skills for cross-workspace access
disable-model-invocation: true
---

# Wiki Setup

One-time setup to enable llm-wiki access from any workspace.

## Steps

1. **Save wiki root path**

Create `~/.config/llm-wiki/root` containing the absolute path to this wiki repository:

```bash
mkdir -p ~/.config/llm-wiki
echo "!`pwd`" > ~/.config/llm-wiki/root
```

2. **Register additionalDirectories in global settings**

Add this wiki's path to `~/.claude/settings.json` under `permissions.additionalDirectories`:

Read `~/.claude/settings.json`. If `permissions` does not exist, create it as an empty object. If `permissions.additionalDirectories` does not exist, create it as an empty array. Add the current directory's absolute path to the array (if not already present). Write the file back.

3. **Symlink skills to user-level directory**

Create symlinks for wiki skills so they are discoverable from any workspace:

```bash
WIKI_ROOT="!`pwd`"
mkdir -p "$HOME/.claude/skills"
for skill in wiki-ingest wiki-query wiki-lint wiki-research; do
  ln -sfn "$WIKI_ROOT/.claude/skills/$skill" "$HOME/.claude/skills/$skill"
done
```

Note: `wiki-setup` is NOT symlinked — it is only needed inside this repo.

4. **Confirm to user**

Print:
- Wiki root saved to `~/.config/llm-wiki/root`
- `permissions.additionalDirectories` registered in `~/.claude/settings.json`
- Skills symlinked to `~/.claude/skills/`: wiki-ingest, wiki-query, wiki-lint, wiki-research
- You can now use `/wiki-ingest`, `/wiki-query`, `/wiki-lint`, `/wiki-research` from any workspace
