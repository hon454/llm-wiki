---
name: wiki-setup
description: One-time setup — registers llm-wiki as a global addDir and saves the wiki root path for cross-workspace access
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

2. **Register addDir in global settings**

Add this wiki's path to `~/.claude/settings.json` under `addDirs`:

Read `~/.claude/settings.json`. If `addDirs` does not exist, create it as an empty array. Add the current directory's absolute path to the array (if not already present). Write the file back.

Example entry to add:
```json
{
  "addDirs": ["/Users/jeonjihun/Workspace/llm-wiki"]
}
```

3. **Confirm to user**

Print:
- Wiki root saved to `~/.config/llm-wiki/root`
- addDir registered in `~/.claude/settings.json`
- You can now use `/wiki-ingest`, `/wiki-query`, `/wiki-lint` from any workspace
