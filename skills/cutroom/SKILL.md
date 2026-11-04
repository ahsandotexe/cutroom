---
name: cutroom
description: Turn the current project website into a short, polished, shareable launch video using Hyperframes. Use when someone says "/cutroom", "let's cutroom about this", "make a launch video", "turn this into a video", or wants to share what they built. Reads the project code directly — no live URL or screenshots needed.
---

# /cutroom

You built it. Now let's cutroom about it.

## Invocation dispatch (must happen first)

**Model check.** If you are Claude Opus 5.5 and the invocation doesn't ask for the full workflow (`--full`, "use the full cutroom") or for voiceover (`--voice`, which cutroom-slim doesn't do), switch to cutroom-slim: read `<skill-dir>/slim.md` (the /cutroom-slim skill, bundled here) and follow it for the rest of this run instead of this file. Pass along the user's input, and pass any other options (`--no-music`, `--title`, …) as plain-language direction. Tell the user in one line first, e.g. "You're on Opus 5.5, so I'm using /cutroom-slim: I build the whole video myself. Say 'use the full cutroom' to switch back." If you are any other model, or can't tell which model you are, skip this check.

If the invocation is `--style list`, print the style list (see "Styles" below) and stop. Don't inspect the project or start a run.

Before inspecting the project, parse the complete `/cutroom` invocation. If the
invocation contains `--voice`, set `voice.enabled = true`. Enable narration
only for that run. Do not enable narration automatically and do not fall back
to the normal no-voice workflow.

`/cutroom` turns the current project website or app into a short, polished, shareable launch video using Hyperframes. It is narrow, opinionated, and fun.

## What this skill does

1. Reads the current project code to understand the app.
2. Plans a short cutroom concept specific to this project.
3. Scripts and storyboards the video.
4. Hands a focused composition brief to Hyperframes.
5. Validates, renders, and writes share copy.

## Parsing the invocation

The user may invoke with natural language or flags:

```
/cutroom
/cutroom --tone chaotic
/cutroom --tone polished --format vertical
/cutroom --style slap-wrapped
/cutroom --style list
/cutroom this. Make it feel like a ridiculous startup launch.
```

Parse these options:

| Option | Values | Default |
|---|---|---|
| `--tone` | preset or freeform description | inferred |
| `--format` | `landscape`, `vertical`, `square` | `vertical` (1080x1920, per `references/editing-rules.md`) |
| `--duration` | seconds | auto (15-25s) |
| `--no-music` | flag | music on |
| `--no-sfx` | flag | sfx on |
| `--title` | string | inferred from project |
| `--voice` | flag | narration off |
| `--style` | style name, or `list` | picked to fit (see "Styles") |

Voice is opt-in. If `--voice` is present, use Kokoro via Hyperframes and do
not add any provider-selection logic. The voice workflow is intentionally
single-provider.

Tone can be a preset (`default`, `polished`, `yc-parody`, `chaotic`, `deadpan`, `cinematic`, `app-store`) or a creative direction such as "fake Series A launch from 2016", "museum exhibit", or "overproduced mobile game ad".

When the user gives freeform tone direction, map it to the nearest preset for pacing and structure, but preserve the user's direction in the plan and composition brief.

## Styles

A style sets how the whole video looks: colors, type, layout, texture, footage framing and signature moves. Each style is one file in `<skill-dir>/references/styles/`. `_template.md` is the blank template for new styles, never a style itself.

- `--style list`: for every style file except `_template.md`, print its name (the file name without `.md`) and a one-line description (the first sentence under its `# Style:` heading). Then stop.
- `--style <name>`: read `references/styles/<name>.md` and apply it to the whole video. If there's no such file, say so and print the list.
- No `--style`: read the "Use it for" and "Avoid it for" sections of every style file and pick the best fit. If none fits, design a look of your own; it still follows `references/editing-rules.md`.

Either way, the plan names the style used and says why in one or two sentences.

## Narration guidance

When `--voice` is enabled, write narration that complements the visuals, does
not simply read visible text, matches scene pacing, sounds natural and
conversational, and moves smoothly between scenes. Keep the script concise and
specific to the product so the voice feels like part of the edit rather than a
separate narration track.

---

## Output directory

By default, output goes to `cutroom-output/`. To avoid overwriting previous runs, use a timestamped directory:

```
cutroom-output-2026-05-04-143022/
```

Use a timestamp when:
- The user explicitly asks for a new run without overriding previous results
- A `cutroom-output/` directory already exists in the project

Generate the timestamp at the start of the run (`YYYY-MM-DD-HHmmss`) and use it consistently for all output paths in that run: plan, brief, composition, render, and share copy.

## Skill directory

`<skill-dir>` is the directory containing this `SKILL.md`. Claude Code prints it as "Base directory for this skill" when the skill loads; for other agents it's wherever the skill was installed. Bundled assets are under `<skill-dir>/assets/` and scripts under `<skill-dir>/scripts/`. Don't guess an install path: a plugin install, a `~/.claude/skills/` copy, and this repo all put it somewhere different.

---

## Step 1: Inspect the project

**Read:** [references/step-1-inspect.md](references/step-1-inspect.md)

Scan the project directory and extract the information needed to plan the cutroom video.

**Gate:** You can answer all 9 questions in the cutroom planning rubric.

