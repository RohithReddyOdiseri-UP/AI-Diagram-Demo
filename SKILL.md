---
name: draw-io-diagram-generator
description: Use when creating, editing, or generating draw.io diagram files (.drawio, .drawio.svg, .drawio.png). Covers mxGraph XML authoring, shape libraries, style strings, flowcharts, system architecture, sequence diagrams, ER diagrams, UML class diagrams, network topology, layout strategy, the hediet.vscode-drawio VS Code extension, and the full agent workflow from request to a ready-to-open file.
---

# Draw.io Diagram Generator

This skill enables you to generate, edit, and validate draw.io (`.drawio`) diagram files with
correct mxGraph XML structure. All generated files open immediately in the
[Draw.io VS Code extension](https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio)
(`hediet.vscode-drawio`) without any manual fixes required. You can also open the files in the draw.io web app or desktop app if you prefer.

---

## 1. When to Use This Skill

**Trigger phrases (load this skill when you see these)**

- "create a diagram", "draw a flowchart", "generate an architecture diagram"
- "design a sequence diagram", "make a UML class diagram", "build an ER diagram"
- "add a .drawio file", "update the diagram", "visualise the flow"
- "document the architecture", "show the data model", "diagram the service interactions"
- "UP Level 1 diagram", "UP architecture", "deployment unit diagram", "feature group diagram"
- "Level 0 diagram", "Level 0.5 diagram", "service interaction diagram"
- Any request to produce or modify a `.drawio`, `.drawio.svg`, or `.drawio.png` file

**Supported diagram types**

| Diagram Type | Template Available | Description |
|---|---|---|
| **UP Level 0/0.5/1** | `assets/up-templates/Level 1 Diagram Template.drawio` | UP DU interactions, domain containers, services/events |
| **UP Level 2** | `assets/up-templates/Level 2 Diagram Template.drawio` | UP business process flows |

---

## 2. Prerequisites

- If running with VS Code integration enabled, Install the drawio extension: **draw.io VS Code extension** — `hediet.vscode-drawio` (extension id). Install with:
  ```
  ext install hediet.vscode-drawio
  ```
- **Supported file extensions**: `.drawio`, `.drawio.svg`, `.drawio.png`
- **Python 3.8+** (optional) — for the validation and shape-insertion scripts in `scripts/`

---

## 3. Step-by-Step Agent Workflow

Follow these steps in order for every diagram generation task.

### Step 1 — Understand the Request

Ask or infer:
1. **Diagram type** — What kind of diagram? (Level 0.5, Level 1, Level 2 ...)
2. **Entities / actors** — What are the main components, actors, classes, or tables?
3. **Relationships** — How are they connected? What direction? What cardinality?
4. **Output path** — Where should the `.drawio` file be saved?
5. **Existing file** — Are we creating new or editing an existing file?

If the request is ambiguous, infer the most sensible diagram type from context (e.g. "show the tables" → ER diagram, "show how the API call flows" → sequence diagram).

### Step 2 — Select a Template or Start Fresh

- **Use a template** when the diagram type matches one in `assets/up-templates/`. Copy the template structure and replace placeholder values.
- **Start fresh** for novel layouts. Begin with the minimal valid skeleton.

#### UP Architecture Templates

| Diagram Level | Template File | Description |
|---|---|---|
| Level 0 / 0.5 / 1 | `assets/up-templates/Level 1 Diagram Template.drawio` | 3-page template: Master Notes, FG diagram, Background |
| Level 2 | `assets/up-templates/Level 2 Diagram Template.drawio` | Business process flow template |

**UP template structure** (Level 1 Diagram Template):

| Page | Name | Purpose |
|------|------|---------|
| 1 | Master Notes Page | General info, Feature Group assumptions, scenario summary |
| 2 | FG.01 | Working architecture diagram page |
| 3 | Background | Shared template chrome; referenced by other pages via `backgroundImage` |

To add a new diagram page to the UP template:
1. Add a new `<diagram>` element after `FG.01`
2. Reference the Background page in `mxGraphModel` attributes:
   ```xml
   backgroundImage="{&quot;src&quot;:&quot;data:page/id,K9_SMR9-HG65Afowcf0h&quot;}"
   ```
3. Add the Title Text shape from the stencil library to the header area

#### Minimal Skeleton (Start Fresh)

