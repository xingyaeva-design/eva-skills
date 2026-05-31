---
name: ckm:design
description: "Eva Lan 设计执行层，默认套用 eva-design-system（月白底+烟绿+衬线体）。做 logo、banner、社交图片、PPT、CIP、icon 时触发。所有输出自动继承 Eva 品牌 DNA，除非用户明确指定其他风格。Actions: design logo, create CIP, generate mockups, build slides, design banner, generate icon, create social photos, social media images, brand identity. Platforms: WeChat, Xiaohongshu, LinkedIn, Twitter/X, YouTube."
argument-hint: "[design-type] [context]"
license: MIT
metadata:
  author: eva
  version: "3.0.0"
---

# Design（Eva 版）

Eva Lan 设计执行层。**所有设计任务开始前，先读 `../eva-design-system/brand-dna.md`**，获取品牌 DNA 作为默认约束。

## 默认品牌上下文

| 属性 | 默认值 |
|------|--------|
| 底色 | 月白 `#F5F4F0` |
| 主色 | 烟绿 `#8A9E92` |
| 点缀色 | 暖金 `#C4A882` |
| 正文色 | 深墨 `#2E2820` |
| 英文字体 | Cormorant Garamond |
| 中文字体 | Noto Serif SC |
| 深色背景 | `#2C2C28`（仅打破节奏用，≤30%） |

如用户提供其他品牌色/风格，以用户输入为准，忽略上述默认值。

## When to Use

- Logo 设计与 AI 生成
- Corporate Identity Program（CIP）全套物料
- 演示文稿 / Pitch Deck
- Banner：公众号封面、社交媒体头图、广告图
- 社交图片：小红书封面、X 配图、LinkedIn 图
- SVG icon 设计

## Sub-skill Routing

| Task | Sub-skill | Details |
|------|-----------|---------|
| Brand identity, voice, assets | `brand` | External skill |
| Tokens, specs, CSS vars | `design-system` | External skill |
| shadcn/ui, Tailwind, code | `ui-styling` | External skill |
| Logo creation, AI generation | Logo (built-in) | `references/logo-design.md` |
| CIP mockups, deliverables | CIP (built-in) | `references/cip-design.md` |
| Presentations, pitch decks | Slides (built-in) | `references/slides.md` |
| Banners, covers, headers | Banner (built-in) | `references/banner-sizes-and-styles.md` |
| Social media images/photos | Social Photos (built-in) | `references/social-photos-design.md` |
| SVG icons, icon sets | Icon (built-in) | `references/icon-design.md` |

## Logo Design (Built-in)

55+ styles, 30 color palettes, 25 industry guides. Gemini Nano Banana models.

### Logo: Generate Design Brief

```bash
python3 ~/.claude/skills/design/scripts/logo/search.py "tech startup modern" --design-brief -p "BrandName"
```

### Logo: Search Styles/Colors/Industries

```bash
python3 ~/.claude/skills/design/scripts/logo/search.py "minimalist clean" --domain style
python3 ~/.claude/skills/design/scripts/logo/search.py "tech professional" --domain color
python3 ~/.claude/skills/design/scripts/logo/search.py "healthcare medical" --domain industry
```

### Logo: Generate with AI

**ALWAYS** generate output logo images with white background.

```bash
python3 ~/.claude/skills/design/scripts/logo/generate.py --brand "TechFlow" --style minimalist --industry tech
python3 ~/.claude/skills/design/scripts/logo/generate.py --prompt "coffee shop vintage badge" --style vintage
```

**IMPORTANT:** When scripts fail, try to fix them directly.

After generation, **ALWAYS** ask user about HTML preview via `AskUserQuestion`. If yes, invoke `/ui-ux-pro-max` for gallery.

## CIP Design (Built-in)

50+ deliverables, 20 styles, 20 industries. Gemini Nano Banana (Flash/Pro).

### CIP: Generate Brief

```bash
python3 ~/.claude/skills/design/scripts/cip/search.py "tech startup" --cip-brief -b "BrandName"
```

### CIP: Search Domains

```bash
python3 ~/.claude/skills/design/scripts/cip/search.py "business card letterhead" --domain deliverable
python3 ~/.claude/skills/design/scripts/cip/search.py "luxury premium elegant" --domain style
python3 ~/.claude/skills/design/scripts/cip/search.py "hospitality hotel" --domain industry
python3 ~/.claude/skills/design/scripts/cip/search.py "office reception" --domain mockup
```

### CIP: Generate Mockups

