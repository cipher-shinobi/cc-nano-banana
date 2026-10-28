---
name: nano-banana
description: >
  Generate and edit still images with Nano Banana (Gemini image models through
  the Gemini CLI nanobanana extension). Gemini is a permitted still lane, not
  the default. GPT Image (gpt-image-2.5-sunburst via images.edit) is the
  default image lane under the Kage's rulings of 2026-10-06 and 2026-10-28,
  so route a general image request there instead (prompt design with
  chatgpt-image-designer; PromptObject execution and filing with image-forge).
  Use this skill when the user names Nano Banana or Gemini, or a brief gives a
  reason to make or edit a still in Gemini. Never for video (Seedance only).
  No Gemini comparison passes unless the Kage asks.
license: "MIT"
metadata:
  cs-upstream: "https://github.com/kkoppenhaver/cc-nano-banana"
  cs-pin: "3b136993b1218c3f3bb5d1489b3c1ecb441dc842"
  cs-tree: "b5c375ea9f948209eacf0801d63843ded13189be"
  cs-gate: "CS.AK.LISA.Intel.SkillGate.MediaLaneAlignment, D6185, D6221 and D6253; CS.AK.LISA.Intel.NanoBananaTest0Fixes, D6222 and D6241; Kage yes 2026-10-28"
  cs-edits: "C1, C2, E1, E2, E3, E4, E5, E6, E7, E8, E9, E10, N1 v2, N2 v2"
  cs-ext-upstream: "https://github.com/gemini-cli-extensions/nanobanana"
  cs-ext-pin: "5badc5aafea8751fe059a054b629cf2d989bceb1"
  cs-ext-release: "v1.0.12 nanobanana-release.tar.gz sha256:c37b87f29c4a776e7a4d29a735aa333a05740f21a967835bfad2609a31322f39"
  cs-ext-tree: "28542d49968adab0a73fe9cb903346b9bef70409178958ede8c957b9e896e281"
  cs-ext-license: "Apache-2.0"
  cs-cli: "@google/gemini-cli 0.64.0 (npm, nvm node v24.15.0), sha512-FHrZ1NPf9QszyVMaa/BP3SsPy40ogBLnbZNWkv/PMi1Uz2zoDNMLGiBzBGmx4Bl7iLRq01K9PY/5kmloXFTpsA==, package tree 3e1362e0e49404b3935f190d9e71c44725a61be3171879f82d1ae3ac75d1c989; policy and settings verified on this version only"
---

# Nano Banana Image Generation

Generate professional images via the Gemini CLI's nanobanana extension.

## When to Use This Skill

> **CS lane rule (Kage, 2026-10-06 and 2026-10-28).** GPT Image is the default still lane (`gpt-image-2.5-sunburst` via `images.edit`): design the prompt with `/chatgpt-image-designer`, execute and file a PromptObject with `/image-forge` (its provider list is `gpt-image-2` and `nano-banana-pro` today; Sunburst is not wired into it yet, so a Sunburst render runs through `images.edit` as the memory below describes). Gemini (this skill) is permitted for stills when the user names it or a brief gives a reason to switch. No Gemini comparison passes unless the Kage asks. Video is Seedance only (`/seedance-designer`), never Gemini or Veo. Media is never a 3D model. Source: memory `feedback_media_production_stills_to_seedance_no_3d`.

Use this skill when the user:
- Asks for Nano Banana or Gemini image generation by name
- Has a stated reason to make or edit a still in Gemini rather than GPT

**For complex prompt engineering** — structured P.I.C.T.U.R.E. framework, coherence validation, reference mapping, or iterative prompt design — use `/nano-banana-designer` first. It produces optimised prompts that this skill then executes.

## CS run rules (Skill Gate E4, gray-fox D6222 and D6241)