```xml
<!-- Set modified="" to the current ISO 8601 timestamp when generating a new file -->
<mxfile host="Electron" modified="" version="26.0.0">
  <diagram id="page-1" name="Page-1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1169" pageHeight="827"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Your cells go here -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

#### UP Multi-Page Skeleton

```xml
<mxfile host="Electron" modified="" version="26.0.0" pages="3">
  <diagram name="Master Notes Page" id="notes-page">
    <mxGraphModel dx="2578" dy="1490" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  backgroundImage="{&quot;src&quot;:&quot;data:page/id,bg-page&quot;}"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Notes page content -->
      </root>
    </mxGraphModel>
  </diagram>
  <diagram name="FG.01" id="fg01-page">
    <mxGraphModel dx="2578" dy="1490" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  backgroundImage="{&quot;src&quot;:&quot;data:page/id,bg-page&quot;}"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Architecture diagram content -->
      </root>
    </mxGraphModel>
  </diagram>
  <diagram name="Background" id="bg-page">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Background template chrome -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

> **Rule**: ids `0` and `1` are ALWAYS required and must be the first two cells in every diagram page. Never reuse them.

### Step 3 — Plan the Layout

Before generating XML, sketch the logical placement:
- Organise into **rows** or **tiers** (use swimlanes for layers)
- **Horizontal spacing**: 40–60px between same-row shapes
- **Vertical spacing**: 80–120px between tier rows
- Standard shape size: `120x60` px for process boxes, `160x80` px for swimlanes
- Default canvas: A4 landscape = `1169 x 827` px

#### UP Level 1 Layout Conventions

For UP architecture diagrams, use the following layout strategy:

- **Canvas size**: `1700 x 1100` px (UP template standard)
- **Deployment Unit size**: `100 x 60` px
- **Logical Component size**: `100 x 60` px (dashed border, acts as container)
- **App/Domain container**: `140 x 110` px minimum; expand to fit children
- **UI container**: `130 x 100` px minimum; expand to fit children
- **Sequence indicators**: `30 x 30` px (Mandatory circle), `35 x 35` px (Optional diamond)
- **Database**: `37.5 x 50` px
- **Cloud**: `120 x 80` px
- **Actors (person icons)**: `24.18 x 63.73` px

**Grouping strategy**:
1. Place **App/Domain containers** as the outermost grouping (light cyan boundaries)
2. Nest **Deployment Units** and **Logical Components** inside their domain containers
3. Place **UI containers** (peach) near the top for user-facing components
4. Position **databases** and **caches** near the bottom or sides
5. Route **services/events** (edges) between DUs using orthogonal connectors
6. Attach **callouts** (yellow boxes) to shapes that need annotations (NFRs, cache details)
7. Place **sequence indicators** adjacent to DUs or services to show ordering

**Title placement**: Place the Title Text shape at coordinates `(910, 15)` in the header area with format:
```
{FG}-{####} - {FG Name}
{Scenario #} - {Scenario Name}
```

### Step 4 — Generate the mxGraph XML

**Vertex cell** (every shape):
```xml
<mxCell id="unique-id" value="Label"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry" />
</mxCell>
```

**Edge cell** (every connector):
```xml
<mxCell id="edge-id" value="Label (optional)"
        style="edgeStyle=orthogonalEdgeStyle;html=1;"
        edge="1" source="source-id" target="target-id" parent="1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

#### UP Service/Event Edges with Placeholders

UP services and events use draw.io's **placeholder** system via `<object>` wrappers. This allows labels to reference custom properties:

**Service (sync) with placeholder:**
```xml
<object placeholders="1" name="service-name/1.0" label="%name%" id="svc-1">
  <mxCell style="endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;"
          edge="1" source="du-source" target="du-target" parent="1">
    <mxGeometry relative="1" as="geometry" />
  </mxCell>
</object>
```

**Event (async message) with placeholder + metrics:**
```xml
<object label="%name%&lt;div&gt;%metrics%&lt;/div&gt;" placeholders="1"
        metrics="[Daily Vol k/day]{Volume/sec}(Max Resp (ms))"
        name="event-name/1.0" id="evt-1">
  <mxCell style="endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;"
          edge="1" source="du-source" target="du-target" parent="1">
    <mxGeometry relative="1" as="geometry" />
  </mxCell>
