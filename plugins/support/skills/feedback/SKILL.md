---
name: feedback
description: File feedback about a skill from the azamath/skills marketplace (a bug or an improvement idea) as a GitHub issue.
disable-model-invocation: true
argument-hint: "[what went wrong or what to improve]"
---

Turn the user's feedback about a skill into one GitHub issue in the private repo `azamath/skills-feedback`. A later agent acts on the issue without access to this session, so it must stand on its own.

User's feedback: $ARGUMENTS

## Steps

1. **Classify.** The feedback is a **bug** (the skill did something wrong or failed to do what its instructions promise) or an **enhancement** (the skill worked as written, but the user wants it to do more, less or differently). Done when you have picked one. If the feedback mixes both, file the main point and mention the rest in the body.

2. **Identify the skill.** A loaded skill prints `Base directory for this skill: <path>`; for this marketplace the path looks like `.../plugins/cache/azamath/<plugin>/<version>/skills/<skill>`, which gives plugin, version and skill name. An enhancement may name a skill that did not run in this session: find it under `~/.claude/plugins/cache/azamath/`. Done when you have plugin, skill and version. If several skills fit, ask the user. If the skill is not from this marketplace, tell the user and stop.

3. **Write the report.** Fill the template for the kind from step 1, using the session and the feedback. Done when a reader who never saw this session can act on it: for a bug, reproduce the failure; for an enhancement, see what to change and why.
   - Quote only the turns that matter, trimmed to the relevant lines. Replace secrets, tokens, personal data and proprietary code with `<redacted>`.
   - Ground "Suspected cause" and "Where in the skill" in `SKILL.md`: quote the instruction involved, or name the section the change belongs in. Write "unknown" when you cannot tell.

4. **Confirm.** Show the user the title and body and ask to file it. Apply their edits.

5. **File it.** `<kind>` is `bug` or `enhancement`.

   ```bash
   gh label create skill-feedback --repo azamath/skills-feedback --color D93F0B --force
   gh label create <kind> --repo azamath/skills-feedback --force
   gh label create "skill:<plugin>/<skill>" --repo azamath/skills-feedback --color 0E8A16 --force
   gh issue create --repo azamath/skills-feedback --title "<title>" --label skill-feedback --label <kind> --label "skill:<plugin>/<skill>" --body-file <tmp file>
   ```

   If the repo does not exist or `gh` is not authenticated, show the report to the user and say what failed. Done when you have given the user the issue URL.

## Templates

Title: `<plugin>/<skill>: <the point in a few words>`

Both kinds start with:

```markdown
**Skill:** <plugin>/<skill> v<version>
**Project:** <repo or folder name, no absolute paths>
```

Bug:

```markdown
## Expected
<what the user wanted the skill to do>

## Actual
<what the skill did instead>

## Evidence
<trimmed, redacted quotes from the session>

## Suspected cause
<quoted SKILL.md instruction or missing guidance, or "unknown">
```

Enhancement:

```markdown
## Now
<what the skill does today>

## Wanted
<the change the user asks for>

## Why
<the situation that prompted it; quotes from the session if it came up there>

## Where in the skill
<SKILL.md section or instruction to change, or "unknown">
```
