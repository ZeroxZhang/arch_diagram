---
name: architecture-diagram
description: Create professional, light-themed architecture diagrams as standalone HTML files with SVG graphics. Use when the user asks for system architecture diagrams, infrastructure diagrams, cloud architecture visualizations, security diagrams, network topology diagrams, or any technical diagram showing system components and their relationships.
license: MIT
metadata:
  version: "3.0"
  author: ZeroxZhang
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
- Identify components with multiple outgoing connections (bus-style routing needed)

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

#### Bus-style multi-connection routing (CRITICAL)

When a component has **multiple outgoing connections** from the same edge (e.g., API Server → DB and API Server → Redis), all connections MUST share a single exit point on the source component, then branch out. This prevents arrows from fanning out directly from the component edge.

**Rule:** All arrows from the same source edge share ONE common start point, then diverge via orthogonal paths.

```
Example: API Server (x=580, y=280, W=140, H=60) connects to:
  - DB (x=780, y=240, W=120, H=60)
  - Redis (x=780, y=340, W=120, H=60)

Common exit point: right edge center of API Server = (720, 310)
Branch point: x = 740 (20px to the right of the exit point)

Arrow to DB:   M 720,310 L 740,310 L 740,270 L 778,270
Arrow to Redis: M 720,310 L 740,310 L 740,370 L 778,370
```

The shared segment from the exit point to the branch point creates a clean "bus" trunk. Each branch then routes orthogonally to its target.

**For vertical bus-style (top/bottom edge):**
```
Common exit point: bottom edge center = (center_x, y + H)
Branch point: y = exit_y + 20 (20px below the exit point)

Arrow to Target1: M center_x,exit_y L center_x,branch_y L target1_center_x,branch_y L target1_center_x,target1_top
Arrow to Target2: M center_x,exit_y L center_x,branch_y L target2_center_x,branch_y L target2_center_x,target2_top
```

### Step 3: Generate SVG

Follow the design system below to generate the final HTML/SVG.

### Step 4: Review & Fix (REQUIRED)

**After generating the SVG, you MUST perform a systematic review before output.** Different AI models may produce incorrect connections, overlapping elements, or routing errors. This step catches and fixes those issues.

#### Review Checklist

Go through EACH item below. If ANY check fails, fix the SVG before outputting.

**A. Component Overlap Check**
- For every pair of components (A, B), verify their bounding boxes do NOT intersect:
  ```
  overlap = !(A.x + A.W <= B.x || B.x + B.W <= A.x || A.y + A.H <= B.y || B.y + B.H <= A.y)
  ```
  If `overlap` is true for any pair, adjust positions.

**B. Arrow Routing Check**
- For each arrow, trace its full path (including L-shaped or U-shaped segments) and verify:
  - The path does NOT pass through any component that is neither its source nor its target
  - The start point is on the source component's boundary (not inside it)
  - The end point is 2px before the target component's boundary (not inside it)
  - The arrow does not overlap with another arrow's path for more than 10px (unless they share a bus trunk)

**C. Bus-style Connection Check**
- For every component with 2+ outgoing connections from the same edge:
  - Verify all connections share a SINGLE exit point on the source component
  - Verify there is a common trunk segment before branching
  - Verify each branch routes cleanly to its target without crossing other branches

**D. Connection Correctness Check**
- Verify each arrow matches the intended connection from the layout plan
- Verify no arrow is missing (every edge in the plan has a corresponding SVG element)
- Verify no extra arrows exist that aren't in the plan
- Verify arrow directions are correct (source → target, not reversed)

**E. Legend Check**
- Verify the legend is centered horizontally within the SVG viewBox
- Verify the legend does not overlap any component or boundary
- Verify all component types used in the diagram appear in the legend

**F. Label Readability Check**
- Verify no two arrow labels overlap each other
- Verify no arrow label overlaps a component box
- Verify all text is legible (sufficient contrast against background)

If ANY check fails, modify the SVG to fix the issue, then re-run the failed check. Repeat until all checks pass.

