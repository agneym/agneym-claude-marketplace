# Gemini Imagen - Claude Code Plugin

Generate and edit high-quality images using Google's Gemini API directly from Claude Code. Specialized for creating technical illustrations, architecture diagrams, code concept visualizations, and educational content.

Extracted from https://github.com/EveryInc/every-marketplace/tree/d44804fc39d60bb7914d971cc90bdb98e3b3e710/plugins/compounding-engineering/skills/gemini-imagegen

## Features

- **Technical Diagrams**: Optimized prompts for architecture diagrams, flowcharts, UML, database schemas, and more

## Installation


```bash
npx claude-plugins install @agneym/agney-cm/gemini-imagen
```

## Requirements

- **Python 3.11+**: Required for running the image generation scripts
- uv: Python dependency management
- **GEMINI_API_KEY**: Environment variable with your Google Gemini API key
- **Dependencies**: `google-genai`, `pillow` (automatically managed via uv scripts)

## Quick Start

Once installed, the gemini-imagen skill is available as an agent in Claude Code:

```bash
# Generate a technical diagram
Ask the gemini-imagen agent to create an architecture diagram showing microservices communication

# Edit an existing image
I want to understand how two factor authentication is setup. Use gemini-imagen for illustrations.

# Multi-turn refinement
Have a conversation with the gemini-imagen agent to iteratively refine your visualization
```

## Documentation

For detailed documentation including:
- Comprehensive prompting best practices
- Technical diagram templates
- API usage examples
- Advanced features and configurations

See [SKILL.md](./SKILL.md) for complete documentation.

## Python Scripts

The plugin includes command-line tools for direct use:

- `scripts/generate_image.py` - Generate images from text prompts
- `scripts/edit_image.py` - Edit existing images
- `scripts/multi_turn_chat.py` - Interactive multi-turn refinement
- `scripts/compose_images.py` - Compose multiple reference images
- `scripts/gemini_images.py` - Python library for programmatic access

## License

MIT License - see [LICENSE](./LICENSE) for details.
