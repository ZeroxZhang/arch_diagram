---
name: architecture-diagram
description: Create professional, dark-themed architecture diagrams as standalone HTML files with SVG graphics. Use when the user asks for system architecture diagrams, infrastructure diagrams, cloud architecture visualizations, security diagrams, network topology diagrams, or any technical diagram showing system components and their relationships.
license: MIT
metadata:
  version: "2.0"
  author: Cocoon AI (hello@cocoon-ai.com)
---

# Architecture Diagram Skill

Create professional technical architecture diagrams as self-contained HTML files with inline SVG graphics and CSS styling.

## Workflow

**You MUST follow these steps in order:**

### Step 1: Layout Planning (REQUIRED)

Before writing any SVG code, create an ASCII layout plan. This prevents arrow misrouting and overlapping.

```
Format:
[Component]  x,y  WxH  → edges: →Target1, →Target2
```

Example:
```
[Users]        x=40,y=300   W=120,H=60  → CloudFront
[CloudFront]   x=220,y=300  W=120,H=60  → ALB
[ALB]          x=400,y=300  W=120,H=60  → API Server, →Auth
[API Server]   x=580,y=300  W=140,H=60  → DB, →Redis
[DB]           x=780,y=280  W=120,H=60  → (none)
[Redis]        x=780,y=380  W=120,H=60  → (none)
[Auth]         x=580,y=440  W=140,H=60  → DB
```

Rules for the ASCII plan:
- List ALL components with their intended x, y, width, height
- List ALL connections (edges) for each component
- Verify no two components share the same bounding box
- Ensure arrow sources and targets have clear, non-overlapping paths

### Step 2: Coordinate Calculation

Use these formulas to calculate arrow coordinates. **NEVER guess coordinates.**

#### Horizontal arrows (left → right)
```
source: x=sx, y=sy, width=sw, height=sh
target: x=tx, y=ty, width=tw, height=th

arrow_start_x = sx + sw          // right edge of source
arrow_end_x   = tx - 2           // 2px gap before target left edge
arrow_y       = sy + sh / 2      // vertical center of source
              = ty + th / 2      // must also equal vertical center of target (aligned rows)
```

#### Horizontal arrows (right → left)
```
arrow_start_x = sx               // left edge of source
arrow_end_x   = tx + tw + 2      // 2px gap after target right edge
arrow_y       = sy + sh / 2
```

#### Vertical arrows (top → bottom)
```
source: x=sx, y=sy, width=sw, height=sh
target: x=tx, y=ty, width=tw, height=th

arrow_x       = sx + sw / 2      // horizontal center
arrow_start_y = sy + sh          // bottom edge of source
arrow_end_y   = ty - 2           // 2px gap before target top edge
```

#### Vertical arrows (bottom → top)
```
arrow_x       = sx + sw / 2
arrow_start_y = sy               // top edge of source
arrow_end_y   = ty + th + 2      // 2px gap after target bottom edge
```

#### Arrow label positioning
```
Horizontal arrow label:  label_x = (arrow_start_x + arrow_end_x) / 2
                         label_y = arrow_y - 8     // 8px above the arrow line

Vertical arrow label:    label_x = arrow_x + 14    // 14px to the right
                         label_y = (arrow_start_y + arrow_end_y) / 2 + 4
```

### Step 3: Generate SVG

Follow the design system below to generate the final HTML/SVG.

---

## Design System

### Color Palette

Use these semantic colors for component types:

