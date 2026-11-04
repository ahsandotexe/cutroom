# Editing rules

These rules apply to every cutroom video, whatever style is chosen. A style file decides how the video looks. This file decides how it is cut. When a style file and this file disagree, the style file wins on looks (colors, fonts, textures) and this file wins on craft (composition, timing, readability, delivery). The one exception: a style file may relax a pacing rule in a section titled "Pacing note", and that note applies for that style only. The first-two-seconds rules and the minimum sizes are never relaxed.

Cut every video as a senior motion editor with 10 years of short-form experience would: restraint over decoration. If an effect does not make the message clearer or the edit tighter, leave it out.

## Contents
1. Format and delivery
2. Composition and scale
3. The first two seconds
4. Typography in motion
5. Motion
6. Pacing
7. Voiceover
8. Sound
9. Endings and loops
10. Review checklist before rendering

## 1. Format and delivery
- Default to vertical 9:16, 1080x1920, 30 fps, unless the user asks for another format.
- Export H.264, at least 16 Mbps. Audio normalized to -14 LUFS integrated, true peak no higher than -1 dBTP.
- Muted-first: the story must work with sound off. Every spoken line also appears on screen.
- Safe area for vertical: keep all text and key visuals out of the top 250px and bottom 350px, where platform UI sits.

## 2. Composition and scale
This is the most common failure. Content that looks fine on a laptop is unreadable on a phone.
- Vertically center each scene's content group inside the safe area. Never leave the bottom half of the frame empty while content crowds the top.
- Minimum sizes at 1080 width:
  - Hero number or hero word: 75 to 80 percent of frame width.
  - Headlines: 64px or larger.
  - Body and supporting lines: 44px or larger.
  - Labels and small captions: 30px or larger. Nothing on screen is ever smaller than 30px.
  - Chart bars: at least 48px tall, row spacing at least 36px, names and values at least 40px.
  - Product, app or gameplay footage: at least 85 percent of frame width, framed in a card or device.
  - End card URL or call to action: 110px or larger.
- One hero element per frame, at most one supporting line, at most one label.
- Pick one margin and keep every scene on it, so cuts feel continuous.

## 3. The first two seconds
Viewers decide in the first second. Treat the opening as its own deliverable.
- Frame 1 must contain a readable hook: large text, ideally also spoken. Never open on an unexplained visual floating in empty space.
- The cold open runs 2 to 2.5 seconds, fills the frame, and shows the most striking moment of the product or story, repeated or escalated (for example three hits, not one).
- The first frame must work as a thumbnail on its own.
- Do not spend the opening on logos, titles or setup.

## 4. Typography in motion
- Reveal text word by word, sliding up about 24px from behind a mask, with 80 ms between words.
- Hold every line for at least (word count / 3) + 0.5 seconds after it finishes animating in.
- Use tabular figures for any number that counts or changes, so digits do not jitter.
- Captions match the spoken words exactly.

## 5. Motion
- Entrances: ease-out, cubic-bezier(0.22, 1, 0.36, 1), 350 to 450 ms.
- Exits: ease-in, 200 ms.
- Moves between positions: ease-in-out, 400 ms.
- Nothing moves linearly except the progress of a counter, which uses an exponential ease-out so it slows into its final value.
- Stagger groups by 80 to 120 ms. Never animate everything at once.
- No dead frames: every static hold gets a slow 2 percent scale push-in.
- Motion blur on fast moves: rolling counters, speed ramps, whip transitions.
- Prefer match cuts, where one element becomes or leads into the next scene's element. Avoid generic fades, spins and zoom-blur transitions.

## 6. Pacing
- When there is a voiceover, the voice sets each scene's length. The scene's key animation lands on the key spoken word, and the scene cuts no more than 0.5 seconds after the line ends (end card excepted).
- Without a voiceover, no static hold runs longer than 1.5 seconds after its text finishes animating.
- A setup line ("And here's the problem...") holds for 2 seconds at most.
- Use silence on purpose: drop the music for about half a second right before the biggest reveal.

## 7. Voiceover
- Voiceover is opt-in: add one only when the user asks for it (for example with `--voice`). Without one, the on-screen text carries the story. The rules below apply whenever there is a voiceover.
- If the user supplies recordings, expect one file per line (for example vo/01.mp3, vo/02.mp3) and place each at the start of its scene. Never guess line boundaries inside one long file if separate files are available.
- If a line is missing, generate it with the TTS option as a placeholder and tell the user which lines are placeholders.
- Duck music by about 8 dB under the voice.
- Write VO lines short: one idea per line, ideally under 12 words.

## 8. Sound
- Music cuts on downbeats.
- Layer small, purposeful effects: a soft whoosh on major moves, a low sub hit when a key number or word lands, light ticks while a counter rolls (slowing as it slows), and the product's own sounds only on product footage.
- Never let sound effects mask the voice.

## 9. Endings and loops
- The end card holds at least 2.5 seconds with a slow push-in.
- One call to action, stated once, in the user's exact wording.
- The final frame cuts cleanly back into the first so the video loops; a rewatch counts as watch time.

## 10. Review checklist before rendering
Write the scene plan, timings, data used, VO lines and sound cue list to the output folder and stop for the user's review. In the plan, confirm each item:
- [ ] Frame 1 has a readable hook.
- [ ] Cold open is 2 to 2.5 seconds and fills the frame.
- [ ] Every scene's content is vertically centered; no half-empty frames.
- [ ] Nothing on screen is smaller than 30px at 1080 width.
- [ ] Every hold respects the reading-time rule and the 1.5 second dead-time limit.
- [ ] If there is a voiceover, VO lines map one to one to scenes, with key animations on key words.
- [ ] All live data was pulled fresh, and any names or content the user excluded are absent from every frame.
- [ ] The ending loops back to the start.