</object>
```

#### UP Container with Children

**App/Domain container with nested Deployment Unit:**
```xml
<!-- App/Domain container -->
<mxCell id="domain-1" value="Order Management"
        style="swimlane;whiteSpace=wrap;html=1;rounded=1;dashed=1;horizontal=1;align=right;spacingLeft=20;startSize=23;spacingRight=10;strokeColor=default;swimlaneLine=0;labelBackgroundColor=#E6FFFF;fillColor=#E6FFFF;swimlaneFillColor=#E6FFFF;fontColor=#808080;opacity=50;collapsible=0;fontStyle=0;verticalAlign=top;"
        vertex="1" parent="1">
  <mxGeometry x="200" y="200" width="300" height="200" as="geometry" />
</mxCell>

<!-- DU inside container (coords relative to parent container) -->
<mxCell id="du-1" value="Order Service"
        style="rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#c8d6e6;strokeWidth=0.5;"
        vertex="1" parent="domain-1">
  <mxGeometry x="30" y="40" width="100" height="60" as="geometry" />
</mxCell>
```

**Critical rules**:
- Every cell id must be **globally unique** within the file
- Every vertex must have an `mxGeometry` child with `x`, `y`, `width`, `height`, `as="geometry"`
- Every edge must have `source` and `target` matching existing vertex ids — **exception**: floating edges (e.g. sequence diagram lifelines) use `sourcePoint`/`targetPoint` inside `<mxGeometry>` instead; see §4 Sequence Diagram
- Every cell's `parent` must reference an existing cell id
- Use `html=1` in style when the label contains HTML (`<b>`, `<i>`, `<br>`)
- Escape XML special characters in labels: `&` => `&amp;`, `<` => `&lt;`, `>` => `&gt;`
- For UP services using placeholders, wrap `<mxCell>` inside `<object placeholders="1" ...>` with `label="%name%"` and custom properties as attributes
- Container children use coordinates **relative to the parent container**, not the canvas

### Step 5 — Apply Correct Styles

#### UP Level 1 Color Palette

For Union Pacific architecture diagrams, use this status-based color scheme:

**Component Status Colors (shape fills):**

| Status | fillColor | Usage |
|---|---|---|
| Needs Decomposition | `#ffff00` | DU or Logical Component requiring further breakdown |
| New | `#c8d6e6` | Newly introduced component |
| Rework | `#CADAA9` | Existing component being modified |
| Existing | *(default/white)* | Unchanged pre-existing component |
| External | `#FFCB7D` | Third-party / external component |

**Service Label Background Colors (edges):**

| Status | labelBackgroundColor | Usage |
|---|---|---|
| Existing | *(none)* | No highlight |
| Rework | `#CADAA9` | Service being reworked |
| New | `#C8D6E6` | New service |

**Container Colors:**

| Container | fillColor | Usage |
|---|---|---|
| App/Domain | `#E6FFFF` | Domain boundary / application grouping |
| UI | `#FDEFE3` | User interface container |

**Infrastructure Colors:**

| Element | fillColor | strokeColor | Usage |
|---|---|---|---|
| Database | `#F8B57E` | `#666666` | Data store |
| Callout | `#FFFF99` | `#d6b656` | Annotations |
| Sequence Indicator | `#FFC000` | none | Mandatory/Optional markers |

#### UP Level 1 Style Strings

