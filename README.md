# Slidev Skill for Claude Code

A Claude Code skill that generates beautiful presentations using [Slidev](https://sli.dev) - the presentation tool for developers.

## What This Does

When you ask Claude to create a presentation, this skill enables it to:

- Generate properly structured Slidev projects (slides.md + package.json)
- Use native Slidev features: themes, layouts, animations
- Create Mermaid diagrams for architecture and flowcharts
- Apply UnoCSS/Tailwind styling
- Handle images (local, Unsplash, Iconify icons)
- Avoid common pitfalls (overflow, missing dependencies)

## Installation

Copy `SKILL.md` to your Claude Code skills directory:

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

- **Themes**: Professional styling out of the box (seriph, default, apple-basic)
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
| Themes | seriph, default, apple-basic, bricks |
| Layouts | two-cols, image-right, center, image |
| Animations | v-click, v-clicks, transitions |
| Diagrams | Mermaid flowcharts, sequence diagrams |
| Code | Line highlighting, magic-move |
| Images | Local, Unsplash, Iconify icons |
| Styling | UnoCSS/Tailwind classes |

## Why Slidev?

Compared to other approaches:

| Tool | Pros | Cons |
|------|------|------|
| **Slidev** | Native Mermaid, animations, themes, dev-friendly | Requires npm |
| Marp | Simple, VS Code plugin | Limited layouts, no animations |
| html2pptx | Native PowerPoint shapes | Complex, validation issues |
| Google Slides API | Real slides | Complex auth, limited styling |

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
