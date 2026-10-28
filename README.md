# Nano Banana

A Claude Code skill for generating and editing images using the Gemini CLI's nanobanana extension.

## Features

- **Text-to-image generation** - Create images from natural language prompts
- **Image editing** - Modify existing images with instructions
- **Photo restoration** - Repair damaged or old photos
- **Icon generation** - Create app icons and favicons
- **Diagram creation** - Generate flowcharts and architecture diagrams
- **Pattern generation** - Create seamless textures and patterns
- **Story/sequential images** - Generate narrative image sequences

## Prerequisites

1. **Gemini CLI** - Install from [gemini-cli](https://github.com/google-gemini/gemini-cli)
2. **Gemini API Key** - Get one from [Google AI Studio](https://aistudio.google.com/)
3. **nanobanana extension** - Installed via Gemini CLI

## Installation

CS: do not follow the upstream install steps. This lane runs only through the run block in `SKILL.md`, at the pins in its frontmatter. See `CS.AK.LISA.Intel.NanoBananaTest0Fixes`.

## Usage

Once installed, Claude Code will automatically use this skill when you ask for image generation. Examples:

- "Generate a blog header image for a post about machine learning"
- "Create a YouTube thumbnail for a coding tutorial"
- "Make an app icon for a productivity tool"
- "Draw a flowchart showing user authentication"
- "Edit this image to remove the background"

## Available Commands

| Command | Description |
|---------|-------------|
| `/generate` | Text-to-image generation |
| `/edit` | Modify existing images |
| `/restore` | Repair damaged photos |
| `/icon` | Generate app icons |
| `/diagram` | Create flowcharts and diagrams |
| `/pattern` | Generate seamless patterns |
| `/story` | Create sequential/narrative images |
| `/nanobanana` | Natural language interface |

## Common Options

| Option | Description |
|--------|-------------|
| `--count=N` | Generate N variations (1-8) |
| `--styles="style1,style2"` | Apply artistic styles |
| `--seed=N` | Seed for reproducible results |

## Output

Generated images are saved to `./nanobanana-output/` in your current working directory.

## Model Selection

Default model: `gemini-2.5-flash-image` (~$0.04/image)

For higher quality output:

```bash
export NANOBANANA_MODEL=gemini-3-pro-image-preview
```

## License

MIT

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.