```
# App/Domain Container
swimlane;whiteSpace=wrap;html=1;rounded=1;dashed=1;horizontal=1;align=right;spacingLeft=20;startSize=23;spacingRight=10;strokeColor=default;swimlaneLine=0;labelBackgroundColor=#E6FFFF;fillColor=#E6FFFF;swimlaneFillColor=#E6FFFF;fontColor=#808080;opacity=50;collapsible=0;fontStyle=0;verticalAlign=top;

# UI Container
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=12;fontColor=#808080;verticalAlign=top;align=right;fillColor=#FDEFE3;opacity=50;collapsible=0;container=1;dropTarget=1;

# Deployment Unit (New)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#c8d6e6;strokeWidth=0.5;

# Deployment Unit (Rework)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#CADAA9;strokeWidth=0.5;

# Deployment Unit (Existing)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;strokeWidth=0.5;

# Deployment Unit (Needs Decomposition)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#ffff00;strokeWidth=0.5;

# Deployment Unit (External)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#FFCB7D;strokeWidth=0.5;

# Logical Component (New)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#c8d6e6;strokeWidth=0.5;fontColor=default;dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;

# Logical Component (Rework)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#CADAA9;strokeWidth=0.5;fontColor=default;dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;

# Logical Component (Existing)
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;strokeWidth=0.5;fontColor=default;dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;

# Mandatory Sequence Indicator
ellipse;whiteSpace=wrap;html=1;rounded=1;fontSize=12;fillColor=#FFC000;strokeColor=none;connectable=0;

# Optional Sequence Indicator
rhombus;whiteSpace=wrap;html=1;rounded=0;fillColor=#FFC000;strokeColor=default;connectable=0;rotation=0;

# Database
strokeWidth=1;html=1;shape=mxgraph.flowchart.database;whiteSpace=wrap;strokeColor=#666666;fillColor=#F8B57E;

# Cloud
ellipse;shape=cloud;whiteSpace=wrap;html=1;

# Timer (BPMN)
points=[[0.145,0.145,0],[0.5,0,0],[0.855,0.145,0],[1,0.5,0],[0.855,0.855,0],[0.5,1,0],[0.145,0.855,0],[0,0.5,0]];shape=mxgraph.bpmn.event;html=1;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;verticalAlign=top;align=center;perimeter=ellipsePerimeter;outlineConnect=0;aspect=fixed;outline=standard;symbol=timer;fontFamily=Helvetica;fontSize=11;fontColor=default;

# Callout Box
shape=partialRectangle;whiteSpace=wrap;html=1;right=0;top=0;bottom=0;fillColor=#FFFF99;routingCenterX=-0.5;fontFamily=Helvetica;fontSize=11;strokeColor=#d6b656;align=left;horizontal=1;verticalAlign=top;spacingLeft=0;

# Callout Connector Line
endArrow=none;html=1;rounded=0;labelBackgroundColor=default;strokeColor=#999999;fontFamily=Helvetica;fontSize=11;fontColor=default;shape=connector;
```

#### UP Level 1 Edge (Service/Event) Styles

```
# Enterprise Service (Existing) — solid black arrow
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;

# Enterprise Service (Rework) — green label bg
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;

# Enterprise Service (New) — blue label bg
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;

# Enterprise Event (Existing) — dashed black arrow
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;

# Enterprise Event (Rework)
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;dashed=1;dashPattern=8 8;

# Enterprise Event (New)
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;dashed=1;dashPattern=8 8;

# Enterprise Async / Post Service
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;

# Endorsement Service (Existing) — blue hollow block arrow
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Service (Rework)
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Service (New)
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Event (Existing) — dashed blue hollow block
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Event (Rework)
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#CADAA9;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Event (New)
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;

# Endorsement Async / Post Service
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;

# Special Message — thick dashed
endArrow=classic;html=1;rounded=0;dashed=1;dashPattern=1 2;strokeWidth=2;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

#### UP Key Style Rules

1. **All shapes** use `html=1;whiteSpace=wrap;` for HTML label rendering
2. **Font**: `fontFamily=Helvetica;fontSize=11;` (standard) or `fontSize=12;` (UI container labels)
3. **Edges**: Always use `edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;` for routing
4. **Containers**: Use `container=1;collapsible=0;` for non-collapsible groupings
5. **Sequence indicators**: Use `connectable=0;` to prevent connection points
6. **Status differentiation**:
   - DU vs Logical Component: solid border vs dashed border (`dashed=1;dashPattern=1 4`)
   - Service type: arrow style (`classic` = sync, `block` = endorsement, `dash` = async/post)
   - Service scope: stroke color (default black = Enterprise; `#0000FF` = Endorsement)
   - Sync vs Async: line style (solid = sync; `dashed=1;dashPattern=8 8` = event)
   - Status: fill color (shapes) or `labelBackgroundColor` (edges)

> See `up-references/UP-level-1-style-reference.md` for the complete UP style catalog and `up-references/UP-level-1-shape-library.md` for all UP shape details.

### Step 6 — Save and Validate

1. **Write the file** to the requested path with `.drawio` extension
2. **Run the validator** (optional but recommended):
   ```bash
   python .github/skills/draw-io-up-diagram-generator/scripts/validate-drawio.py <path-to-file.drawio>
   ```
3. **Tell the user** how to open the file:
   > "Open `<filename>` in VS Code — it will render automatically with the draw.io extension. You can use draw.io's web app or desktop app as well if you prefer."
4. **Provide a brief description** of what is in the diagram so the user knows what to expect.

---

