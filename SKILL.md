---
name: slidev
description: Generate professional presentations using Markdown with Slidev. This skill should be used when users request a presentation, slide deck, or pitch deck—especially for developer-focused content with code examples, Mermaid diagrams, and animations. Always creates both slides.md and package.json.
---

# Slidev Presentation Skill

This skill generates professional presentations using [Slidev](https://sli.dev), a markdown-based presentation tool for developers.

## When to Use

This skill should be triggered when:
- User requests a "presentation", "slides", "slide deck", or "pitch deck"
- Content includes code examples, architecture diagrams, or technical content
- User wants animations, progressive reveal, or interactive elements

## CRITICAL: Always Create Both Files

**Every Slidev presentation requires TWO files. Always create both:**

1. `slides.md` - The presentation content
2. `package.json` - Dependencies (without this, `npm run dev` fails!)

Never create just slides.md alone. The presentation won't run.

## Project Structure

Every Slidev presentation needs:

```
presentation/
├── slides.md        # Main presentation file (required)
├── package.json     # Dependencies (required)
├── components/      # Custom Vue components (optional)
└── public/          # Static assets like images (optional)
```

## Step 1: Create package.json (REQUIRED)

**Always create this file first. Without it, nothing works.**

```json
{
  "name": "presentation-name",
  "private": true,
  "scripts": {
    "dev": "slidev",
    "build": "slidev build",
    "export": "slidev export",
    "export-pptx": "slidev export --format pptx"
  },
  "dependencies": {
    "@slidev/cli": "^0.50.0",
    "slidev-theme-dracula": "latest"
  }
}
```

### Theme Selection

**Default theme: `dracula`** - Dark with purple/cyan/green accents, great for tech content.

Before creating the presentation, ask the user if they want a different theme. Offer these options:

**Official Themes** (package: `@slidev/theme-{name}`):
| Theme | Style | Best For |
|-------|-------|----------|
| `default` | Clean, minimal | General purpose |
| `seriph` | Elegant serif typography | Business, professional |
| `apple-basic` | Apple Keynote inspired | Product launches (white bg only) |
| `bricks` | Colorful, playful blocks | Creative presentations |
| `shibainu` | Modern, distinctive | Tech talks |

**Popular Community Themes** (package: `slidev-theme-{name}`):
| Theme | Style | Best For |
|-------|-------|----------|
| `dracula` | Dark purple/cyan/green | Developer content, tech (DEFAULT) |
| `academic` | Academic/university style | Research, education |
| `purplin` | Purple professional | Corporate tech |

To switch themes:
1. Change package.json dependency to the theme package
2. Change `theme:` in slides.md frontmatter
3. Run `npm install` again

### AI Image Generation (Optional)

**Before creating the presentation, ask the user:**
1. Which theme? (default: dracula)
2. Do you want custom images for title/closing slides?

**If user wants custom images:**

Check if the `nano-banana` MCP is available. If available, use it to generate images:

| Slide Type | Generate Image? | Why |
|------------|-----------------|-----|
| Title/Hero | **Yes** | Sets the visual tone, unique to topic |
| Section dividers | **Optional** | Visual breaks between major sections |
| Content slides | **No** | Text-heavy, don't need images |
| Closing slide | **Yes** | Memorable ending, call-to-action |

**Workflow with nano-banana MCP:**
1. Create `public/` folder first
2. Generate images with `generate_image_to_file`:
   - Use `aspect_ratio: "16:9"` for slides
   - Use `style_preset: "tech"` for tech content (or "corporate", "minimal")
   - Save to `public/hero.png`, `public/closing.png`, etc.
3. Reference in slides.md with `background: /hero.png`
4. **IMPORTANT:** Use `layout: cover` for background images to display properly

**Example prompts for image generation:**
- Title: "Abstract visualization of [topic], dark purple and cyan, tech aesthetic"
- Closing: "Glowing path toward bright horizon, hopeful mood, blue and gold tones"

**Fallback if nano-banana MCP is not available:**
- Use `background: https://cover.sli.dev` for abstract gradient backgrounds
- Use Unsplash URLs: `background: https://images.unsplash.com/photo-xxx?w=1920`
- Use solid color backgrounds with the theme's accent colors
- Skip background images entirely - Slidev looks good without them

## Step 2: Create slides.md

### Frontmatter (First Slide)

```yaml
---
theme: dracula
background: https://cover.sli.dev
title: Presentation Title
class: text-center
transition: slide-left
mdc: true
---
```

### Slide Separators

Use `---` to separate slides. Each slide can have its own frontmatter:

```markdown
---
transition: fade-out
layout: two-cols
---
```

## Layouts

### `default` - Standard slide
```markdown
---
---

# Title

Content here
```

### `two-cols` - Two columns
```markdown
---
layout: two-cols
layoutClass: gap-8
---

# Left Column Title

Left content

::right::

Right content
```

### `image-right` - Image on right
```markdown
---
layout: image-right
image: https://example.com/image.png
---

# Title

Content on left, image on right
```

### `center` - Centered content
```markdown
---
layout: center
class: text-center
---

# Centered Title

Centered content
```

### `cover` - Background image with content overlay
```markdown
---
layout: cover
background: /hero.png
class: text-center
---

# Title over background

Content here - background image visible behind
```

**IMPORTANT:** Use `layout: cover` when you want a `background:` image to display. Other layouts like `center` or `default` may ignore or override the background property depending on the theme.

### `image` - Full background image (no content)
```markdown
---
layout: image
image: https://example.com/bg.png
---

# Title over image
```

## Animations

### Progressive Reveal with v-clicks

```markdown
<v-clicks>

- First item (appears on click 1)
- Second item (appears on click 2)
- Third item (appears on click 3)

</v-clicks>
```

### Individual v-click

```markdown
<div v-click>This appears on first click</div>
<div v-click>This appears on second click</div>
```

### Click to Show/Hide

```markdown
<div v-click="[1, 3]">Visible from click 1 to 3</div>
```

## Code Blocks

### Basic with Syntax Highlighting

```markdown
\`\`\`python
def hello():
    print("Hello, world!")
\`\`\`
```

### Line Highlighting (Progressive)

```markdown
\`\`\`python {all|2|3-4|all}
@dataclass
class Memory:
    content: str
    confidence: float
\`\`\`
```

- `{all}` - highlight all
- `{2}` - highlight line 2
- `{3-4}` - highlight lines 3-4
- `{all|2|3-4|all}` - progressive: all → line 2 → lines 3-4 → all

### Line Numbers

```markdown
\`\`\`python {lines:true}
code here
\`\`\`
```

## Mermaid Diagrams

### CRITICAL: Slide Size Constraints

**Slidev slides are 16:9 aspect ratio (960×540 effective pixels).** Diagrams overflow easily!

**Golden Rules:**
1. **Use LR (left-right) for pipelines** - horizontal fits 16:9 better than vertical
2. **Max 4 nodes vertically** in TB flow before overflow
3. **Max 6-7 nodes horizontally** in LR flow before overflow
4. **Always use scale** - `{scale: 0.5}` to `{scale: 0.7}` for complex diagrams
5. **Subgraphs add height** - avoid stacking subgraphs vertically
6. **Short labels** - use abbreviations, 2-3 words max per node

### Direction Selection Guide

| Diagram Type | Direction | Why |
|--------------|-----------|-----|
| Full-width slide | **LR** | Horizontal flows use wide 16:9 space well |
| In `two-cols` layout | **TB** | Columns are ~50% width, LR overflows! |
| Hierarchy/tree | TB | Trees grow down naturally |
| Simple 3-4 step | Either | Both fit fine |
| Complex with subgraphs | **LR** | Subgraphs stack horizontally (full-width only) |

**CRITICAL: Layout affects direction choice!**
- Full-width slides (`default`, `center`): Use **LR** for pipelines
- Half-width (`two-cols`, `image-right`): Use **TB** - columns are narrow!
- **NEVER use subgraphs in two-cols** - they're too wide even with TB

### Scaling

**Always specify scale for diagrams with 4+ nodes:**

```markdown
\`\`\`mermaid {scale: 0.6, theme: 'dark'}
flowchart LR
    ...
\`\`\`
```

| Complexity | Scale |
|------------|-------|
| Simple (3-4 nodes) | `0.8` or omit |
| Medium (5-7 nodes) | `0.6` - `0.7` |
| Complex (8+ nodes) | `0.5` - `0.6` |
| With subgraphs | `0.5` - `0.6` |

### Flowchart - Horizontal Pipeline (Recommended)

```markdown
\`\`\`mermaid {scale: 0.6, theme: 'dark'}
flowchart LR
    A[Input] --> B[Process] --> C[Validate] --> D{OK?}
    D -->|Yes| E[Output]
    D -->|No| F[Error]

    style A fill:#4EC5D4,color:#000
    style E fill:#16A085,color:#fff
    style F fill:#E74C3C,color:#fff
\`\`\`
```

### Flowchart - With Subgraphs (Horizontal Layout)

**DO: Subgraphs side-by-side with LR**
```markdown
\`\`\`mermaid {scale: 0.55, theme: 'dark'}
flowchart LR
    subgraph In[Input]
    A[Email] --> B[Queue]
    end

    subgraph Proc[Processing]
    C[Extract] --> D[Match]
    end

    subgraph Out[Output]
    E[API] --> F[Done]
    end

    B --> C
    D --> E
\`\`\`
```

**DON'T: Subgraphs stacked vertically with TB** (overflows!)
```markdown
%% This will overflow the slide!
flowchart TB
    subgraph Input
    ...
    end
    subgraph Process  %% Adds more height
    ...
    end
    subgraph Output   %% Even more height - overflow!
    ...
    end
```

### Sequence Diagram

Sequence diagrams are naturally horizontal, but limit participants:

```markdown
\`\`\`mermaid {scale: 0.7}
sequenceDiagram
    User->>API: Request
    API->>DB: Query
    DB-->>API: Results
    API-->>User: Response
\`\`\`
```

**Max 4-5 participants** before horizontal overflow.

### When Diagrams Don't Fit

If a diagram is too complex for one slide:

1. **Simplify** - Remove detail, show high-level flow
2. **Split** - Overview slide + detail slide(s)
3. **Use two-cols** - Diagram on one side, explanation on other
4. **Smaller scale** - Go down to `{scale: 0.5}` if needed

### Node Label Tips

Keep labels short to save space:

| Instead of | Use |
|------------|-----|
| `Invoice Extractor Service` | `Extractor` |
| `Message Queue System` | `Queue` |
| `Business Central API` | `ERP API` |
| `Human Review Process` | `Review` |

## Styling with UnoCSS

Slidev includes UnoCSS (Tailwind-like). Common classes:

### Spacing
- `mt-4`, `mb-8`, `p-4`, `px-2`, `py-1` - margin/padding
- `space-y-4` - vertical spacing between children
- `gap-4`, `gap-8` - grid/flex gaps

### Layout
- `grid grid-cols-2` - 2-column grid
- `flex items-center justify-center` - flexbox centering
- `max-w-2xl mx-auto` - constrained width, centered

### Text
- `text-center`, `text-left`, `text-right` - alignment
- `text-sm`, `text-xl`, `text-4xl` - sizes
- `font-bold`, `font-light` - weight
- `text-gray-500`, `text-teal-500` - colors

### Backgrounds & Borders
- `bg-gray-50`, `bg-teal-500` - backgrounds
- `rounded`, `rounded-lg` - border radius
- `border-l-4 border-teal-500` - left border accent

### Example Card Pattern

```html
<div class="p-4 border-l-4 border-teal-500 bg-gray-50 rounded">
  <span class="text-teal-500 font-bold">1.</span> <b>Title</b><br>
  <span class="text-gray-500 text-sm">Description</span>
</div>
```

## Images

### Using User-Provided Images

If the user has images they want to use, they should specify them in their prompt:

- "I have these images: hero.png, diagram.png, screenshot.jpg"
- "Use images from `./assets/` folder"
- "I have product screenshots for each feature"

When user provides images:
1. Read the folder/files to see what's available
2. Copy or reference them from `public/` folder
3. Match images to relevant slides

If no images specified, use Unsplash URLs or cover.sli.dev for backgrounds.

### Local Images (Recommended for User-Provided)

Place images in `public/` folder:

```
presentation/
├── slides.md
├── package.json
└── public/
    ├── hero.jpg
    ├── diagram.png
    └── logo.svg
```

Reference with absolute path:
```markdown
---
layout: image-right
image: /robot.png
---
```

Or in content:
```html
<img src="/diagram.png" class="w-64" />
```

### Slidev Cover (Abstract Backgrounds)

Always works, generates nice gradients:
```markdown
---
background: https://cover.sli.dev
---
```

### Unsplash (External Images)

Direct URL (specific image):
```markdown
image: https://images.unsplash.com/photo-1485827404703-89b55fcc595e?w=800
```

Random by keyword (less predictable):
```markdown
image: https://source.unsplash.com/800x600/?robot,technology
```

### Iconify (SVG Icons)

Inline icons with customizable color:
```html
<img src="https://api.iconify.design/carbon:machine-learning.svg?color=%2316A085" class="w-32" />
```

Common icon sets:
- `carbon:` - IBM Carbon icons
- `mdi:` - Material Design Icons
- `logos:` - Brand logos
- `twemoji:` - Twitter emoji

Browse icons at: https://icon-sets.iconify.design/

### Image Sizing with UnoCSS

```html
<img src="/photo.jpg" class="w-64" />        <!-- fixed width -->
<img src="/photo.jpg" class="w-full" />      <!-- full width -->
<img src="/photo.jpg" class="h-48" />        <!-- fixed height -->
<img src="/photo.jpg" class="rounded-lg" />  <!-- rounded corners -->
<img src="/photo.jpg" class="opacity-50" />  <!-- semi-transparent -->
```

## Transitions

Set per-slide or globally:

```yaml
---
transition: slide-left
---
```

Options: `slide-left`, `slide-right`, `slide-up`, `slide-down`, `fade`, `fade-out`, `zoom`

## Scoped Styles

Add styles that only affect current slide:

```markdown
# My Slide

Content here

<style>
h1 {
  background: linear-gradient(45deg, #4EC5D4, #146b8c);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
```

## Speaker Notes

Add notes as HTML comments at end of slide:

```markdown
# Slide Title

Content

<!--
These are speaker notes.
- Visible in presenter mode (press 'p')
- Not shown in presentation
-->
```

## Complete Slide Example

```markdown
---
layout: two-cols
layoutClass: gap-8
transition: fade-out
---

# Feature Overview

<div class="space-y-4">

<div v-click class="p-3 border-l-4 border-teal-500 bg-gray-50 rounded">
  <span class="text-teal-500 font-bold">1.</span> <b>Fast</b><br>
  <span class="text-gray-500 text-sm">Instant hot reload</span>
</div>

<div v-click class="p-3 border-l-4 border-teal-500 bg-gray-50 rounded">
  <span class="text-teal-500 font-bold">2.</span> <b>Beautiful</b><br>
  <span class="text-gray-500 text-sm">Built-in themes</span>
</div>

</div>

::right::

\`\`\`mermaid {scale: 0.7}
flowchart TB
    A[Write Markdown] --> B[Preview Live]
    B --> C[Export PDF/PPTX]
\`\`\`

<!--
Talk about the developer experience here.
Mention hot reload and export options.
-->
```

## Running & Exporting

```bash
# Install dependencies
npm install

# Development with live preview
npm run dev
# Opens http://localhost:3030

# Export to PDF
npm run export

# Export to PPTX (image-based)
npm run export-pptx

# Export to PNG images
npx slidev export --format png
```

## Keyboard Shortcuts (Dev Mode)

| Key | Action |
|-----|--------|
| `Space` / `→` | Next slide/click |
| `←` | Previous |
| `o` | Slide overview |
| `p` | Presenter mode |
| `d` | Toggle dark mode |
| `g` | Go to slide |

## Tips for Great Presentations

1. **Use v-clicks** for progressive reveal - don't overwhelm with info
2. **Use Mermaid** for diagrams instead of ASCII art or images
3. **Use layouts** (`two-cols`, `image-right`) instead of custom CSS
4. **Use UnoCSS classes** for spacing/styling - avoid inline styles
5. **Keep code blocks short** - use line highlighting to focus attention
6. **Use themes** - don't reinvent styling from scratch
7. **Add speaker notes** - helps during presentation

## Validation Checklist

Before finalizing:
- [ ] **Created package.json** (presentation won't run without it!)
- [ ] **Created slides.md** with frontmatter
- [ ] Run `npm install && npm run dev` and check all slides render
- [ ] Click through all animations
- [ ] Check Mermaid diagrams render correctly
- [ ] Test in presenter mode (`p`)
- [ ] Export and verify PDF/PPTX output

## Quick Start Template

For every new presentation, create these two files:

**1. package.json**
```json
{
  "name": "my-presentation",
  "private": true,
  "scripts": {
    "dev": "slidev",
    "export": "slidev export"
  },
  "dependencies": {
    "@slidev/cli": "^0.50.0",
    "slidev-theme-dracula": "latest"
  }
}
```

**2. slides.md**
```markdown
---
theme: dracula
background: https://cover.sli.dev
title: My Presentation
class: text-center
transition: slide-left
---

# Title

Subtitle here

---

# Slide 2

Content
```

**3. Run it**
```bash
npm install && npm run dev
```
