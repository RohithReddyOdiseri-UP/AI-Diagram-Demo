# UP Level 1 Shape Library

Reference guide for the Union Pacific Level 1 draw.io shape library (`UP Level 1 Library.xml`). This library provides standard shapes for Level 0, Level 0.5, and Level 1 architecture diagrams.

Load via `File > Open Library…` in draw.io.

---

## Shape Catalog

### Containers & Grouping

| Shape | Title | Size (w×h) | Description |
|-------|-------|------------|-------------|
| Swimlane | App/Domain | 140×110 | Domain/Application grouping container. Rounded, dashed border with light cyan fill (`#E6FFFF`). Label positioned top-right in gray. |
| Rounded box | UI | 130×100 | UI container. Rounded corners, peach fill (`#FDEFE3`), 50% opacity. Acts as drop target container. |

---

### Sequence Indicators

| Shape | Title | Size (w×h) | Description |
|-------|-------|------------|-------------|
| Ellipse | Mandatory Sequence | 30×30 | Gold-filled circle (`#FFC000`), no stroke. Used to mark mandatory steps in a flow. Non-connectable. |
| Rhombus | Optional Sequence | 35×35 | Gold-filled diamond (`#FFC000`), default stroke. Used to mark optional steps. Non-connectable. |

---

### Deployment Units

Deployment units represent deployable software components. All are rounded rectangles (100×60) with `strokeWidth=0.5`.

| Title | Fill Color | Meaning |
|-------|-----------|---------|
| Deployment Unit (Needs Decomposition) | `#ffff00` (Yellow) | Requires further breakdown at Level 2 |
| Deployment Unit (New) | `#c8d6e6` (Light Blue) | New component being introduced |
| Deployment Unit (Rework) | `#CADAA9` (Light Green) | Existing component being reworked |
| Deployment Unit (Existing) | *(default white)* | Pre-existing, unchanged component |
| Deployment Unit (External) | `#FFCB7D` (Orange) | External/third-party component |

**Style pattern:**
```text
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=<COLOR>;strokeWidth=0.5;
```

---

### Logical Components

Logical components represent non-deployable logical groupings. All are rounded rectangles (100×60) with **dashed borders** (`dashed=1;dashPattern=1 4`) and act as containers.

| Title | Fill Color | Meaning |
|-------|-----------|---------|
| Logical Component (Needs Composition) | `#ffff00` (Yellow) | Requires further decomposition |
| Logical Component (New) | `#c8d6e6` (Light Blue) | New logical component |
| Logical Component (Rework) | `#CADAA9` (Light Green) | Existing component being reworked |
| Logical Component (Existing) | *(default white)* | Pre-existing logical component |
| Logical Component (External) | `#FFCB7D` (Orange) | External logical component |

**Style pattern:**
```text
rounded=1;whiteSpace=wrap;html=1;fontFamily=Helvetica;fontSize=11;fillColor=<COLOR>;strokeWidth=0.5;dashed=1;dashPattern=1 4;container=1;collapsible=0;recursiveResize=0;
```

---

### Actors / People

| Shape | Title | Size (w×h) | Description |
|-------|-------|------------|-------------|
| Image (person icon) | Business Partner | 24.18×63.73 | Stick-figure person icon representing a business partner. |
| Image (person icon) | External User | 24.18×63.72 | Stick-figure person icon representing an external user. |

---

### Services (Edge Connectors)

Services are represented as **edges** (arrows/connectors) with orthogonal routing and arc jump style. They use **placeholders** for label display (`%name%`) and a custom property `name` (default: `service-name/1.0`).

#### Enterprise Services (Synchronous)

Solid arrow with `endArrow=classic`.

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Enterprise Service (Existing) | *(none)* | Pre-existing service call |
| Enterprise Service (Rework) | `#CADAA9` (Light Green) | Service being reworked |
| Enterprise Service (New) | `#C8D6E6` (Light Blue) | New service being introduced |