## 4. Diagram-Type Recipes

### UP Level 1 Architecture Diagram

UP Level 1 diagrams show Deployment Unit (DU) interactions within application domains. They use the UP Level 1 shape library and follow a status-based color convention.

**Key elements**: App/Domain containers → Deployment Units → Services/Events (edges) → Callouts

**Complete example** — two domains with DUs connected by an enterprise service:

```xml
<mxfile host="Electron" modified="2026-05-01T00:00:00.000Z" version="26.0.0" pages="3">
  <diagram name="Master Notes Page" id="notes-1">
    <mxGraphModel dx="2578" dy="1490" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  backgroundImage="{&quot;src&quot;:&quot;data:page/id,bg-1&quot;}"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Notes content here -->
      </root>
    </mxGraphModel>
  </diagram>
  <diagram name="FG.01" id="fg01">
    <mxGraphModel dx="2578" dy="1490" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  backgroundImage="{&quot;src&quot;:&quot;data:page/id,bg-1&quot;}"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />

        <!-- App/Domain Container: Order Management -->
        <mxCell id="domain-orders" value="Order Management"
                style="swimlane;whiteSpace=wrap;html=1;rounded=1;dashed=1;horizontal=1;align=right;spacingLeft=20;startSize=23;spacingRight=10;strokeColor=default;swimlaneLine=0;labelBackgroundColor=#E6FFFF;fillColor=#E6FFFF;swimlaneFillColor=#E6FFFF;fontColor=#808080;opacity=50;collapsible=0;fontStyle=0;verticalAlign=top;"
                vertex="1" parent="1">
          <mxGeometry x="100" y="200" width="350" height="250" as="geometry" />
        </mxCell>

        <!-- Deployment Unit (New) inside Order Management -->
        <mxCell id="du-order-svc" value="Order Service"
                style="rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#c8d6e6;strokeWidth=0.5;"
                vertex="1" parent="domain-orders">
          <mxGeometry x="40" y="50" width="100" height="60" as="geometry" />
        </mxCell>

        <!-- Deployment Unit (Existing) inside Order Management -->
        <mxCell id="du-order-db" value="Order DB"
                style="strokeWidth=1;html=1;shape=mxgraph.flowchart.database;whiteSpace=wrap;strokeColor=#666666;fillColor=#F8B57E;"
                vertex="1" parent="domain-orders">
          <mxGeometry x="200" y="55" width="37.5" height="50" as="geometry" />
        </mxCell>

        <!-- App/Domain Container: Payment -->
        <mxCell id="domain-payment" value="Payment"
                style="swimlane;whiteSpace=wrap;html=1;rounded=1;dashed=1;horizontal=1;align=right;spacingLeft=20;startSize=23;spacingRight=10;strokeColor=default;swimlaneLine=0;labelBackgroundColor=#E6FFFF;fillColor=#E6FFFF;swimlaneFillColor=#E6FFFF;fontColor=#808080;opacity=50;collapsible=0;fontStyle=0;verticalAlign=top;"
                vertex="1" parent="1">
          <mxGeometry x="600" y="200" width="300" height="250" as="geometry" />
        </mxCell>

        <!-- Deployment Unit (Rework) inside Payment -->
        <mxCell id="du-pay-svc" value="Payment Gateway"
                style="rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=#CADAA9;strokeWidth=0.5;"
                vertex="1" parent="domain-payment">
          <mxGeometry x="40" y="50" width="100" height="60" as="geometry" />
        </mxCell>

        <!-- Enterprise Service (New): Order Service → Payment Gateway -->
        <object placeholders="1" name="process-payment/2.0" label="%name%" id="svc-pay">
          <mxCell style="endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;"
                  edge="1" source="du-order-svc" target="du-pay-svc" parent="1">
            <mxGeometry relative="1" as="geometry" />
          </mxCell>
        </object>

        <!-- Enterprise Event (New): Payment Gateway → Order Service -->
        <object label="%name%&lt;div&gt;%metrics%&lt;/div&gt;" placeholders="1"
                metrics="[50k/day]{2/sec}(200ms)"
                name="payment-confirmed/1.0" id="evt-confirmed">
          <mxCell style="endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=#C8D6E6;dashed=1;dashPattern=8 8;"
                  edge="1" source="du-pay-svc" target="du-order-svc" parent="1">
            <mxGeometry relative="1" as="geometry" />
          </mxCell>
        </object>

        <!-- Mandatory Sequence Indicator -->
        <mxCell id="seq-1" value="1"
                style="ellipse;whiteSpace=wrap;html=1;rounded=1;fontSize=12;fillColor=#FFC000;strokeColor=none;connectable=0;"
                vertex="1" parent="1">
          <mxGeometry x="70" y="270" width="30" height="30" as="geometry" />
        </mxCell>

        <!-- Callout annotation -->
        <mxCell id="callout-1" value="NFR: 50ms + 150ms = 200ms"
                style="shape=partialRectangle;whiteSpace=wrap;html=1;right=0;top=0;bottom=0;fillColor=#FFFF99;routingCenterX=-0.5;fontFamily=Helvetica;fontSize=11;strokeColor=#d6b656;align=left;horizontal=1;verticalAlign=top;spacingLeft=0;"
                vertex="1" parent="1">
          <mxGeometry x="100" y="500" width="240" height="30" as="geometry" />
        </mxCell>

        <!-- Callout connector line -->
        <mxCell id="callout-1-edge" value=""
                style="endArrow=none;html=1;rounded=0;labelBackgroundColor=default;strokeColor=#999999;fontFamily=Helvetica;fontSize=11;fontColor=default;shape=connector;"
                edge="1" source="callout-1" target="du-order-svc" parent="1">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
  <diagram name="Background" id="bg-1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1"
                  page="1" pageScale="1" pageWidth="1700" pageHeight="1100"
                  math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <!-- Background template chrome -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

**UP Level 1 edge decision guide:**

| Communication Type | Scope | Status | Style Key Differences |
|---|---|---|---|
| Sync Service | Enterprise | Existing | `endArrow=classic;` (no label bg) |
| Sync Service | Enterprise | New | `endArrow=classic;labelBackgroundColor=#C8D6E6;` |
| Sync Service | Enterprise | Rework | `endArrow=classic;labelBackgroundColor=#CADAA9;` |
| Async Event | Enterprise | Existing | `endArrow=classic;dashed=1;dashPattern=8 8;` |
| Async Event | Enterprise | New | `endArrow=classic;dashed=1;dashPattern=8 8;labelBackgroundColor=#C8D6E6;` |
| Fire-and-Forget | Enterprise | Any | `endArrow=dash;endFill=0;` |
| Sync Service | Endorsement | Existing | `endArrow=block;endFill=0;strokeColor=#0000FF;` |
| Sync Service | Endorsement | New | `endArrow=block;endFill=0;strokeColor=#0000FF;labelBackgroundColor=#C8D6E6;` |
| Async Event | Endorsement | Existing | `endArrow=block;endFill=0;strokeColor=#0000FF;dashed=1;dashPattern=8 8;` |
| Fire-and-Forget | Endorsement | Any | `endArrow=dash;endFill=0;strokeColor=#0000FF;` |
| Special Message | — | — | `strokeWidth=2;dashed=1;dashPattern=1 2;` |