---

## Design System

### Color Palette (Light Theme)

Use these semantic colors for component types:

| Component Type | Fill (rgba) | Stroke | Text |
|---------------|-------------|--------|------|
| Frontend | `rgba(236, 254, 255, 0.9)` | `#0891b2` (cyan-600) | `#155e75` |
| Backend | `rgba(236, 253, 245, 0.9)` | `#059669` (emerald-600) | `#065f46` |
| Database | `rgba(245, 243, 255, 0.9)` | `#7c3aed` (violet-600) | `#5b21b6` |
| Cache/Redis | `rgba(250, 245, 255, 0.9)` | `#9333ea` (purple-600) | `#6b21a8` |
| AWS/Cloud | `rgba(255, 251, 235, 0.9)` | `#d97706` (amber-600) | `#92400e` |
| Security | `rgba(255, 241, 242, 0.9)` | `#e11d48` (rose-600) | `#9f1239` |
| Message Bus | `rgba(255, 247, 237, 0.9)` | `#ea580c` (orange-600) | `#9a3412` |
| API Gateway | `rgba(236, 254, 255, 0.9)` | `#0891b2` (cyan-600) | `#155e75` |
| Container/K8s | `rgba(239, 246, 255, 0.9)` | `#2563eb` (blue-600) | `#1e40af` |
| External/Generic | `rgba(248, 250, 252, 0.9)` | `#64748b` (slate-500) | `#334155` |

### Language

**Default language: Chinese (中文).** All text in the generated diagram MUST use Chinese as the primary language, including:

- **Title**: Use Chinese (e.g., "系统架构图" instead of "Architecture")
- **Component labels**: Use Chinese names (e.g., "用户" instead of "Users", "API 服务器" instead of "API Server", "数据库" instead of "Database")
- **Sublabels**: Technical details like port numbers and technology names can remain in English (e.g., "FastAPI :8000", "PostgreSQL :5432")
- **Arrow labels**: Use Chinese (e.g., "请求" instead of "Request", "认证" instead of "Auth")
- **Legend**: Use Chinese (e.g., "图例", "前端", "后端", "数据库", "云服务", "安全")
- **Region/boundary labels**: Use Chinese (e.g., "AWS 区域: us-west-2")
- **Summary cards**: Use Chinese for titles and descriptions

Only use English when the user explicitly requests it or when the content is a proper noun / technical term that has no common Chinese equivalent.

### Typography

Use the following font stack for all text:
```css
font-family: 'SimHei', 'Microsoft YaHei', 'PingFang SC', 'JetBrains Mono', monospace;
```

SimHei (黑体) is the primary font for all Chinese text. JetBrains Mono is the fallback monospace font for technical labels and code-like content.

Font sizes: 12px for component names, 9px for sublabels, 8px for annotations, 7px for tiny labels.

**Title:** The diagram title MUST be centered horizontally, use a larger font size (20px), and use SimHei for Chinese characters. Do NOT include a pulsing dot indicator next to the title.

### Grid Alignment

**ALL component x, y coordinates MUST be aligned to a 20px grid.**

This means every x and y value must be divisible by 20 (e.g., 40, 60, 80, 100, 120...).

Width and height values must be divisible by 10.

This ensures:
- Components visually align in clean rows and columns
- Arrow calculations are predictable
- No accidental overlaps from misaligned positions

### Visual Elements

**Background:** `#ffffff` (white) with subtle grid pattern:
```svg
<pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
  <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#e2e8f0" stroke-width="0.5"/>
</pattern>
```

**Component boxes:** Rounded rectangles (`rx="6"`) with 1.5px stroke, semi-transparent light fills.

**Security groups:** Dashed stroke (`stroke-dasharray="4,4"`), transparent fill, rose color.

**Region boundaries:** Larger dashed stroke (`stroke-dasharray="8,4"`), amber color, `rx="12"`.

### Masking arrows behind fills

