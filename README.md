# /cutroom

**You built it. Now cutroom.**

`/cutroom` is an agent skill that turns the project you created into a short, shareable launch video — music, motion, and share copy included. One command, powered by [Hyperframes](https://hyperframes.heygen.com/).

## New: `/cutroom-slim`

**The same /cutroom, rebuilt lean for Opus 5.5.**

A smooth launch video, designed for your specific project, with its own soundtrack and share copy.

No Hyperframes, no bundled assets, same creative rules.

Just tell Opus 5.5: *let's /cutroom about this.*

On Opus 5.5, `/cutroom` switches to `/cutroom-slim` automatically. Run `/cutroom --full` to keep the classic Hyperframes workflow.

**Install just `/cutroom-slim`:**

```bash
npx skills add https://github.com/ahsandotexe/cutroom --skill cutroom-slim
```

Already have the Claude Code `/cutroom` plugin? `/cutroom-slim` is included from version 0.4.0. Run `claude plugin update cutroom` to get it.

## Install /cutroom

**Codex:**

```bash
codex plugin marketplace add ahsandotexe/cutroom
codex plugin add cutroom@cutroom
```

To update it later:

```bash
codex plugin marketplace upgrade cutroom
codex plugin add cutroom@cutroom
```

**Claude Code:**

```bash
/plugin marketplace add ahsandotexe/cutroom
/plugin install cutroom@cutroom
```

Then run `/cutroom` inside any project. The plugin includes `/cutroom-slim` too.

**Any other agent** — one command via the [`skills`](https://github.com/vercel-labs/skills) CLI (Cursor, Codex, Copilot, Gemini CLI, opencode, and more):

```bash
npx skills add https://github.com/ahsandotexe/cutroom --skill cutroom
```

Add `-g` to install globally (available in every project); drop it to scope to the current one. ([browse on skills.sh](https://www.skills.sh/ahsandotexe/cutroom/cutroom))

<details>
<summary>No installer? Copy the skill directly.</summary>

```bash
rsync -a --exclude '.DS_Store' skills/cutroom/ ~/.claude/skills/cutroom/
rsync -a --exclude '.DS_Store' skills/cutroom-slim/ ~/.claude/skills/cutroom-slim/  # optional: the /cutroom-slim command
```

Restart Claude Code after copying.
</details>

### Also works with

This repo exposes the skill at every agent's standard discovery path via symlinks. No extra config needed.

| Agent | How it discovers |
|---|---|
| **Google Antigravity** | Auto-detects from `.agents/skills/cutroom/` at project root or `~/.gemini/config/skills/cutroom/` globally |
| **opencode** | Auto-detects from `.opencode/skills/cutroom/` at project root |
| **Codex CLI** | Reads `.agents/skills/cutroom/`, walking up to repo root |
| **Claude Code** | Also reads `.claude/skills/cutroom/` (in addition to the `.claude-plugin/` marketplace install above) |
| **Other agents** | Point custom instructions at `skills/cutroom/SKILL.md` — see [`other-agents.md`](other-agents.md) |

> **Windows users:** Git requires `git config core.symlinks true` (or `git clone -c core.symlinks=true`) and Windows Developer Mode or Administrator privileges to create symlinks. If symlinks don't work on your system, copy `skills/cutroom/` to the agent's skill directory manually instead.

## Use it

From any project directory, ask your agent:

```text
let's /cutroom
```

Or steer the tone:

```text
/cutroom --tone "fake Series A launch from 2016"
```

Voiceover is off by default. Enable it explicitly with:

```text
/cutroom --voice
```

Narration uses Kokoro through Hyperframes when enabled.

You get a `cutroom-output/` folder with the plan, a composition brief, share copy, and the rendered `cutroom.mp4`.

## How it works

`/cutroom` owns the story — the product angle, tone, and which moments to show. It hands a focused brief to [Hyperframes](https://hyperframes.heygen.com/), which builds, times, and renders the video.

## Requirements

- An agent that supports Agent Skills — Claude Code, opencode, Codex CLI, or any agent with custom instructions (see "Also works with" above)
- Node.js 22+
- FFmpeg on `PATH`
- Hyperframes CLI — `npx hyperframes` (check it with `npx hyperframes doctor`)

## What's in this repo

- `skills/cutroom/` — the skill, references, and bundled music + SFX
- `skills/cutroom-slim/` — `/cutroom-slim`, the single-file skill for Claude Opus 5.5
- `examples/` — fake product sites used as a benchmark suite
- `plugin.json` — portable Agent Plugins manifest
- `.codex-plugin/` — Codex compatibility manifest
- `.claude-plugin/` — Claude Code manifest + shared marketplace catalog (also recognized by Codex)
- `.claude/skills/cutroom/` — symlink → `skills/cutroom/` (Claude Code discovery)
- `.agents/skills/cutroom/` — symlink → `skills/cutroom/` (Codex CLI + opencode discovery)
- `.opencode/skills/cutroom/` — symlink → `skills/cutroom/` (opencode discovery)

## Contributing

Contributions, ideas, and new demo cutrooms are welcome — open an issue or a PR.

## Star History

<a href="https://www.star-history.com/?type=date&repos=ahsandotexe%2Fcutroom">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=ahsandotexe/cutroom&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=ahsandotexe/cutroom&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=ahsandotexe/cutroom&type=date&legend=top-left" />
 </picture>
</a>

## Credits

- Music — [ende.app](https://ende.app/en) "Happy Beats / Business Moves" (CC BY 4.0)
- Sound effects — [Kenney](https://kenney.nl/)
- Video generation — [Hyperframes](https://hyperframes.heygen.com/)
- Fake demo sites — built with [Impeccable](https://impeccable.style/)
- Based on [latent-spaces/brag](https://github.com/latent-spaces/brag) (MIT)