**Style pattern:**
```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;labelBackgroundColor=<COLOR>;
```

#### Enterprise Events (Asynchronous Messages)

Dashed arrow (`dashed=1;dashPattern=8 8`) with `endArrow=classic`. Includes `metrics` placeholder for volume data.

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Enterprise Event (Existing) | *(none)* | Pre-existing event |
| Enterprise Event (Rework) | `#CADAA9` (Light Green) | Event being reworked |
| Enterprise Event (New) | `#C8D6E6` (Light Blue) | New event |

**Style pattern:**
```text
endArrow=classic;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;labelBackgroundColor=<COLOR>;
```

#### Enterprise Async / Post Services

Dash-ended arrow (`endArrow=dash;endFill=0`) representing fire-and-forget or async post calls.

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Enterprise Async / Post Service (Existing) | *(none)* | Pre-existing async call |
| Enterprise Async / Post Service (Rework) | *(none)* | Async call being reworked |
| Enterprise Async / Post Service (New) | *(none)* | New async call |

**Style pattern:**
```text
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

#### Endorsement Services (Blue)

Solid blue arrow (`strokeColor=#0000FF`) with hollow block end (`endArrow=block;endFill=0`).

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Endorsement Service (Existing) | *(none)* | Pre-existing endorsement service |
| Endorsement Service (Rework) | `#CADAA9` (Light Green) | Endorsement service being reworked |
| Endorsement Service (New) | `#C8D6E6` (Light Blue) | New endorsement service |

**Style pattern:**
```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;labelBackgroundColor=<COLOR>;
```

#### Endorsement Events (Blue, Dashed)

Dashed blue arrow representing endorsement events.

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Endorsement Event (Existing) | *(none)* | Pre-existing endorsement event |
| Endorsement Event (Rework) | `#CADAA9` (Light Green) | Endorsement event being reworked |
| Endorsement Event (New) | `#C8D6E6` (Light Blue) | New endorsement event |

**Style pattern:**
```text
endArrow=block;html=1;rounded=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;dashed=1;dashPattern=8 8;fillColor=#dae8fc;strokeColor=#0000FF;endFill=0;labelBackgroundColor=<COLOR>;
```

#### Endorsement Async / Post Services (Blue)

Dash-ended blue arrow for async endorsement interactions.

| Title | Label Background | Meaning |
|-------|-----------------|---------|
| Endorsement Async / Post Service (Existing) | *(none)* | Existing endorsement async service |
| Endorsement Async / Post Service (Rework) | *(none)* | Endorsement async service being reworked |
| Endorsement Async / Post Service (New) | *(none)* | New endorsement async service |

**Style pattern:**
```text
endArrow=dash;html=1;rounded=0;endFill=0;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;fillColor=#dae8fc;strokeColor=#0000FF;
```

#### Exclusive Services

*(Exclusive services follow similar patterns to Enterprise and Endorsement but are scoped to exclusive interactions.)*

| Title | Meaning |
|-------|---------|
| Exclusive Service (Existing) | Pre-existing exclusive service |
| Exclusive Service (Rework) | Exclusive service being reworked |
| Exclusive Async / Post Service (Existing) | Existing exclusive async service |
| Exclusive Async / Post Service (Rework) | Exclusive async service being reworked |
| Exclusive Async / Post Service (New) | New exclusive async service |
| Exclusive Event (Existing) | Pre-existing exclusive event |
| Exclusive Event (Rework) | Exclusive event being reworked |
| Exclusive Event (New) | New exclusive event |

---

### Special Message

A dashed, thick connector (`strokeWidth=2;dashed=1;dashPattern=1 2`) used for special/non-standard message flows between components.

**Style:**
```text
endArrow=classic;html=1;rounded=0;dashed=1;dashPattern=1 2;strokeWidth=2;edgeStyle=orthogonalEdgeStyle;jumpStyle=arc;
```

---

### Infrastructure & Data