**Key conventions:**
- Always wrap services in `<object placeholders="1" name="service-name/version" label="%name%">`
- Events additionally include `metrics` attribute: `metrics="[Daily Vol k/day]{Volume/sec}(Max Resp (ms))"`
- All UP edges use `edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;` for clean routing with arc jumps
- Load the UP Level 1 Library via `File > Open Library…` → `up-references/UP Level 1 Library.xml`

---

## 5. Multi-Page Diagrams

Add multiple `<diagram>` elements for complex systems:

```xml
<mxfile host="Electron" version="26.0.0">
  <diagram id="overview" name="Overview">
    <!-- overview mxGraphModel -->
  </diagram>
  <diagram id="detail" name="Detail View">
    <!-- detail mxGraphModel -->
  </diagram>
</mxfile>
```

Each page has its own independent cell id namespace. The same id value can appear in different pages without conflict.

---

## 6. Editing Existing Diagrams

When modifying an existing `.drawio` file:

1. **Read** the file first to understand existing cell ids, positions, and parent hierarchy
2. **Identify the target diagram page** — by index or `name` attribute
3. **Assign new unique ids** that do not collide with existing ids
4. **Respect the container hierarchy** — children of a swimlane use coordinates relative to their parent
5. **Verify edges** — after repositioning nodes, confirm edge source/target ids remain valid

