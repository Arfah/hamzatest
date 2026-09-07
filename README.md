# Claude Skills — source of truth

Durable copies of Arfah's custom Claude.ai skills, so edits survive the session
they were made in.

## Why this repo exists

Claude.ai syncs account skills **one way** into a session container at startup:

```
~/.claude/skills/synced/<bucket-id>/<skill-name>/SKILL.md
```

That folder is writable, so an edit there appears to work and the skill even
behaves differently for the rest of the session — but there is no write-back
path. The container is ephemeral, so the change is destroyed when the session
ends and Claude.ai never sees it.

These skills have `source: custom` (uploaded directly in Claude.ai, not backed
by a repo), so pushing here does not auto-publish either. This repo is the
durable draft; Claude.ai remains the place a skill actually takes effect.

## Update workflow

1. Edit the file under `skills/<skill-name>/SKILL.md` in this repo.
2. Commit and push. The change is now safe.
3. Go to **Claude.ai → Settings → Capabilities → Skills**, open the skill, and
   replace its contents with this file (or re-upload it).
4. Start a **fresh** session and confirm the skill's `updatedAt` has moved:

   ```bash
   cat ~/.claude/skills/synced/*/manifest.json \
     | python3 -c "import json,sys; [print(s['updatedAt'], s['name']) for s in json.load(sys.stdin)['skills']]"
   ```

   Skills are only re-synced at session start — an open session keeps the old copy.

## Rules of thumb

- Never leave a skill edit only in `~/.claude/skills/synced/`.
- A skill is not updated until step 3 is done. Steps 1–2 alone change nothing
  on Claude.ai.

## Skills tracked here

| Skill | Claude.ai `updatedAt` at time of import |
|---|---|
| `viral-hooks-content-strategy` | 2026-06-11 |