| Component Type | Fill (rgba) | Stroke |
|---------------|-------------|--------|
| Frontend | `rgba(8, 51, 68, 0.4)` | `#22d3ee` (cyan-400) |
| Backend | `rgba(6, 78, 59, 0.4)` | `#34d399` (emerald-400) |
| Database | `rgba(76, 29, 149, 0.4)` | `#a78bfa` (violet-400) |
| Cache/Redis | `rgba(88, 28, 135, 0.35)` | `#c084fc` (purple-400) |
| AWS/Cloud | `rgba(120, 53, 15, 0.3)` | `#fbbf24` (amber-400) |
| Security | `rgba(136, 19, 55, 0.4)` | `#fb7185` (rose-400) |
| Message Bus | `rgba(251, 146, 60, 0.3)` | `#fb923c` (orange-400) |
| API Gateway | `rgba(21, 94, 117, 0.35)` | `#06b6d4` (cyan-500) |
| Container/K8s | `rgba(30, 64, 175, 0.3)` | `#60a5fa` (blue-400) |
| External/Generic | `rgba(30, 41, 59, 0.5)` | `#94a3b8` (slate-400) |

### Typography

Use JetBrains Mono for all text (monospace, technical aesthetic):
```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Font sizes: 12px for component names, 9px for sublabels, 8px for annotations, 7px for tiny labels.

### Grid Alignment

**ALL component x, y coordinates MUST be aligned to a 20px grid.**

This means every x and y value must be divisible by 20 (e.g., 40, 60, 80, 100, 120...).

Width and height values must be divisible by 10.

This ensures:
- Components visually align in clean rows and columns
- Arrow calculations are predictable
- No accidental overlaps from misaligned positions

### Visual Elements

**Background:** `#020617` (slate-950) with subtle grid pattern:
```svg
<pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
  <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#1e293b" stroke-width="0.5"/>
</pattern>
```

**Component boxes:** Rounded rectangles (`rx="6"`) with 1.5px stroke, semi-transparent fills.

**Security groups:** Dashed stroke (`stroke-dasharray="4,4"`), transparent fill, rose color.

**Region boundaries:** Larger dashed stroke (`stroke-dasharray="8,4"`), amber color, `rx="12"`.

### Masking arrows behind transparent fills

Since component boxes use semi-transparent fills (`rgba(..., 0.4)`), arrows behind them will show through. To fully mask arrows, draw an opaque background rect (e.g., `fill="#0f172a"`) at the same position before drawing the semi-transparent styled rect on top:
```svg
<!-- Opaque background to mask arrows -->
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#0f172a"/>
<!-- Styled component on top -->
<rect x="X" y="Y" width="W" height="H" rx="6" fill="rgba(76, 29, 149, 0.4)" stroke="#a78bfa" stroke-width="1.5"/>
```

**Auth/security flows:** Dashed lines in rose color (`#fb7185`).

**Message buses / Event buses:** Small connector elements between services. Use orange color (`#fb923c` stroke, `rgba(251, 146, 60, 0.3)` fill):
```svg
<rect x="X" y="Y" width="120" height="20" rx="4" fill="rgba(251, 146, 60, 0.3)" stroke="#fb923c" stroke-width="1"/>
<text x="CENTER_X" y="Y+14" fill="#fb923c" font-size="7" text-anchor="middle">Kafka / RabbitMQ</text>
```

### Spacing Rules

**CRITICAL:** When stacking components vertically, ensure proper spacing to avoid overlaps:

- **Standard component height:** 60px for services, 80-120px for larger components
- **Minimum vertical gap between components:** 40px
- **Minimum horizontal gap between components:** 40px
- **Inline connectors (message buses):** Place IN the gap between components, not overlapping

**Example vertical layout:**
```
Component A: y=70,  height=60  → ends at y=130
Gap:         y=130 to y=170   → 40px gap, place bus at y=140 (20px tall)
Component B: y=170, height=60  → ends at y=230
```

**Wrong:** Placing a message bus at y=160 when Component B starts at y=170 (causes overlap)
**Right:** Placing a message bus at y=140, centered in the 40px gap (y=130 to y=170)

### Legend Placement

**CRITICAL:** Place legends OUTSIDE all boundary boxes (region boundaries, cluster boundaries, security groups).

- Calculate where all boundaries end (y position + height)
- Place legend at least 20px below the lowest boundary
- Expand SVG viewBox height if needed to accommodate