```bash
# With logo (RECOMMENDED)
python3 ~/.claude/skills/design/scripts/cip/generate.py --brand "TopGroup" --logo /path/to/logo.png --deliverable "business card" --industry "consulting"

# Full CIP set
python3 ~/.claude/skills/design/scripts/cip/generate.py --brand "TopGroup" --logo /path/to/logo.png --industry "consulting" --set

# Pro model (4K text)
python3 ~/.claude/skills/design/scripts/cip/generate.py --brand "TopGroup" --logo logo.png --deliverable "business card" --model pro

# Without logo
python3 ~/.claude/skills/design/scripts/cip/generate.py --brand "TechFlow" --deliverable "business card" --no-logo-prompt
```

Models: `flash` (default, `gemini-2.5-flash-image`), `pro` (`gemini-3-pro-image-preview`)

### CIP: Render HTML Presentation

```bash
python3 ~/.claude/skills/design/scripts/cip/render-html.py --brand "TopGroup" --industry "consulting" --images /path/to/cip-output
```

**Tip:** If no logo exists, use Logo Design section above first.

## Slides (Built-in)

Strategic HTML presentations with Chart.js, design tokens, copywriting formulas.

Load `references/slides-create.md` for the creation workflow.

### Slides: Knowledge Base

| Topic | File |
|-------|------|
| Creation Guide | `references/slides-create.md` |
| Layout Patterns | `references/slides-layout-patterns.md` |
| HTML Template | `references/slides-html-template.md` |
| Copywriting | `references/slides-copywriting-formulas.md` |
| Strategies | `references/slides-strategies.md` |

## Banner Design (Built-in)

22 art direction styles across social, ads, web, print. Uses `frontend-design`, `ai-artist`, `ai-multimodal`, `chrome-devtools` skills.

Load `references/banner-sizes-and-styles.md` for complete sizes and styles reference.

### Banner: Workflow

1. **Gather requirements** via `AskUserQuestion` — purpose, platform, content, brand, style, quantity
2. **Research** — Activate `ui-ux-pro-max`, browse Pinterest for references
3. **Design** — Create HTML/CSS banner with `frontend-design`, generate visuals with `ai-artist`/`ai-multimodal`
4. **Export** — Screenshot to PNG at exact dimensions via `chrome-devtools`
5. **Present** — Show all options side-by-side, iterate on feedback

### Banner: Eva 常用尺寸

| 平台 | 类型 | 尺寸 (px) |
|------|------|-----------|
| 微信公众号 | 封面首图 | 900 x 383 |
| 微信公众号 | 次图/方图 | 500 x 500 |
| 小红书 | 封面竖图 | 1080 x 1350 |
| 小红书 | 图文卡片 | 1080 x 1440（3:4） |
| Twitter/X | 配图 | 1200 x 675 |
| Twitter/X | 头图 | 1500 x 500 |
| LinkedIn | 配图 | 1200 x 627 |
| YouTube | 封面 | 1280 x 720 |
| 网站 | Hero | 1920 x 600–1080 |

### Banner: Eva 风格优先级

| 风格 | 适用场景 | 说明 |
|------|----------|------|
| 衬线大字留白 | 公众号封面、观点型内容 | Eva 首选，气质最匹配 |
| 深色底+暖金 | 小红书 C 端、有力量感内容 | `#2C2C28` 底，暖金点缀 |
| 月白极简 | 方法论、教程、工具类 | 烟绿 accent，零装饰 |
| 信息密度型 | 数据、对比、清单类 | 结构清晰优先，装饰最少 |

**⚠️ Eva 品牌禁用**：glassmorphism、渐变文字、neon、蓝紫渐变、AI 光效

### Banner: Design Rules

- Safe zones: critical content in central 70-80%
- One CTA per banner, bottom-right, min 44px height
- Max 2 fonts, min 16px body, ≥32px headline
- Text under 20% for ads (Meta penalizes)
- Print: 300 DPI, CMYK, 3-5mm bleed

## Icon Design (Built-in)

15 styles, 12 categories. Gemini 3.1 Pro Preview generates SVG text output.

### Icon: Generate Single Icon

```bash
python3 ~/.claude/skills/design/scripts/icon/generate.py --prompt "settings gear" --style outlined
python3 ~/.claude/skills/design/scripts/icon/generate.py --prompt "shopping cart" --style filled --color "#6366F1"
python3 ~/.claude/skills/design/scripts/icon/generate.py --name "dashboard" --category navigation --style duotone
```

### Icon: Generate Batch Variations

```bash
python3 ~/.claude/skills/design/scripts/icon/generate.py --prompt "cloud upload" --batch 4 --output-dir ./icons
```

### Icon: Multi-size Export

```bash
python3 ~/.claude/skills/design/scripts/icon/generate.py --prompt "user profile" --sizes "16,24,32,48" --output-dir ./icons
```