> **Never install or update the extension.** Never run `gemini extensions install`, `gemini extensions update`, `npm install -g @google/gemini-cli` or `brew upgrade gemini-cli` for this lane. Never run `npm install -g @google/gemini-cli`, `npm update -g` or an interactive `gemini` session that could auto-update it; the CLI is pinned at 0.64.0 (`cs-cli`). The extension is frozen at a verified pin (`cs-ext-pin`). If the pre-flight fails or the extension is missing, stop and tell LISA. The re-install procedure is `CS.AK.LISA.Intel.NanoBananaTest0Fixes` §R2.3: a plain copy of the verified bytes into the lane home. The Gemini CLI installer is never used, even locally.
>
> **Never run `gemini` any other way.** Never pass `--yolo`, `-y`, `--approval-mode yolo` or `auto_edit`, `--preview` or `--raw-output`. Never run it from the vault or from any repository. Every run uses the block below: a new empty run folder, a scrubbed environment and the lane-only Gemini home `~/.cache/cs-nanobanana/home` (never the Kage's `~/.gemini`), the CS admin policy (`cs/nanobanana-lane.toml`: only the seven nanobanana image tools may run; shell, file, web and memory tools are denied) and the CS lane settings (`cs/gemini-lane-settings.json`).
>
> **Inputs and outputs.** For `/edit` and `/restore`, set `NB_IN` to the input image. The block copies it into the run folder as `input.png`; refer to it by that bare name, because a path is denied by policy. Results land in `<run folder>/nanobanana-output/`. Copy the chosen file to its vault destination yourself (FCF name, ArtefactMap entry). Never point Gemini at the vault.

### Run block (one Bash call per run; change only `NB_CMD`, and `NB_IN` for edit or restore)

```bash
NB_CMD="/generate 'modern flat illustration of a developer at a laptop, no text' --count=1"
NB_IN=""
NB_SKILL="$HOME/.claude/skills/nano-banana"
NB_NODE="$HOME/.nvm/versions/node/v24.15.0/bin"
NB_CLI="$HOME/.nvm/versions/node/v24.15.0/lib/node_modules/@google/gemini-cli"
NB_LANE="$HOME/.cache/cs-nanobanana"
NB_HOME="$NB_LANE/home"
NB_EXT="$NB_HOME/.gemini/extensions/nanobanana"
NB_RUN="$NB_LANE/runs/run-$(date +%Y%m%dT%H%M%S)-$$"
nb_check() {
  [ "$("$NB_NODE/node" --version)" = "v24.15.0" ] || { echo "FAIL: node is not v24.15.0"; return 1; }
  [ "$(find "$NB_CLI" -type f -print0 | LC_ALL=C sort -z | xargs -0 shasum -a 256 | sed "s|  $NB_CLI/|  ./|" | shasum -a 256 | cut -c1-64)" = "3e1362e0e49404b3935f190d9e71c44725a61be3171879f82d1ae3ac75d1c989" ] || { echo "FAIL: gemini-cli 0.64.0 files changed (auto-update or reinstall?)"; return 1; }
  [ ! -e "/Library/Application Support/GeminiCli" ] || { echo "FAIL: system Gemini settings or policies exist and would override the lane"; return 1; }
  [ "$(shasum -a 256 < "$NB_SKILL/cs/nanobanana-lane.toml" | cut -c1-64)" = "3f5af8b2c8603be33908df1303a8f2d4d3092fb2a082da51d9209dd4953bb9fc" ] || { echo "FAIL: CS policy changed"; return 1; }
  [ "$(shasum -a 256 < "$NB_SKILL/cs/gemini-lane-settings.json" | cut -c1-64)" = "aa2120be115be79f8c36a48254dd17047b5683b17feb5f14c426a3274c879db4" ] || { echo "FAIL: CS lane settings changed"; return 1; }
  [ "$(shasum -a 256 < "$NB_EXT/gemini-extension.json" | cut -c1-64)" = "659d95ba67fdce7099eb3c262d83e12edb202b9f24496a7a083c8e20c3f9a0b1" ] || { echo "FAIL: lane extension manifest changed (literal key back?)"; return 1; }
  [ "$(find "$NB_EXT" -type f -not -name .env -not -name .gemini-extension-install.json -not -name gemini-extension.json -print0 | LC_ALL=C sort -z | xargs -0 shasum -a 256 | sed "s|  $NB_EXT/|  ./|" | shasum -a 256 | cut -c1-64)" = "28542d49968adab0a73fe9cb903346b9bef70409178958ede8c957b9e896e281" ] || { echo "FAIL: lane extension files changed"; return 1; }
  [ "$(ls -A "$NB_HOME/.gemini/extensions")" = "nanobanana" ] || { echo "FAIL: another extension sits in the lane home"; return 1; }
  { [ ! -s "$NB_EXT/.env" ] && [ ! -e "$NB_HOME/.env" ] && [ ! -e "$NB_HOME/.gemini/.env" ] && [ ! -e "$NB_HOME/.gemini/policies" ]; } || { echo "FAIL: an .env file or user policy folder would be loaded"; return 1; }
  [ -n "${GEMINI_API_KEY:-}" ] || { echo "FAIL: GEMINI_API_KEY not set (custody: ~/.claude/secrets/gemini-api-key via ~/.zshrc)"; return 1; }
}
nb_check && install -m 600 "$NB_SKILL/cs/gemini-lane-settings.json" "$NB_HOME/.gemini/settings.json" \
  && mkdir -m 700 "$NB_RUN" \
  && { [ -z "$NB_IN" ] || cp -- "$NB_IN" "$NB_RUN/input.png"; } \
  && ( cd "$NB_RUN" && env -i PATH="$NB_NODE:/usr/bin:/bin" HOME="$NB_HOME" GEMINI_CLI_HOME="$NB_HOME" LANG=en_GB.UTF-8 TERM=dumb \
         GEMINI_API_KEY="$GEMINI_API_KEY" \
         NANOBANANA_MODEL="${NANOBANANA_MODEL:-gemini-3.1-flash-image-preview}" \
         "$NB_NODE/gemini" --approval-mode default \
           --admin-policy "$NB_SKILL/cs/nanobanana-lane.toml" \
           --skip-trust -e nanobanana \
           -p "$NB_CMD" ) ; \
ls -la "$NB_RUN/nanobanana-output/" 2>/dev/null; echo "run folder: $NB_RUN"
```

After copying the chosen file into the vault, remove the run folder by its printed path:
`NB_RUN="<run folder>"; case "$NB_RUN" in "$HOME/.cache/cs-nanobanana/runs/run-"*) rm -rf -- "$NB_RUN" ;; esac`

## Command Selection

| User Request | Command |
|--------------|---------|
| "make me a blog header" | `/generate` |
| "create an app icon" | `/icon` |
| "draw a flowchart of..." | `/diagram` |
| "fix this old photo" | `/restore` |
| "remove the background" | `/edit` |
| "create a repeating texture" | `/pattern` |
| "make a comic strip" | `/story` |

## Available Commands

**CS:** each command below is the value of `NB_CMD` in the run block. Never run it as a bare `gemini` call.

| `NB_CMD` | Use Case |
|---------|----------|
| `"/generate 'prompt'"` | Text-to-image generation |
| `"/edit input.png 'instruction'"` (with `NB_IN`) | Modify existing image |
| `"/restore input.png 'fix scratches'"` (with `NB_IN`) | Repair damaged photos |
| `"/icon 'description'"` | App icons, favicons, UI elements |
| `"/diagram 'description'"` | Flowcharts, architecture diagrams |
| `"/pattern 'description'"` | Seamless textures and patterns |
| `"/story 'description'"` | Sequential/narrative images |
| `"/nanobanana prompt"` | Natural language interface |

## Common Options

- `--count=N` - Generate N variations (1-8)
- `--styles="style1,style2"` - Apply artistic styles
- `--format=grid|separate` - Output arrangement
- Never `--yolo` or `--preview` (CS run rules: `--yolo` is a fatal error under the CS settings, and preview is denied by policy)

## Common Sizes

| Use Case | Dimensions | Notes |
|----------|------------|-------|
| YouTube thumbnail | 1280x720 | `--aspect=16:9` |
| Blog featured image | 1200x630 | Social preview friendly |
| Square social | 1080x1080 | Instagram, LinkedIn |
| Twitter/X header | 1500x500 | Wide banner |
| Vertical story | 1080x1920 | `--aspect=9:16` |

## Model Selection

Default: `gemini-3.1-flash-image-preview` (~$0.04/image)

For higher quality (better reasoning):
```bash
export NANOBANANA_MODEL=gemini-3-pro-image-preview
```

## Default Resolution

All generations default to **4K resolution**. Append `4K resolution, ultra high detail` to every prompt unless the user explicitly requests a smaller size or a specific dimension.

## Blog Featured Image Examples

```bash
# Modern illustration style
NB_CMD="/generate 'modern flat illustration of developer coding at laptop, purple and blue gradient background, minimalist style, no text'"

# Professional photography style
NB_CMD="/generate 'professional editorial photo of coffee cup next to laptop on wooden desk, morning sunlight, shallow depth of field, no text' --count=3"

# Tech/abstract
NB_CMD="/generate 'abstract visualization of neural network connections, dark background with glowing blue nodes, futuristic style'"
```

## Icon Generation

```bash
NB_CMD="/icon 'minimalist app logo for productivity tool' --sizes='64,128,256,512' --type='app-icon' --corners='rounded'"
```

## Diagram Generation

```bash
NB_CMD="/diagram 'user authentication flow with OAuth' --type='flowchart' --style='modern'"
```

## Output Location

Generated images are saved to `nanobanana-output/` inside the run folder the block prints (`~/.cache/cs-nanobanana/runs/run-…`), never in the vault.

## Presenting Results

After generation completes:
1. Read the listing the run block prints (the run folder's `nanobanana-output/`)
2. Present the most recent image(s) to the user
3. Offer to regenerate with variations if needed

## Refinements and Iterations

When the user asks for changes:
- **"Try again" / "Give me options"**: Regenerate with `--count=3`
- **"Make it more [adjective]"**: Adjust prompt and regenerate
- **"Edit this one"**: start a new run with `NB_IN` set to the earlier output's absolute path and `NB_CMD="/edit input.png 'adjustment'"`
- **"Different style"**: Add `--styles="requested_style"` to the command

## Prompt Tips

1. **Be specific**: Include style, mood, colors, composition details
2. **Add "no text"**: If you don't want text rendered in the image
3. **Reference styles**: "editorial photography", "flat illustration", "3D render", "watercolor"
4. **Specify aspect ratio context**: "wide banner", "square thumbnail", "vertical story"

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `GEMINI_API_KEY` not set | Never paste a key. It comes from `~/.claude/secrets/gemini-api-key` via `~/.zshrc`; if it is missing, tell LISA (`lisa-secrets`) |
| Extension not found, or pre-flight FAIL | Stop and tell LISA. Never install from a URL or with the Gemini CLI installer; the re-install is a plain copy of the verified bytes (`CS.AK.LISA.Intel.NanoBananaTest0Fixes` §R2.3) |
| `YOLO mode is disabled …` (exit 52) | Someone added `--yolo` or `--approval-mode yolo`. Remove it; the lane never uses yolo |
| `not running in a trusted directory` (exit 55) | The block's `--skip-trust` is missing. Use the run block unchanged |
| Quota exceeded | Wait for reset or switch to flash model |
| Image generation failed | Check prompt for policy violations, simplify request |
| Output directory missing | Will be created automatically on first run |