Use `scripts/add-shape.py` to safely add a single shape without editing raw XML:
```bash
python .github/skills/draw-io-up-diagram-generator/scripts/add-shape.py docs/arch.drawio "New Service" 700 380
```

---

## 7. Best Practices

**Layout**
- Align shapes to the 10px grid (all coordinates divisible by 10)
- Group related shapes inside swimlane containers
- One diagram topic per page; use multi-page files for complex systems
- Aim for 40 or fewer cells per page for readability

**Labels**
- Add a title text cell (`text;strokeColor=none;fillColor=none;fontSize=18;fontStyle=1`) at top of every page
- Always set `whiteSpace=wrap;html=1` on vertex shapes
- Keep labels concise — 3 words or fewer per shape where possible

**Style consistency**
- Use the semantic color palette from Section 3 Step 5 consistently across a project
- Prefer `edgeStyle=orthogonalEdgeStyle` for clean right-angle connectors
- Do not inline arbitrary HTML in labels unless necessary

**File naming**
- Use kebab-case: `order-service-flow.drawio`, `database-schema.drawio`
- Place diagrams alongside the code they document: `docs/` or `architecture/`

---

## 8. Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| File opens blank in VS Code | Missing id=0 or id=1 cell | Add both root cells before any other cells |
| Shape at wrong position | Child inside container — coords are relative | Check `parent`; adjust x/y relative to container |
| Edge not visible | source or target id does not match any vertex | Verify both ids exist exactly as written |
| Diagram shows "Compressed" | mxGraphModel is base64-encoded | Open in draw.io web, File > Export > XML (uncompressed) |
| Shape style not rendering | Typo in shape= name | Check `up-references/UP-level-1-shape-library.md` for exact style string |
| Label shows escaped HTML | html=0 on a cell with HTML label | Add `html=1;` to the cell style |
| Container children overlap container edge | Container height too small | Increase container height in mxGeometry |

---

## 9. Validation Checklist

Before delivering any generated `.drawio` file, verify:

- [ ] File starts with `<mxfile>` root element
- [ ] Every `<diagram>` has a non-empty `id` attribute
- [ ] `<mxCell id="0" />` is the first cell in every diagram
- [ ] `<mxCell id="1" parent="0" />` is the second cell in every diagram
- [ ] All cell `id` values are unique within each diagram
- [ ] Every vertex cell has `vertex="1"` and a child `<mxGeometry as="geometry">`
- [ ] Every edge cell has `edge="1"` and either: (a) `source`/`target` pointing to existing vertex ids, or (b) `<mxPoint as="sourcePoint">` and `<mxPoint as="targetPoint">` in its `<mxGeometry>` (floating edge — used for sequence diagram lifelines)
- [ ] Every cell (except id=0) has a `parent` pointing to an existing id
- [ ] `html=1` is in the style for any label containing HTML tags
- [ ] XML is well-formed (no unclosed tags, no unescaped `&`, `<`, `>` in attribute values)
- [ ] A title label cell exists at the top of each page

Run the automated validator:
```bash
python .github/skills/draw-io-up-diagram-generator/scripts/validate-drawio.py <file.drawio>
```

---

## 10. Output Format

When delivering a diagram, always provide:

1. **The `.drawio` file** written to the requested path
2. **A one-sentence summary** of what the diagram shows
3. **How to open it**:
   > "Open `<filename>` in VS Code — the draw.io extension will render it automatically. Or you can open it in the draw.io web app or desktop app if you prefer."
4. **How to edit it** (if the user is likely to customise):
   > "Click any shape to select it. Double-click to edit the label. Drag to reposition."
5. **Validation status** — whether the validator script was run and passed

---

## 11. UP References

| File | Contents |
|---|---|
| `up-references/UP-level-1-shape-library.md` | UP Level 1 shape catalog — all 50+ shapes organized by category with sizes and style patterns |
| `up-references/UP-level-1-style-reference.md` | UP Level 1 style reference — color palette, exact style strings, template structure, usage conventions |
| `up-references/UP Level 1 Library.xml` | UP Level 1 draw.io shape library (load via File > Open Library…) |
| `up-references/UP Level 2 Library.xml` | UP Level 2 draw.io shape library |
| `assets/up-templates/Level 1 Diagram Template.drawio` | 3-page UP template for Level 0/0.5/1 diagrams |
| `assets/up-templates/Level 2 Diagram Template.drawio` | UP template for Level 2 business process diagrams |
