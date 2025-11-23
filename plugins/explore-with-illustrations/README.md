# Gemini Imagen - Claude Code Plugin

Generate and edit high-quality images using Google's Gemini API directly from Claude Code. Supports text-to-image, image editing, multi-turn refinement, and composition from multiple reference images.

Extracted from https://github.com/EveryInc/every-marketplace/tree/d44804fc39d60bb7914d971cc90bdb98e3b3e710/plugins/compounding-engineering/skills/gemini-imagegen

## Features

- **Image Generation**: Create professional-quality images from text prompts
- **Image Editing**: Modify existing images with natural language instructions
- **Multi-turn Refinement**: Iteratively improve images through conversation
- **Architecture Analysis**: `/explore` command for visualizing codebase architecture
- **Google Search Grounding**: Generate images based on real-time data
- **High Resolution**: Up to 4K output with various aspect ratios

## Installation


```bash
npx claude-plugins install @agneym/agney-cm/explore-with-illustrations
```

## Requirements

- **Python 3.11+**: Required for running the image generation scripts
- uv: Python dependency management
- **GEMINI_API_KEY**: Environment variable with your Google Gemini API key
- **Dependencies**: `google-genai`, `pillow` (automatically managed via uv scripts)

## Quick Start

### Slash Command: `/explore`

Analyze and visualize a codebase or library:

```bash
# Analyze a specific directory
/explore src/api

# Explore entire codebase
/explore .

# Analyze a feature area
/explore components/authentication
```

The command will:
1. Explore the codebase to understand components and relationships
2. Explain the architecture concisely
3. Generate technical diagrams with explicit labels and data flows

### Using the Skill Directly

The gemini-imagen skill is also available for general image generation:

```bash
# Generate any image
"Create a logo for my company with a mountain theme"

# Edit an existing image
"Add a sunset background to image.png"

# Technical illustrations
"Generate a flowchart showing the OAuth 2.0 authentication flow"
```

## Documentation

- **[SKILL.md](skills/gemini-imagen/SKILL.md)** - Complete image generation skill documentation with API patterns, prompting best practices, and advanced features
- **[explore.md](commands/explore.md)** - Architecture analysis slash command with technical diagram templates and visualization guidelines

## Python Scripts

The plugin includes command-line tools for direct use:

- `scripts/generate_image.py` - Generate images from text prompts
- `scripts/edit_image.py` - Edit existing images
- `scripts/multi_turn_chat.py` - Interactive multi-turn refinement
- `scripts/compose_images.py` - Compose multiple reference images
- `scripts/gemini_images.py` - Python library for programmatic access

## License

MIT License - see [LICENSE](./LICENSE) for details.
