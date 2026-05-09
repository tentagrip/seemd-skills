# seemd-skills

AI agent skill bundle for [seemd](https://seemd.xyz) — share local markdown files as public URLs by saying "공유해줘" / "share this".

## Install

```sh
npx skills add tentagrip/seemd-skills
```

The [`skills` CLI](https://skills.sh) auto-detects whatever agent you use (Claude Code, Cursor, Codex, OpenCode, GitHub Copilot, Windsurf, Gemini, Cline, …) and installs to the right path.

Project-scoped (default) drops the skill into your repo so teammates pick it up; pass `-g` for global install.

## What you get

After install, your agent triggers on phrases like:

- "share this markdown / share this doc / give me a link / send this as a URL"
- "publish this / make a public page for this"
- "공유해줘 / 이 README 공유 / 링크로 보내줘"

…and runs:

```sh
npx @tentagrip/seemd <path-to-md-file>
```

The CLI uploads the markdown file (and every relative `.md` it links to), rewriting cross-document links to slug URLs so navigation works on the shared page. Returns a URL like `https://seemd.xyz/<slug>`. Documents auto-expire after 3 days.

## What's in this bundle

| Skill | Description |
|---|---|
| [`seemd`](./skills/seemd/SKILL.md) | Share a markdown file as a public URL via the `@tentagrip/seemd` CLI. |

## Manual install

If you don't want to use `npx skills`, copy the skill file directly into your agent's location:

| Agent | Destination |
|---|---|
| Claude Code | `~/.claude/skills/seemd/SKILL.md` |
| Cursor | `.cursor/rules/seemd.mdc` (or `~/.cursor/rules/seemd.mdc`) |
| Codex / generic AGENTS.md | append `skills/seemd/SKILL.md` body to your `AGENTS.md` |

```sh
mkdir -p ~/.claude/skills/seemd
curl -fsSL https://raw.githubusercontent.com/tentagrip/seemd-skills/main/skills/seemd/SKILL.md \
  -o ~/.claude/skills/seemd/SKILL.md
```

## Related

- **Source**: https://github.com/tentagrip/seemd
- **CLI on npm**: [`@tentagrip/seemd`](https://www.npmjs.com/package/@tentagrip/seemd)
- **Service**: https://seemd.xyz

## License

MIT
