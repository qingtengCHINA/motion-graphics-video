---
name: motion-graphics-video
description: Produce portfolio-grade motion graphics videos (15/30/60/custom seconds): documentary evidence flow + liquid-glass design system + data-driven timeline + TTS narration + chunked rendering + full acceptance. Use when the user asks for motion graphics video / portfolio video / motion graphics / portfolio video / narrated promo / model-history video. Before starting, ask the user three things: brief/topic, main model, TTS model; recommend the Qwen family.
---
# Motion Graphics Video · portfolio-grade motion graphics

Treat "a video" as a piece of motion design — like a portfolio piece on a résumé. Use any tool or technique you believe produces the best result. The agent has full creative freedom: pursue outstanding visual design, bold art direction, striking composition and powerful motion; make every second count. Go all in.
This Skill is distilled from the complete craft of a 201-second documentary-grade film (fact-check → evidence capture → motion study → visual system → data-driven timeline → narration read-back → chunked render → four-track mix → item-by-item acceptance).

---

## 0. Before starting: three questions you must ask (never skip)

Collect them in one pass with AskUserQuestion (or equivalent); do not start if any is missing:

1. **Brief / topic** — what does the video say? Target audience and destination? Any facts, products, people or brand colors that must appear?
   Offer duration options: `15 / 30 / 60 / custom` seconds (default 60). Duration is a budget, not a hard value: the final cut is derived from content density and narration length.
2. **Main model** — the model doing planning, copy, code and orchestration.
   **Recommend the Qwen family** (e.g. Qwen4-Max / Qwen3.8-Plus): long context suits whole-film timelines and multi-file projects; Chinese copy and fact-checking are stable. The user may name another model; respect their choice.
3. **TTS model** — the model voicing the narration.
   **Strongly recommend Qwen TTS speech** (e.g. `qwen-audio-3.0-tts-plus`): supports voice and tone instructions; natural documentary tone. Also confirm: voice, tone instruction, language.

Also confirm (defaults allowed): visual style preference, resolution/fps (default 1920×1080@30), music needed (default yes), sound effects (default **none** — synthesized SFX read cheap unless explicitly requested), and any reference files/images/URLs the user provides.

**Key discipline**: any API key goes only into the project `.env`, added to `.gitignore`; never into docs, memory, logs, or chat echoes.

---

## 1. Master prompt (optimized · slots follow user input)

Hand the following prompt to the executing agent as the creative charter; `{{…}}` are slots filled from section 0:

```text
Produce a {{DURATION}}-second motion graphics video that shows your level as a motion designer, like a portfolio piece on a résumé.
Topic: {{THEME}}.
You have full creative freedom; use any tool or technique that produces the best result; pursue outstanding visual design,
bold art direction, striking composition and powerful motion; make every second count. Go all in.

Planning and code by {{MAIN_MODEL}}; narration synthesized by {{TTS_MODEL}} (voice {{VOICE}}, tone {{TTS_INSTRUCTION}}).
Visual style: {{STYLE}}; aspect {{RESOLUTION}}@{{FPS}}fps; music {{MUSIC}}; SFX {{SFX}}.

If you need UI, symbols, real material, video assets or background music, find, download and use them online yourself;
you must open and consult every file, image and URL the user gave: {{REFERENCES}}.
First browse the motion cases at https://snapcn.dev/docs/components and learn broadly before building; member components: learn the idea only, never bypass payment or verification.

Craft requirements (non-negotiable):
1 Facts: every on-screen number/date/quote needs a source URL and as-of date; events within the past year need ≥2 independent sources; wording must not outrun evidence.
2 Evidence: real screenshots/recordings/paper pages shown full-bleed or in glass floats; record DOM anchors at capture so push-ins and underlines always land on the words;
  prefer Wayback snapshots for historical shots; hide popups at capture; never bypass anti-bot or captcha.
3 Shots: three sizes (WIDE/MID/MACRO); each shot move→settle (≥18 frames, only 0.5% breathing drift)→exit;
  smooth bezier acceleration into place, linear camera motion forbidden; zoom cap 1.8×, emphasized content fully inside safe areas.
4 Continuity: Gestalt continuity — no element appears/disappears abruptly; entrances and exits animated; fixed transition vocabulary, each with meaning.
5 Layout: safe areas ≥96px; ≤2 text layers on screen (subtitle + one title or one number); subtitle fixed position, per-glyph fill, never animated by other motion;
  on-screen text never repeats narration — only dates/models/values/sources; no sentence-final punctuation on screen.
6 Material: cards/floats/panels share liquid glass (frost≈18px, rim≈0.22, bend≈0.45, dispersion≈0.35);
  palette drawn from the topic (7–8 colors), accent only as highlighter; keep grain and vignette; motion blur and chromatic aberration only for fast motion.
7 Rhythm: hard cuts snap to music beats (±3 frames); question cards/titles hold ≥1.5s after typing; lists and walls hold ≥3s;
  duration follows content — never flash-cut to pad time, never cut holds to squeeze time.
8 Sound: narration −16 LUFS; music ducks 6–9dB under narration (attack≈150ms/release≈400ms); chapter mood via low-pass/silence breaths;
  no SFX by default; bus true peak ≤ −1 dBTP.
9 Engineering: timeline generated by script from narration durations; scenes read relative frames only; chunk boundaries at shot cuts; render failures retry with reduced concurrency.
10 Delivery: deliver exactly one video file + one complete script (shots/narration/on-screen text/assets/motion/sources); intermediates move to trash, never deleted.

Acceptance (measure the delivered file, item by item): frame count = timeline; resolution/fps; I≈−16 LUFS and TP≤−1 dBTP; blackdetect shows no unintended true black;
no static stretch >1.5s; chunk seams show no duplicated/dropped frames; ≥70% of hard cuts on beat; contact sheet at each shot's settle midpoint inspected by eye;
moving shots checked in 4-frame sequences for easing and blur; narration read back line by line (word-level timestamps or local Whisper); source list matches what actually appears.
```

