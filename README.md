# Slidev Skill for Claude Code

A Claude Code skill that generates presentations using [Slidev](https://sli.dev) - the presentation tool for developers.

## What This Does

When you ask Claude to create a presentation, this skill enables it to:

- Generate properly structured Slidev projects (slides.md + package.json)
- Use native Slidev features: themes, layouts, animations
- Create Mermaid diagrams sized correctly for slides (avoiding overflow!)
- Apply UnoCSS/Tailwind styling
- Handle images (local, Unsplash, AI-generated if MCP available)
- Use proper layouts for background images (`layout: cover`)
- Avoid common pitfalls (diagram overflow, missing dependencies)

## Installation

Copy `SKILL.md` to your Claude Code skills directory (you can also put it in a specific project):

```bash
# macOS/Linux
cp SKILL.md ~/.claude/skills/slidev/SKILL.md

# Or create the directory first
mkdir -p ~/.claude/skills/slidev
cp SKILL.md ~/.claude/skills/slidev/
```

## Usage

Just ask Claude to create a presentation:

```
"Create a 10-slide presentation about [topic]"
"Make a pitch deck for [product]"
"Generate slides explaining [technical concept]"
```

Claude will create:
1. `package.json` - Dependencies and scripts
2. `slides.md` - The presentation content

Then run:
```bash
npm install && npm run dev
```

## Example Output

The skill generates presentations with:

- **Themes**: Professional styling out of the box (dracula default, seriph, apple-basic)
- **Layouts**: two-cols, image-right, center
- **Animations**: v-click for progressive reveal
- **Diagrams**: Mermaid flowcharts and sequence diagrams
- **Code blocks**: Syntax highlighting with line focus

## Example Prompt

```
Create a presentation about our new API:
- Title slide
- Problem we're solving
- Architecture diagram
- Code examples
- Demo screenshots (I have them in ./images/)
- Pricing
- Call to action
```

## Features Documented in the Skill

| Feature | Description |
|---------|-------------|
| Themes | dracula (default), seriph, default, apple-basic, bricks |
| Layouts | two-cols, image-right, center, cover, image |
| Animations | v-click, v-clicks, transitions |
| Diagrams | Mermaid with sizing/overflow prevention |
| Code | Line highlighting, syntax coloring |
| Images | Local, Unsplash, Iconify, AI-generated (optional) |
| Styling | UnoCSS/Tailwind classes |

## Why Slidev?

Compared to other approaches:

| Tool | Pros | Cons |
|------|------|------|
| **Slidev** | Native Mermaid, animations, themes, dev-friendly | Requires npm |
| Marp | Simple, VS Code plugin | Limited layouts, no animations |
| html2pptx | Native PowerPoint shapes | Complex, validation issues |
| Google Slides API | Real slides | Complex auth, limited styling |

## Key Learnings Built Into This Skill

This skill was refined through real-world usage. Key lessons learned:

- **Mermaid diagrams overflow easily** - Use TB (vertical) in two-cols layouts, LR (horizontal) for full-width slides
- **Subgraphs are wide** - Never use subgraphs in two-cols layouts
- **Background images need `layout: cover`** - Other layouts may ignore the `background:` property
- **Always create package.json** - Slidev won't run without it
- **Scale diagrams** - Use `{scale: 0.5}` to `{scale: 0.7}` for complex diagrams

## Contributing

Found an issue or improvement? PRs welcome!

Common things to add:
- New layout patterns
- Mermaid diagram examples
- Theme recommendations
- Overflow prevention tips

## License

MIT

## Credits

Created through human-AI collaboration, iteratively refined by fixing real issues encountered during presentation generation.
