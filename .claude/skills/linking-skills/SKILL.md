---
name: linking-skills
description: Use when the user wants to test a skill from a local working copy without publishing or reinstalling its plugin. Links the skill folder into Claude's skills directory as `<skill>-dev`, lists linked skills, and removes the links.
argument-hint: "[link|unlink|list] [skill name or path]"
---

# Linking Skills

Make a skill from a local folder available to Claude without installing its plugin. A symlink points at the working copy, so edits to the skill apply right away and new sessions see them too.

## Naming

Every link is named `<skill>-dev`, where `<skill>` is the skill's folder name. Claude takes the skill's name from the link name, so the linked copy shows up as `/<skill>-dev`, next to an installed plugin version such as `/<plugin>:<skill>`. The `-dev` suffix marks every link this skill makes; links without it belong to the user and are never touched.

## Target directory

- Default: `~/.claude/skills/` (global, all projects).
- Project: `<repo>/.claude/skills/` only when the user asks for project scope. Check that `.claude/skills/*-dev` is git-ignored there and tell the user if it is not.

## Link

1. Resolve the source to an absolute folder that contains `SKILL.md`. Accept a path, or a skill name to find under the current repo (`plugins/*/skills/<name>/`). If several folders match, ask which one.
2. Let `link=<target>/<skill>-dev`.
   - If `link` is already a symlink to the same folder, report that it is linked and stop.
   - If `link` exists and is anything else, stop and tell the user. Never overwrite it.
3. Create it: `mkdir -p <target> && ln -s <source> <link>`.
4. Tell the user to call `/<skill>-dev`. It works in this session right away, and in any new session.

## List

Show links with the suffix and where they point:

```bash
for link in ~/.claude/skills/*-dev; do [ -L "$link" ] && echo "${link##*/} -> $(readlink "$link")"; done
```

Also check `.claude/skills/` in the current repo if it exists. Flag links whose target no longer exists.

## Unlink

Remove one link (`unlink <skill>`) or all of them (`unlink all`). Remove only entries that are symlinks and end in `-dev`, with `rm <link>` (never `rm -r`). This deletes only the link; the source folder stays as it is. Report what was removed.