### Icon: Top Styles

| Style | Best For |
|-------|----------|
| outlined | UI interfaces, web apps |
| filled | Mobile apps, nav bars |
| duotone | Marketing, landing pages |
| rounded | Friendly apps, health |
| sharp | Tech, fintech, enterprise |
| flat | Material design, Google-style |
| gradient | Modern brands, SaaS |

**Model:** `gemini-3.1-pro-preview` — text-only output (SVG is XML text). No image generation API needed.

## Social Photos (Built-in)

Multi-platform social image design: HTML/CSS → screenshot export. Uses `ui-ux-pro-max`, `brand`, `design-system`, `chrome-devtools` skills.

Load `references/social-photos-design.md` for sizes, templates, best practices.

### Social Photos: Workflow

1. **Orchestrate** — `project-management` skill for TODO tasks; parallel subagents for independent work
2. **Analyze** — Parse prompt: subject, platforms, style, brand context, content elements
3. **Ideate** — 3-5 concepts, present via `AskUserQuestion`
4. **Design** — `/ckm:brand` → `/ckm:design-system` → randomly invoke `/ck:ui-ux-pro-max` OR `/ck:frontend-design`; HTML per idea × size
5. **Export** — `chrome-devtools` or Playwright screenshot at exact px (2x deviceScaleFactor)
6. **Verify** — Use Chrome MCP or `chrome-devtools` skill to visually inspect exported designs; fix layout/styling issues and re-export
7. **Report** — Summary to `plans/reports/` with design decisions
8. **Organize** — Invoke `assets-organizing` skill to sort output files and reports

### Social Photos: Eva 常用尺寸

| 平台 | 尺寸 (px) | 账号 |
|------|-----------|------|
| 小红书封面 | 1080×1350 | Eva·克劳得 |
| 小红书图文卡片 | 1080×1440 | Eva·克劳得 |
| 公众号首图 | 900×383 | Agent架构局 |
| X/Twitter 配图 | 1200×675 | Eva Lan |
| LinkedIn 配图 | 1200×627 | Eva Lan |
| YouTube 封面 | 1280×720 | — |

## Workflows

### Complete Brand Package

1. **Logo** → `scripts/logo/generate.py` → Generate logo variants
2. **CIP** → `scripts/cip/generate.py --logo ...` → Create deliverable mockups
3. **Presentation** → Load `references/slides-create.md` → Build pitch deck

### New Design System

1. **Brand** (brand skill) → Define colors, typography, voice
2. **Tokens** (design-system skill) → Create semantic token layers
3. **Implement** (ui-styling skill) → Configure Tailwind, shadcn/ui

## References

| Topic | File |
|-------|------|
| Design Routing | `references/design-routing.md` |
| Logo Design Guide | `references/logo-design.md` |
| Logo Styles | `references/logo-style-guide.md` |
| Logo Colors | `references/logo-color-psychology.md` |
| Logo Prompts | `references/logo-prompt-engineering.md` |
| CIP Design Guide | `references/cip-design.md` |
| CIP Deliverables | `references/cip-deliverable-guide.md` |
| CIP Styles | `references/cip-style-guide.md` |
| CIP Prompts | `references/cip-prompt-engineering.md` |
| Slides Create | `references/slides-create.md` |
| Slides Layouts | `references/slides-layout-patterns.md` |
| Slides Template | `references/slides-html-template.md` |
| Slides Copy | `references/slides-copywriting-formulas.md` |
| Slides Strategy | `references/slides-strategies.md` |
| Banner Sizes & Styles | `references/banner-sizes-and-styles.md` |
| Social Photos Guide | `references/social-photos-design.md` |
| Icon Design Guide | `references/icon-design.md` |

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/logo/search.py` | Search logo styles, colors, industries |
| `scripts/logo/generate.py` | Generate logos with Gemini AI |
| `scripts/logo/core.py` | BM25 search engine for logo data |
| `scripts/cip/search.py` | Search CIP deliverables, styles, industries |
| `scripts/cip/generate.py` | Generate CIP mockups with Gemini |
| `scripts/cip/render-html.py` | Render HTML presentation from CIP mockups |
| `scripts/cip/core.py` | BM25 search engine for CIP data |
| `scripts/icon/generate.py` | Generate SVG icons with Gemini 3.1 Pro |

## Setup

```bash
export GEMINI_API_KEY="your-key"  # https://aistudio.google.com/apikey
pip install google-genai pillow
```

## Integration

**External sub-skills:** brand, design-system, ui-styling
**Related Skills:** frontend-design, ui-ux-pro-max, ai-multimodal, chrome-devtools