Since component boxes use semi-transparent fills, arrows behind them may show through. To fully mask arrows, draw an opaque background rect (e.g., `fill="#ffffff"`) at the same position before drawing the semi-transparent styled rect on top:
```svg
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#ffffff"/>
<rect x="X" y="Y" width="W" height="H" rx="6" fill="rgba(236, 253, 245, 0.9)" stroke="#059669" stroke-width="1.5"/>
```

**Auth/security flows:** Dashed lines in rose color (`#e11d48`).

**Message buses / Event buses:** Small connector elements between services. Use orange color (`#ea580c` stroke, `rgba(255, 247, 237, 0.9)` fill):
```svg
<rect x="X" y="Y" width="120" height="20" rx="4" fill="rgba(255, 247, 237, 0.9)" stroke="#ea580c" stroke-width="1"/>
<text x="CENTER_X" y="Y+14" fill="#9a3412" font-size="7" text-anchor="middle">Kafka / RabbitMQ</text>
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

**CRITICAL:** The legend MUST be centered horizontally within the SVG viewBox.

- Calculate the total width of the legend content (all items in a row or grid)
- Set the legend's starting x so that it is centered: `legend_x = (viewBox_width - legend_total_width) / 2`
- Place legend at least 20px below the lowest boundary or component
- Expand SVG viewBox height if needed to accommodate
- Wrap the legend in a subtle container box for visual grouping:

```svg
<rect x="LEGEND_X - 10" y="LEGEND_Y - 10" width="LEGEND_W + 20" height="LEGEND_H + 20" rx="8" fill="rgba(248, 250, 252, 0.8)" stroke="#cbd5e1" stroke-width="1"/>
```

**Example:**
```
viewBox_width = 960
Legend content width = 400
legend_x = (960 - 400) / 2 = 280
```

**Wrong:** Legend positioned at x=740, aligned to the right edge
**Right:** Legend centered at x=280, symmetrically placed within the viewBox

### Layout Structure

1. **Header** - Centered title (no pulsing dot), subtitle
2. **Main SVG diagram** - Contained in rounded border card
3. **Summary cards** - Grid of 3 cards below diagram with key details
4. **Footer** - Minimal metadata line

### Component Box Pattern

```svg
<rect x="X" y="Y" width="W" height="H" rx="6" fill="#ffffff"/>
<rect x="X" y="Y" width="W" height="H" rx="6" fill="FILL_COLOR" stroke="STROKE_COLOR" stroke-width="1.5"/>
<text x="CENTER_X" y="Y+20" fill="TEXT_COLOR" font-size="11" font-weight="600" text-anchor="middle">LABEL</text>
<text x="CENTER_X" y="Y+36" fill="#64748b" font-size="9" text-anchor="middle">sublabel</text>
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
<text x="LABEL_X" y="LABEL_Y" fill="#64748b" font-size="9" text-anchor="middle">HTTPS</text>
```
Use the coordinate formulas from Step 2 above.

#### 2. Bidirectional Arrow (request/response, sync)
```svg
<!-- Main direction -->
<line x1="START_X" y1="Y-6" x2="END_X" y2="Y-6"
      stroke="#059669" stroke-width="1.5" marker-end="url(#arrowhead)"/>
<!-- Return direction -->
<line x1="END_X" y1="Y+6" x2="START_X" y2="Y+6"
      stroke="#94a3b8" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```
Two parallel lines, offset by 6px vertically. Top line has primary color, bottom line has neutral color.

#### 3. Dashed Arrow (auth/security flow)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#e11d48" stroke-width="1.5" stroke-dasharray="5,5" marker-end="url(#arrowhead)"/>
<text x="LABEL_X" y="LABEL_Y" fill="#e11d48" font-size="8" text-anchor="middle">JWT</text>
```