**Example:**
```
Kubernetes Cluster: y=30, height=460 → ends at y=490
Legend should start at: y=510 or below
SVG viewBox height: at least 560 to fit legend
```

**Wrong:** Legend at y=470 inside a cluster boundary that ends at y=490
**Right:** Legend at y=510, below the cluster boundary, with viewBox height extended

### Layout Structure

1. **Header** - Title with pulsing dot indicator, subtitle
2. **Main SVG diagram** - Contained in rounded border card
3. **Summary cards** - Grid of 3 cards below diagram with key details
4. **Footer** - Minimal metadata line

### Component Box Pattern

```svg
<rect x="X" y="Y" width="W" height="H" rx="6" fill="FILL_COLOR" stroke="STROKE_COLOR" stroke-width="1.5"/>
<text x="CENTER_X" y="Y+20" fill="white" font-size="11" font-weight="600" text-anchor="middle">LABEL</text>
<text x="CENTER_X" y="Y+36" fill="#94a3b8" font-size="9" text-anchor="middle">sublabel</text>
```

### Info Card Pattern

```html
<div class="card">
  <div class="card-header">
    <div class="card-dot COLOR"></div>
    <h3>Title</h3>
  </div>
  <ul>
    <li>• Item one</li>
    <li>• Item two</li>
  </ul>
</div>
```

---

## Arrow Reference

### Arrow z-order

Draw connection arrows **early in the SVG** (right after the background grid) so they render behind component boxes. SVG elements are painted in document order.

**Render order:**
1. Background grid
2. Region boundaries (dashed boxes)
3. Security group boundaries
4. **All arrows / connections** (drawn first, behind everything)
5. Component boxes (opaque background + styled rect, drawn last on top)
6. Text labels
7. Legend

### Arrow Marker

```svg
<marker id="arrowhead" markerWidth="10" markerHeight="7" refX="9" refY="3.5" orient="auto">
  <polygon points="0 0, 10 3.5, 0 7" fill="#64748b" />
</marker>
```

### Arrow Types

#### 1. Standard Arrow (data flow)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>
<text x="LABEL_X" y="LABEL_Y" fill="#94a3b8" font-size="9" text-anchor="middle">HTTPS</text>
```
Use the coordinate formulas from Step 2 above.

#### 2. Bidirectional Arrow (request/response, sync)
```svg
<!-- Main direction -->
<line x1="START_X" y1="Y-6" x2="END_X" y2="Y-6"
      stroke="#34d399" stroke-width="1.5" marker-end="url(#arrowhead)"/>
<!-- Return direction -->
<line x1="END_X" y1="Y+6" x2="START_X" y2="Y+6"
      stroke="#94a3b8" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```
Two parallel lines, offset by 6px vertically. Top line has primary color, bottom line has neutral color.

#### 3. Dashed Arrow (auth/security flow)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#fb7185" stroke-width="1.5" stroke-dasharray="5,5" marker-end="url(#arrowhead)"/>
<text x="LABEL_X" y="LABEL_Y" fill="#fb7185" font-size="8" text-anchor="middle">JWT</text>
```

#### 4. Async/Event Arrow (message queue, pub/sub)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#fb923c" stroke-width="1.5" stroke-dasharray="2,4" marker-end="url(#arrowhead)"/>
<text x="LABEL_X" y="LABEL_Y" fill="#fb923c" font-size="8" text-anchor="middle">async</text>
```
Uses `dasharray="2,4"` (short dashes) to distinguish from auth flows.

#### 5. Numbered Step Arrow (sequential flow)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#22d3ee" stroke-width="1.5" marker-end="url(#arrowhead)"/>
<circle cx="LABEL_X" cy="LABEL_Y-2" r="8" fill="#020617" stroke="#22d3ee" stroke-width="1"/>
<text x="LABEL_X" y="LABEL_Y+1" fill="#22d3ee" font-size="8" text-anchor="middle" font-weight="600">1</text>
```
Number inside a small circle. Use for step-by-step flows (1, 2, 3...).