### Slot table (flexible with user input/choices)

| Slot | Source | Default |
| --- | --- | --- |
| `{{DURATION}}` | duration choice in section 0 | 60 |
| `{{THEME}}` | user's brief verbatim | required |
| `{{MAIN_MODEL}}` | user choice | Qwen3-Max (recommended) |
| `{{TTS_MODEL}}` / `{{VOICE}}` / `{{TTS_INSTRUCTION}}` | user choice | qwen-audio-3.0-tts-plus / steady documentary voice / steady, magnetic, grave; moderate pace |
| `{{STYLE}}` | user choice | liquid-glass documentary |
| `{{RESOLUTION}}` / `{{FPS}}` | user choice | 1920×1080 / 30 |
| `{{MUSIC}}` / `{{SFX}}` | user choice | needed (CC-licensed track) / none |
| `{{REFERENCES}}` | user-provided files/images/URLs | empty → research online yourself |

---

## 2. Craft pipeline (execution order)

1. **Ask the three elements** → write `data/brief.json`.
2. **Fact-check** → `data/facts.md`: per-item URL + as-of date + measurement differences; wording correction table (delete "first / matches / surpasses" overclaims unless the evidence quotes them).
3. **Assets & anchors** → Playwright dpr2 full-page screenshots + `anchors.json` (DOM bounding boxes / word-level Ranges) + `meta.json` (capturedAt/sha256); scroll shots capture per-frame `scrollTo` screenshots for 30fps smoothness; terminal/thinking-stream text must come from real execution or official public examples.
4. **Motion study** → browse the snapcn component library, extract primitives (phrase spring entrances, spec sweeps, number slams, line riders, model-wall drift, karaoke subtitles…), implement yourself; never copy member code.
5. **Visual system** → palette/fonts/glass material/safe areas/transition vocabulary/texture zones (one texture and one signature shot per chapter).
6. **Narration** → per-line TTS + pronunciation replacement table (e.g. QwQ→Q W Q) + silence trimming + loudness normalize; local Whisper read-back comparison, re-synthesize low-scoring lines.
7. **Beats & timeline** → librosa beat map; shot duration = max(budget, lead-in + narration + tail); cuts snap to beats; chunk boundaries = cuts; emit `timeline.gen.json`, scenes read relative frames only.
8. **Render** → Remotion chunked render (CRF16, yuv420p, BT.709), retry with reduced concurrency on failure; concat then mux.
9. **Mix** → narration/music two tracks (no SFX by default); ducking + chapter low-pass + pre-line silence; two-pass loudnorm (I=−16, TP≤−2 leaving AAC headroom).
10. **Acceptance & delivery** → measure every item of the section 1 acceptance list; contact sheet + motion-sequence eye check; clean intermediates (to trash); deliver single video + script.md.

---

## 3. Hard constraints

- Keys only in `.env` (in `.gitignore`); zero echo in docs/logs/chat.
- Deletions always move to `~/.Trash`; `rm` forbidden.
- Never bypass anti-bot, captcha or paywalls; when blocked, switch source and note it in the fact table.
- Deliver exactly one video file; recreated frames must carry an "illustrative animation" corner tag.
- On-screen text separate from narration; no sentence-final punctuation on screen; narration numbers in Arabic digits without thousands commas.

## 4. Common failures and fallbacks

- Lazy-loaded page missing anchors → pre-scroll before capture + `scrollIntoViewIfNeeded` retry for missing anchors; scene hold points/frame counts computed from actual anchors to keep audio-picture sync.
- Inner-scroll container defeats fullPage → `fullPage:true` + clip; very long pages crop around anchors and rebase anchor coordinates.
- Popup/redirect modal → hide via CSS at capture, or locate by text and hide its fixed/absolute ancestor; never click buttons that navigate.
- Device-shell screen opaque → place the recording layer above the shell PNG, cropped by the template's screen percentages.
- Zoom crops key text → zoom cap 1.8× + focus rect fully inside safe areas; panel crop windows align cx/cy to panel center.
- Static-detection false positives → dual metrics (mean + changed-pixel ratio) at 960×540@10fps; settle keeps 0.5% breathing drift.
- AAC true peak over → leave 0.5–1dB headroom in loudnorm target TP (e.g. −2.2); re-measure after muxing.