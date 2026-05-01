# UP Level 1 Style Reference

Complete style reference for the Union Pacific Level 1 architecture diagrams. Styles are derived from the `UP Level 1 Library.xml` shape library and the `Level 1 Diagram Template.drawio` template.

---

## Style Format

draw.io styles are semicolon-delimited `key=value` pairs on the `style` attribute of `<mxCell>` elements:

```text
style="key1=value1;key2=value2;key3=value3;"
```

---

## UP Color Palette

### Component Status Colors

| Status | Fill Color | Hex | Usage |
|--------|-----------|-----|-------|
| Needs Decomposition | Yellow | `#ffff00` | Components requiring further breakdown |
| New | Light Blue | `#c8d6e6` | Newly introduced components |
| Rework | Light Green | `#CADAA9` | Existing components being modified |
| Existing | White (default) | — | Unchanged, pre-existing components |
| External | Orange | `#FFCB7D` | Third-party / external components |

### Service Label Background Colors

| Status | Color | Hex | Usage |
|--------|-------|-----|-------|
| Existing | None | — | No background highlight |
| Rework | Light Green | `#CADAA9` | Service being reworked |
| New | Light Blue | `#C8D6E6` | Newly introduced service |

### Container Colors

| Container | Fill Color | Hex | Usage |
|-----------|-----------|-----|-------|
| App/Domain | Light Cyan | `#E6FFFF` | Domain boundary / application grouping |
| UI | Peach | `#FDEFE3` | User interface container |

### Infrastructure Colors

| Element | Fill / Stroke | Hex | Usage |
|---------|--------------|-----|-------|
| Database | Fill: Orange | `#F8B57E` | Data store shape |
| Database | Stroke: Gray | `#666666` | Data store border |
| Callout | Fill: Yellow | `#FFFF99` | Annotation / note callout |
| Callout | Stroke: Gold | `#d6b656` | Annotation border |

### Scope Colors (Service/Event Stroke)

| Scope | Stroke Color | Hex | Usage |
|-------|-------------|-----|-------|
| Enterprise | Black (default) | — | Enterprise-wide services/events |
| Endorsement | Blue | `#0000FF` | Endorsement-scoped services/events |

### Sequence Indicator Colors

| Type | Fill Color | Hex |
|------|-----------|-----|
| Mandatory | Gold | `#FFC000` |
| Optional | Gold | `#FFC000` |

### Template Colors

| Element | Color | Hex | Usage |
|---------|-------|-----|-------|
| Title Text | Dark Red | `#C00000` | Header title and version |
| Section Header Text | Blue | `#0000ff` | Notes page section headers |
| Connector Line (Callout) | Gray | `#999999` | Callout leader lines |
| Borders (Notes) | Light Gray | `#E1E1E1` | Content area borders |

---

## Shape Style Strings

### App/Domain Container

```text
swimlane;whiteSpace=wrap;html=1;rounded=1;dashed=1;horizontal=1;align=right;spacingLeft=20;startSize=23;spacingRight=10;strokeColor=default;swimlaneLine=0;labelBackgroundColor=#E6FFFF;fillColor=#E6FFFF;swimlaneFillColor=#E6FFFF;fontColor=#808080;opacity=50;collapsible=0;fontStyle=0;verticalAlign=top;
```

**Key properties:**
- `swimlane` — container type
- `rounded=1;dashed=1` — rounded dashed border
- `fillColor=#E6FFFF` — light cyan
- `opacity=50` — semi-transparent
- `collapsible=0` — cannot collapse
- `fontColor=#808080` — gray label text
- `align=right` — label right-aligned

---

### UI Container

```text
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=12;fontColor=#808080;verticalAlign=top;align=right;fillColor=#FDEFE3;opacity=50;collapsible=0;container=1;dropTarget=1;
```

**Key properties:**
- `rounded=1` — rounded corners
- `fillColor=#FDEFE3` — peach
- `container=1;dropTarget=1` — accepts child elements
- `opacity=50` — semi-transparent

---

### Deployment Unit (by status)

**Base pattern:**
```text
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=<COLOR>;strokeWidth=0.5;
```

| Status | Fill Color Value |
|--------|----------------|
| Needs Decomposition | `#ffff00` |
| New | `#c8d6e6` |
| Rework | `#CADAA9` |
| Existing | *(omit fillColor or use default)* |
| External | `#FFCB7D` |

---

### Logical Component (by status)

**Base pattern:**
```text
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=<COLOR>;strokeWidth=0.5;fontColor=default;dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;
```

| Status | Fill Color Value |
|--------|----------------|
| Needs Composition | `#ffff00` |
| New | `#c8d6e6` |
| Rework | `#CADAA9` |
| Existing | *(omit fillColor or use default)* |
| External | `#FFCB7D` |

**Key differentiator from Deployment Units:** `dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;`

---

### Mandatory Sequence Indicator

```text
ellipse;whiteSpace=wrap;html=1;rounded=1;fontSize=12;fillColor=#FFC000;strokeColor=none;connectable=0;
```

---

### Optional Sequence Indicator

```text
rhombus;whiteSpace=wrap;html=1;rounded=0;fillColor=#FFC000;strokeColor=default;connectable=0;rotation=0;
```

---

### Timer (BPMN)