#### 6. L-Shaped Orthogonal Arrow
Use when a direct line would pass through another component.

```svg
<!-- Source at right edge, target below and to the right -->
<path d="M START_X START_Y
         L START_X MID_Y
         L END_X MID_Y
         L END_X END_Y"
      fill="none" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```
Where:
- `MID_Y = START_Y + (END_Y - START_Y) / 2`  (midpoint vertically)
- The path goes: right → down → right → down (with arrowhead at the very end)

For L-shaped with one turn (source above, target offset horizontally):
```svg
<path d="M CENTER_X START_Y
         L CENTER_X TURN_Y
         L END_X TURN_Y
         L END_X END_Y"
      fill="none" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```
Where `TURN_Y = START_Y + 30` (30px down from source bottom, then horizontal, then down to target).

#### 7. U-Shaped Orthogonal Arrow (loopback, return flow)
Use when the target is above the source or to route around obstacles.

```svg
<path d="M START_X START_Y
         L START_X EXTEND_Y
         L SIDE_X EXTEND_Y
         L SIDE_X TARGET_Y
         L END_X TARGET_Y"
      fill="none" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```
Where:
- `EXTEND_Y = max(START_Y, TARGET_Y) + 40` (go 40px below both components)
- `SIDE_X` is the horizontal bypass position (either left of both or right of both)

### Arrow Label Positioning for Orthogonal Arrows

For L-shaped or U-shaped paths, place the label at the **corner point** or at the **midpoint of the longest segment**:

```svg
<path d="..." id="route1"/>
<text x="CORNER_X + 10" y="CORNER_Y - 6" fill="#94a3b8" font-size="8">label</text>
```

Offset 10px right and 6px above the corner to avoid overlapping the line.

---

## Dynamic viewBox Calculation

Calculate viewBox dimensions based on your layout:

```
viewBox_width  = rightmost_component_x + rightmost_component_width + 60
viewBox_height = lowest_element_y + lowest_element_height + 40
```

Add extra padding for:
- Region boundaries: add 20px margin on all sides
- Legend: add 120px below the lowest component
- Title area inside SVG: add 30px at the top

Minimum viewBox: `0 0 1000 680`. Expand as needed.

Round dimensions UP to the nearest multiple of 20 for clean alignment.

---

## Template

Copy and customize the template at `assets/template.html`. Key customization points:

1. Update the `<title>` and header text
2. Calculate viewBox dimensions using the rules above
3. Write the ASCII layout plan as an HTML comment in the SVG
4. Add component boxes using grid-aligned coordinates
5. Draw connection arrows using the coordinate formulas
6. Update the three summary cards
7. Update footer metadata

---

## Pre-Output Checklist

**BEFORE returning the HTML file, verify each item:**

- [ ] **No overlapping components**: Check that every component's bounding box (x, y, W, H) is unique and doesn't intersect with any other component
- [ ] **Arrows connect to correct edges**: Each arrow starts at the source component's boundary (not its center) and ends 2px before the target component's boundary
- [ ] **No arrow passes through a component**: Visually trace each arrow path — it should not cross through any component box that is neither its source nor its target
- [ ] **All coordinates on 20px grid**: Every component x and y is divisible by 20; every width and height is divisible by 10
- [ ] **Legend outside all boundaries**: The legend's top-left y position is greater than the y+height of every region boundary, security group, and cluster box
- [ ] **Arrows rendered behind components**: All `<line>` and `<path>` elements for arrows appear in the SVG before all `<rect>` elements for component boxes
- [ ] **Label positions readable**: Arrow labels don't overlap any component box or other label

If ANY item fails, fix it before outputting.

---

## Output

Always produce a single self-contained `.html` file with:
- Embedded CSS (no external stylesheets except Google Fonts)
- Inline SVG (no external images)
- No JavaScript required (pure CSS animations)

The file should render correctly when opened directly in any modern browser.