#### 4. Async/Event Arrow (message queue, pub/sub)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#ea580c" stroke-width="1.5" stroke-dasharray="2,4" marker-end="url(#arrowhead)"/>
<text x="LABEL_X" y="LABEL_Y" fill="#ea580c" font-size="8" text-anchor="middle">async</text>
```
Uses `dasharray="2,4"` (short dashes) to distinguish from auth flows.

#### 5. Numbered Step Arrow (sequential flow)
```svg
<line x1="START_X" y1="Y" x2="END_X" y2="Y"
      stroke="#0891b2" stroke-width="1.5" marker-end="url(#arrowhead)"/>
<circle cx="LABEL_X" cy="LABEL_Y-2" r="8" fill="#ffffff" stroke="#0891b2" stroke-width="1"/>
<text x="LABEL_X" y="LABEL_Y+1" fill="#0891b2" font-size="8" text-anchor="middle" font-weight="600">1</text>
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

#### 8. Bus-style Multi-connection Arrow (CRITICAL)
Use when a component connects to **multiple targets** from the same edge. All connections share a single exit point, then branch via orthogonal paths.

```svg
<!-- Source: API Server right edge center = (720, 310) -->
<!-- Branch point: x=740 (20px right of exit) -->

<!-- Shared trunk + branch to DB -->
<path d="M 720,310 L 740,310 L 740,270 L 778,270"
      fill="none" stroke="#64748b" stroke-width="1.5" marker-end="url(#arrowhead)"/>

<!-- Shared trunk + branch to Redis -->
<path d="M 720,310 L 740,310 L 740,370 L 778,370"
      fill="none" stroke="#7c3aed" stroke-width="1.5" marker-end="url(#arrowhead)"/>
```

**Key rules:**
- The exit point is always at the center of the source component's edge
- The branch point is 20px away from the exit point (outside the component)
- Each branch routes orthogonally from the branch point to its target
- The shared trunk segment (exit → branch point) creates a clean visual "bus"
- Do NOT draw separate arrows fanning out from different points on the source edge

### Arrow Label Positioning for Orthogonal Arrows

For L-shaped or U-shaped paths, place the label at the **corner point** or at the **midpoint of the longest segment**:

```svg
<path d="..." id="route1"/>
<text x="CORNER_X + 10" y="CORNER_Y - 6" fill="#64748b" font-size="8">label</text>
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
5. Draw connection arrows using the coordinate formulas (bus-style for multi-connections)
6. Center the legend horizontally within the viewBox
7. Update the three summary cards
8. Update footer metadata

---

## Pre-Output Checklist

**BEFORE returning the HTML file, verify each item:**

- [ ] **No overlapping components**: Check that every component's bounding box (x, y, W, H) is unique and doesn't intersect with any other component
- [ ] **Arrows connect to correct edges**: Each arrow starts at the source component's boundary (not its center) and ends 2px before the target component's boundary
- [ ] **No arrow passes through a component**: Visually trace each arrow path — it should not cross through any component box that is neither its source nor its target
- [ ] **All coordinates on 20px grid**: Every component x and y is divisible by 20; every width and height is divisible by 10
- [ ] **Legend centered**: The legend module is horizontally centered within the SVG viewBox
- [ ] **Legend outside all boundaries**: The legend's top-left y position is greater than the y+height of every region boundary, security group, and cluster box
- [ ] **Arrows rendered behind components**: All `<line>` and `<path>` elements for arrows appear in the SVG before all `<rect>` elements for component boxes
- [ ] **Label positions readable**: Arrow labels don't overlap any component box or other label
- [ ] **Bus-style connections**: Components with multiple outgoing connections from the same edge share a single exit point and common trunk before branching
- [ ] **Title centered**: The diagram title is centered horizontally without a pulsing dot
- [ ] **Light theme colors**: All colors follow the light theme palette (white background, light fills, dark strokes and text)
- [ ] **Chinese language**: All labels, titles, legends, and arrow annotations use Chinese as the primary language (except technical terms and port numbers)

If ANY item fails, fix it before outputting.

---

## Output

Always produce a single self-contained `.html` file with:
- Embedded CSS (no external stylesheets except Google Fonts)
- Inline SVG (no external images)
- No JavaScript required (pure CSS animations)

The file should render correctly when opened directly in any modern browser.