---

## Step 2: Plan and storyboard

**Read first, every run:** [references/editing-rules.md](references/editing-rules.md). These rules apply to every video, whatever the style. Read them before writing any scene plan.
**Read:** the chosen style file in `references/styles/` (see "Styles")
**Read:** [references/step-2-plan.md](references/step-2-plan.md)

Write `<output-dir>/cutroom-plan.md` (where `<output-dir>` is `cutroom-output/` or the timestamped variant chosen above). Answer the planning rubric. Commit to a creative angle. Write the beat-by-beat storyboard including scenes, text, timing, transitions, and SFX cues.

When music is selected, include a compact `Music cue guidance` section: read the bundled track's cue preset from `<skill-dir>/assets/music/cues/` if present, otherwise note cues will be detected at composition time (any track now supports beat sync — see `references/audio.md`). Cue metadata is optional timing guidance only: story, readability, pacing, and product clarity stay primary.

The plan also states the chosen style and why, and ends with the review checklist from `references/editing-rules.md` (section 10), every item confirmed.

Then stop and show the user the plan. Don't start Step 3 until they approve it or ask for changes; apply any changes to the plan first.

**Gate:** `<output-dir>/cutroom-plan.md` exists with a full storyboard, the style choice, and the confirmed review checklist. Scene durations sum to 15–25 seconds. The user has approved the plan.

---

## Step 3: Hand off to Hyperframes

**Read:** The Hyperframes skills — `hyperframes` (contract, `references/format.md`, `references/motion.md`, `references/look.md`), `hyperframes-cli`, `hyperframes-registry`, and `media-use` for audio. /cutroom is its own workflow: skip the `hyperframes` storyline step: don't read `references/types/*` or `references/story.md`, don't propose storylines, and don't write a `SPEC.md`. The cutroom plan and composition brief replace them, including the LOOK line `references/look.md` asks for. Also skip its `usage`, `history`, `feedback` and REVIEW.md steps; `check` is cutroom's gate. Do not route into its generic launch-video workflow.
**Read:** [references/step-3-compose.md](references/step-3-compose.md)
**Read:** [references/audio.md](references/audio.md)

Write the composition brief and use Hyperframes to create the video implementation in `<output-dir>/composition/`.

`/cutroom` owns the product angle, source material, storyboard, tone, format, audio selection, music cue guidance, and delivery expectations. Hyperframes owns the concrete composition structure, exact animation timing, animation mechanics, runtime choices, linting rules, and render workflow.

**Gate:** `npx hyperframes check` passes with zero errors inside `<output-dir>/composition/` (the single browser gate before render — see hyperframes-cli for what it audits).

---

## Step 4: Validate, render, and deliver

**Read:** [references/step-4-deliver.md](references/step-4-deliver.md)

Validate, preview, render to `<output-dir>/cutroom.mp4`, pick the best poster frame into `<output-dir>/cutroom.jpg`, bake that poster as the video's frame 0 so it's the idle thumbnail everywhere, and write `<output-dir>/share-copy.txt`.

**Gate:** `<output-dir>/cutroom.mp4` exists. A best-frame poster `<output-dir>/cutroom.jpg` is picked (not an arbitrary frame) and baked as frame 0 of `cutroom.mp4`. Share copy is written.

---

## Tone system

Seven tone presets ship with `/cutroom`. Each changes scripting energy, pacing, typography personality, and transition style. Presets are defaults, not limits.

Full definitions: [references/tones.md](references/tones.md)

| Tone | Energy | One-liner |
|---|---|---|
| `default` | Playful, clean, postable | The good-vibes default |
| `polished` | Serious, elegant | For projects that are not jokes |
| `yc-parody` | Deadpan startup energy | Fake seriousness applied to absurd projects |
| `chaotic` | Fast, loud, aggressive | Over-the-top and unhinged |
| `deadpan` | Calm, dry, understated | The joke is that nothing is a joke |
| `cinematic` | Dramatic, trailer-scale | Big motion, bigger claims |
| `app-store` | Smooth, feature-card clean | Corporate but not boring |

Always allow a freeform creative direction to refine or override the preset.

---

## Creative laws

These apply to every cutroom video regardless of tone.

**Short.** 15–25 seconds. Not one second more without a reason. This holds whether or not narration is on; narration does not extend the window.

**Readable.** Keep the pace high through motion and cuts, never by flashing text. Every line a viewer must read holds long enough to read it (short label ~0.8s settled; a sentence ~0.3s per word). Fast-in, then hold — never fast-in, then gone.

**Specific.** The video must feel like it was made for this exact project, not any project.

**Show the thing.** At least one scene must display actual UI, copy, or a key visual from the product. No abstract filler.

**No generic SaaS language.** "Streamline your workflow" is banned. Use the project's actual copy and claims.

**The hook is everything.** The first 2 seconds determine whether someone keeps watching. Plan the hook before anything else.

**Funny earns its place.** Humor should come from the project's absurdity, not from trying to be funny.

**Pattern:**
```
Hook (2-3s) → Reveal (2-4s) → 2-3 sharp highlights (5-12s) → Punchline/outro (2-4s)
```

Adapt this. Not every project needs exactly 3 highlights. The pattern is a starting shape, not a template.