```text
points=[[0.145,0.145,0],[0.5,0,0],[0.855,0.145,0],[1,0.5,0],[0.855,0.855,0],[0.5,1,0],[0.145,0.855,0],[0,0.5,0]];shape=mxgraph.bpmn.event;html=1;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;verticalAlign=top;align=center;perimeter=ellipsePerimeter;outlineConnect=0;aspect=fixed;outline=standard;symbol=timer;fontFamily=Helvetica;fontSize=11;fontColor=default;
```

---

### Database

```text
strokeWidth=1;html=1;shape=mxgraph.flowchart.database;whiteSpace=wrap;strokeColor=#666666;fillColor=#F8B57E;
```

---

### Cloud

```text
ellipse;shape=cloud;whiteSpace=wrap;html=1;
```

---

## Edge (Connector) Style Strings

### Enterprise Service (Existing)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

### Enterprise Service (Rework)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;
```

### Enterprise Service (New)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;
```

---

### Enterprise Event (Existing)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;
```

### Enterprise Event (Rework)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;dashed=1;dashPattern=8 8;
```

### Enterprise Event (New)

```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;dashed=1;dashPattern=8 8;
```

---

### Enterprise Async / Post Service

```text
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

---

### Endorsement Service (Existing)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

### Endorsement Service (Rework)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

### Endorsement Service (New)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

---

### Endorsement Event (Existing)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

### Endorsement Event (Rework)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

### Endorsement Event (New)

```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;
```

---

### Endorsement Async / Post Service

```text
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;
```

---

### Special Message

```text
endArrow=classic;html=1;rounded=0;dashed=1;dashPattern=1 2;strokeWidth=2;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

---

### Callout Connector Line

```text
endArrow=none;html=1;rounded=0;labelBackgroundColor=default;strokeColor=#999999;fontFamily=Helvetica;fontSize=11;fontColor=default;shape=connector;anchorPointDirection=0;snapToPoint=0;bendable=1;noJump=0;flowAnimation=0;ignoreEdge=0;orthogonalLoop=0;orthogonal=0;comic=0;enumerate=0;
```

---

## Callout & Annotation Styles

### Generic Callout Box

```text
shape=partialRectangle;whiteSpace=wrap;html=1;right=0;top=0;bottom=0;fillColor=#FFFF99;routingCenterX=-0.5;fontFamily=Helvetica;fontSize=11;strokeColor=#d6b656;align=left;horizontal=1;verticalAlign=top;spacingLeft=0;movable=1;part=0;backgroundOutline=0;perimeter=rectanglePerimeter;container=0;
```

### NFR Callout Box

Same as generic callout with red bold text content using format:
```
NFR: {SVC Times} + {DU Time) = {Total}
```

### Cache Details Callout Box

Same as generic callout with 9px font detail content covering:
- Storage Scheme: `{LRU / FIFO}`
- Time to Live: `{1D, 24H, 3600M, 60S}`
- Refresh Frequency: `{Real-Time or Schedule}`
- Object Count Limit: `{# of Objects / Unlimited}`
- Storage Limit: `{Size in MB / Unlimited}`
- Initial State: `{Empty / PreLoad}`

---

## Template Structure (Level 1 Diagram Template)

The Level 1 Diagram Template consists of three pages:

### Page 1: Master Notes Page
- **Background**: References the Background page via `backgroundImage`
- **Header**: Title block with `{FG}-{####} - {FG Name}` and `{Scenario #} - {Scenario Name}`
- **Version**: Displayed as `1.0`
- **Content sections**:
  - General Information regarding all Scenarios
  - Feature Group Assumptions
  - Scenario Summary
- Each section has a bordered content area (stroke: `#E1E1E1`)

### Page 2: FG.01
- Working diagram page for the first Feature Group scenario
- Uses Background page as background image

### Page 3: Background
- Shared background page referenced by other pages
- Contains the standard UP template chrome/frame

---

## Template Usage Conventions

### Title Format
```
{FG}-{####} - {FG Name}
{Scenario #} - {Scenario Name}
```

### Service/Event Naming (Placeholder Properties)

Services and events use the `placeholders="1"` feature with custom properties:

| Property | Default Value | Description |
|----------|--------------|-------------|
| `name` | `service-name/1.0` | Service or event name with version |
| `metrics` | `[Daily Vol k/day]{Volume/sec}(Max Resp (ms))` | Performance metrics (events only) |

Label template: `%name%` or `%name%<div>%metrics%</div>`

### Key Style Rules

1. **All shapes** use `html=1;whiteSpace=wrap;` for HTML label rendering
2. **Font**: `fontFamily=Helvetica;fontSize=11;` (standard) or `fontSize=12;` (UI labels)
3. **Edges**: Always use `edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;` for routing
4. **Containers**: Use `container=1;collapsible=0;` for non-collapsible groupings
5. **Sequence indicators**: Use `connectable=0;` to prevent connection points
6. **Status differentiation**:
   - DU vs Logical Component: solid border vs dashed border (`dashed=1;dashPattern=1 4`)
   - Service type: arrow style (`classic` vs `block` vs `dash`)
   - Service scope: stroke color (default black = Enterprise; `#0000FF` = Endorsement)
   - Sync vs Async: line style (solid = sync; `dashed=1;dashPattern=8 8` = event; `endArrow=dash` = async/post)
   - Status: fill color (shapes) or label background color (edges)

---

## Adding a New Diagram Page

1. Add a new page in the draw.io file.
2. Open the **Diagram** panel on the right.
3. Select **Background → Change**.
4. Choose the **Background** page.
5. Select **Apply**.
6. Drag in the **Title Text** shape from the stencil library to the header area.
