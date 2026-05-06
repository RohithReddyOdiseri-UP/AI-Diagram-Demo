# Session Context — UP Draw.io Diagram Generator Skill

**Date:** May 1, 2026  
**Project:** `draw-io-diagram-generator - Copy`  
**Goal:** Extend the existing draw-io-diagram-generator skill to support Union Pacific (UP) architecture diagrams using the UP Level 1 shape library and template standards.

---

## What Was Accomplished

### 1. Reference Analyzed

The following source files were fully analyzed:

| File | Purpose |
|------|---------|
| `up-references/UP Level 1 Library.xml` | UP-specific shape library — 50+ shapes for Level 0/0.5/1 diagrams |
| `assets/up-templates/Level 1 Diagram Template.drawio` | 3-page draw.io template (Master Notes Page, FG.01 diagram, Background) |
| `references/shape-libraries.md` | Existing generic shape library reference (used as structural model) |
| `references/style-reference.md` | Existing generic style reference (used as structural model) |
| `README.md` | Overview of Level 0, Level 0.5, Level 1, and Level 2 diagram types |

### 2. Files Created

| File | Description |
|------|-------------|
| `up-references/UP-level-1-shape-library.md` | UP shape catalog — all 50+ shapes organized by category with style notes |
| `up-references/UP-level-1-style-reference.md` | UP style reference — color palette, exact style strings, template structure, usage conventions |

---

## UP Diagram Architecture Overview

### Diagram Levels

| Level | Description | Template |
|-------|-------------|---------|
| Level 0 | External-facing view, outward-facing systems | Level 1 Diagram Template.drawio |
| Level 0.5 | First inward look — module/logical component interactions | Level 1 Diagram Template.drawio |
| Level 1 | Deployment Unit (DU) interactions | Level 1 Diagram Template.drawio |
| Level 2 | Business process flows | Level 2 Diagram Template.drawio |

### Key UP Shape Categories

1. **Containers** — App/Domain (swimlane), UI (group)
2. **Sequence Indicators** — Mandatory Sequence (gold circle), Optional Sequence (gold diamond)
3. **Deployment Units** — 5 variants: Needs Decomposition / New / Rework / Existing / External
4. **Logical Components** — Same 5 variants as DU but with dashed borders and `container=1`
5. **Actors** — Business Partner, External User (person icons)
6. **Services (edges)** — Enterprise / Endorsement / Exclusive × Sync Service / Event / Async Post
7. **Infrastructure** — Database, Local Cache, Distributed Cache, Cloud, Timer, Loop
8. **Callouts** — Generic Callout, NFR Callout, Cache Details Callout
9. **Title** — Title Block, Title Text

### Status Color Convention

| Status | Fill (shapes) | Label BG (edges) |
|--------|--------------|-----------------|
| Needs Decomposition | `#ffff00` (Yellow) | — |
| New | `#c8d6e6` (Light Blue) | `#C8D6E6` |
| Rework | `#CADAA9` (Light Green) | `#CADAA9` |
| Existing | Default white | — |
| External | `#FFCB7D` (Orange) | — |

### Service Scope Convention

| Scope | Stroke Color | Arrow Style |
|-------|-------------|------------|
| Enterprise | Black (default) | `endArrow=classic` (sync), `endArrow=dash` (async) |
| Endorsement | `#0000FF` Blue | `endArrow=block;endFill=0` (sync), `endArrow=dash` (async) |
| Exclusive | *(similar to Enterprise)* | *(same patterns)* |

### Edge Differentiators

| Type | Line Style |
|------|-----------|
| Synchronous Service | Solid line, `endArrow=classic` |
| Event (async message) | Dashed `dashPattern=8 8`, `endArrow=classic` |
| Async / Post Service | Solid line, `endArrow=dash;endFill=0` |
| Special Message | Thick dashed (`strokeWidth=2;dashPattern=1 2`) |

---

## Template Structure

**File:** `assets/up-templates/Level 1 Diagram Template.drawio`

| Page Name | Page ID | Purpose |
|-----------|---------|---------|
| Master Notes Page | `qdjfoi8wspzoLScqgMXj` | General info, assumptions, scenario summary |
| FG.01 | `q55GrXyY6krVHwyM7t16` | Working diagram page |
| Background | `K9_SMR9-HG65Afowcf0h` | Shared template chrome; referenced by other pages |

Other pages reference Background via:
```xml
backgroundImage="{&quot;src&quot;:&quot;data:page/id,K9_SMR9-HG65Afowcf0h&quot;}"
```

**Title format:**
```
{FG}-{####} - {FG Name}
{Scenario #} - {Scenario Name}
```
Version number displayed separately (e.g., `1.0`).

---

## Placeholder / Custom Property Conventions

Services use draw.io's **placeholder** system:

```xml
<object placeholders="1" name="service-name/1.0" label="%name%">
  <mxCell style="..." edge="1" .../>
</object>
```

Events additionally carry a `metrics` property:

```xml
<object label="%name%&lt;div&gt;%metrics%&lt;/div&gt;" placeholders="1"
        metrics="[Daily Vol k/day]{Volume/sec}(Max Resp (ms))"
        name="event-name/1.0">
```

---

## Files Still in Scope / Suggested Next Steps

- [ ] Update `SKILL.md` to add a **UP-specific section** covering Level 0/0.5/1 diagram generation
- [ ] Create `up-references/UP-level-2-shape-library.md` and `UP-level-2-style-reference.md` from `up-references/UP Level 2 Library.xml` and `assets/up-templates/Level 2 Diagram Template.drawio`
- [ ] Add UP-specific example XML recipes (e.g., sample Level 1 diagram with DUs, services, callouts) to the skill
- [ ] Validate that `UP Level 2 Library.xml` exists and analyze its shapes

---

## Key File Paths

```
draw-io-diagram-generator - Copy/
├── SKILL.md                                  # Main skill instructions
├── README.md                                 # Project overview
├── assets/
│   └── up-templates/
│       ├── Level 1 Diagram Template.drawio   # Source template for Level 0/0.5/1
│       └── Level 2 Diagram Template.drawio   # Source template for Level 2
├── references/
│   ├── shape-libraries.md                    # Generic draw.io shape library reference
│   ├── style-reference.md                    # Generic draw.io style reference
│   └── drawio-xml-schema.md                  # mxGraph XML schema reference
└── up-references/
    ├── UP Level 1 Library.xml                # UP Level 1 shape library (source)
    ├── UP Level 2 Library.xml                # UP Level 2 shape library (source)
    ├── UP-level-1-shape-library.md           # ✅ Created — UP L1 shape catalog
    └── UP-level-1-style-reference.md         # ✅ Created — UP L1 style reference
```