| Shape | Title | Size (w×h) | Description |
|-------|-------|------------|-------------|
| BPMN Timer | Timer | 30×30 | Standard BPMN timer event icon. |
| Flowchart Database | Database | 37.5×50 | Drum/cylinder shape. Orange fill (`#F8B57E`), gray stroke. |
| Group (DB + Timer) | Local Cache | 50×60 | Database with timer overlay — represents a local cache. |
| Group (DB + Timer + network icon) | Distributed Cache | 90×63 | Database + timer + distributed network indicator. |
| Cloud | Cloud | 120×80 | Standard cloud shape. |
| Image (Loop) | Loop | 26.42×26 | Circular loop/retry indicator icon. |
| Image (Off-Page) | Off-Page Connector | 30.98×30.5 | Off-page connector icon for cross-diagram references. |
| Image (Filter) | Filter | *(varies)* | Filter/funnel icon. |
| Image (Printer) | Printer | *(varies)* | Printer icon for print output flows. |
| Image (Remove) | Remove | *(varies)* | Red X/remove indicator. |

---

### Callouts & Annotations

| Shape | Title | Size (w×h) | Description |
|-------|-------|------------|-------------|
| Partial Rectangle + Edge | Callout | 156×60 | Yellow callout box (`#FFFF99`) with a connector line. Used for notes/annotations on diagram elements. |
| Partial Rectangle + Edge | NFR Callout | 240×29.73 | Red-text callout specifically for Non-Functional Requirements (NFR) annotations. Shows service time + DU time = total format. |
| Partial Rectangle + Edge | Cache Details Callout | 244×100 | Multi-line yellow callout detailing cache configuration: Storage Scheme, TTL, Refresh Frequency, Object Count Limit, Storage Limit, Initial State. |

---

### Title & Layout

| Shape | Title | Description |
|-------|-------|-------------|
| Title Block | Title Block | Header grouping element for the diagram page. |
| Title Text | Title Text | Editable title text element placed in the diagram header. Format: `{FG}-{####} - {FG Name}` / `{Scenario #} - {Scenario Name}`. |

---

## Color Legend (Status Convention)

The UP Level 1 library uses a consistent color scheme to indicate the status of components:

| Color | Hex Code | Meaning | Used In |
|-------|----------|---------|---------|
| Yellow | `#ffff00` | Needs Decomposition | DU & Logical Component fills |
| Light Blue | `#c8d6e6` / `#C8D6E6` | New | DU & Logical Component fills; Service label backgrounds |
| Light Green | `#CADAA9` | Rework | DU & Logical Component fills; Service label backgrounds |
| White (default) | — | Existing (no change) | DU & Logical Component fills |
| Orange | `#FFCB7D` | External / Third-party | DU & Logical Component fills |
| Gold | `#FFC000` | Sequence indicator | Mandatory/Optional sequence markers |
| Blue | `#0000FF` | Endorsement scope | Endorsement services/events stroke |
| Light Cyan | `#E6FFFF` | Domain/App container | App/Domain swimlane fill |
| Peach | `#FDEFE3` | UI container | UI group fill |
| Database Orange | `#F8B57E` | Data store | Database shape fill |
| Callout Yellow | `#FFFF99` | Annotation | Callout & NFR fills |

---

## Loading the Library

1. Open draw.io (desktop or VS Code extension).
2. Select **File → Open Library…**
3. Navigate to and select `UP Level 1 Library.xml`.
4. The shapes will appear in the left-hand panel for drag-and-drop use.

---

## Usage Notes

- **Deployment Units** have solid borders; **Logical Components** have dashed borders.
- **Services** are connector/edge shapes — drag them between source and target nodes.
- Service labels use **placeholders** (`%name%`). Double-click the label to edit the `name` custom property (format: `service-name/version`).
- Event labels additionally support a `metrics` placeholder (format: `[Daily Vol k/day]{Volume/sec}(Max Resp (ms))`).
- Containers (App/Domain, UI) support child elements — drag shapes into them.
- The **Title Text** shape should be placed in the template header area for consistent branding.
