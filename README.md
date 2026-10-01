# FlowScript

**Simple, intuitive offline flowchart diagramming tool inspired by Eraser.io**

FlowScript is a browser-based flowchart editor that uses a clean text syntax to create beautiful diagrams. Work entirely offline with local file system access, automatic version history, and real-time rendering powered by ELK (Eclipse Layout Kernel).

Live at [fs.gnkz.net](https://fs.gnkz.net) | GitHub Pages: [gunkaragoz.github.io/flowscript](https://gunkaragoz.github.io/flowscript/)

---

## Features

- **Text-based syntax** - Write flowcharts like code, see results instantly
- **Drag & drop** - Drag nodes and groups on the canvas; positions persist in the file as `pos: x,y`
- **5000+ icons** - Full [Tabler Icons](https://tabler-icons.io) library loaded from CDN
- **Dual layout engine** - Smart layout for standard and re-entrant (cross-group) flows
- **Sequence diagrams** - Actors, messages, control-flow blocks and activation bars
- **Org charts** - Indentation-based reporting lines, roles, draggable subtrees
- **Works fully offline** - The layout engine is embedded; download the single HTML file and run it anywhere
- **Local file system** - Direct folder access, your files stay on your machine
- **Version history** - Auto-saves every 5 minutes, manual snapshots with descriptions
- **Customizable** - 11 shapes, 20+ colors, text modes, and more
- **Dark/Light themes** - Comfortable viewing in any environment
- **Export** - Download diagrams as SVG or PNG
- **Auto-layout** - Powered by ELK for professional diagram layouts
- **Eraser.io compatible** - Supports familiar syntax conventions

---

## Quick Start

1. Open [FlowScript](https://fs.gnkz.net)
2. Click **"Open Folder"** to select a local directory
3. Create a new `.flow` file
4. Start writing your flowchart using the syntax below
5. Watch it render in real-time!

### Simple Example

```
Start [shape: oval]
Process Data
Check Valid? [shape: diamond]
Save to DB [shape: cylinder]
End [shape: oval]

Start > Process Data
Process Data > Check Valid?
Check Valid? > Save to DB : yes
Check Valid? > End : no
Save to DB > End
```

---

## Sequence Diagrams

Add `type sequence` at the top of a `.flow` file (FlowScript also infers it from blocks or `activate`):

```
type sequence

User [icon: user, color: gray]
Frontend [icon: layout, color: blue]
Backend [icon: server, color: purple]
DB [icon: database, color: green]

User > Frontend: Sign in
Frontend > Backend: POST /login
Backend > DB: Verify credentials

alt [label: valid] {
  Backend > DB: Create session
  Backend > Frontend: Access token
}
else [label: invalid] {
  Backend > Frontend: 401 Unauthorized
}

activate Backend
Backend > Backend: Refresh tokens
deactivate Backend
```

**Elements:**

| Element | Syntax | Description |
|---------|--------|-------------|
| Actor | `Name [icon: user, color: blue]` | A column; order of first appearance sets column order |
| Message | `A > B: text` | Arrow between two actors; the same arrows as flowcharts work (`>`, `<`, `<>`, `-`, `--`, `-->`) |
| Self message | `A > A: text` | Loop back to the same actor |
| Block | `loop`, `opt`, `alt`, `par`, `break` with `{ }` | Control-flow grouping, optional `[label: ...]` |
| Block section | `else`, `and` | Connected branches of `alt` / `par` |
| Activation | `activate A` / `deactivate A` | Bar on the actor's lifeline while it is active |

Actors accept the usual `label`, `color`, `icon` and `shape` properties, and can be dragged — messages, lifelines, blocks and activation bars follow live.

---

## Org Charts

Add `type org` (optionally followed by a chart title) at the top of a `.flow` file. **Indentation is the reporting line** — the convention every org tool shares:

```
type org Acme Corp

Alice Zhang [title: CEO, icon: user]
  Bob Stone [title: VP Engineering]
    Carol Diaz [title: Engineering Manager]
      Dan Ray [title: Senior Engineer]
      Eve Park [title: Engineer]
  Grace Lee [title: VP Sales]
    Heidi Roy [title: Account Executive]
Ivan Petrov [title: Advisor, color: gray]
```

- Each card shows a **name** plus an optional `title:` (alias `role:`) beneath it
- `color:`, `icon:`, `label:` and `pos:` work like everywhere else
- Explicit reporting lines are also accepted: `Alice Zhang > Bob Stone`
- Standard org-chart rules apply: **single parent** (the first one wins), multiple roots allowed, cycles ignored
- Layout is a tidy top-down tree — leaves spread left to right and every manager is centred over their reports
- **Dragging a manager moves their whole subtree**, and the reporting line from above re-routes to follow

---

## Syntax Guide

### Node Definitions

Define nodes with properties in square brackets:

```
Node Name [shape: rectangle, color: blue, icon: server, text: wrap]
```

**Supported Properties:**

| Property | Values | Description |
|----------|--------|-------------|
| `shape` | See shapes table below | Node shape |
| `color` | Named colors or hex codes | Node fill color |
| `icon` | Any [Tabler icon](https://tabler-icons.io) name | Icon to display in node |
| `text` | `wrap`, `fit`, `clip` | Text rendering mode |
| `label` | Any string | Custom label (different from node ID) |
| `pos` | `x,y` (e.g. `pos: 120,80`) | Manual position override (written by dragging) |

**Available Shapes:**

| Shape | Syntax | Common Use |
|-------|--------|------------|
| Rectangle | `rectangle` (default) | General process |
| Oval/Stadium | `oval` | Start/End terminals |
| Diamond | `diamond` | Decision points |
| Cylinder | `cylinder` | Database operations |
| Parallelogram | `parallelogram` | Input/Output |
| Hexagon | `hexagon` | Preparation steps |
| Trapezoid | `trapezoid` | Manual operations |
| Triangle | `triangle` | Extract/Collapse |
| Document | `document` | Document handling |
| Ellipse | `ellipse` | Connectors |
| Star | `star` | Special markers |

**Color Palette:**

`red`, `green`, `blue`, `yellow`, `purple`, `orange`, `pink`, `cyan`, `gray`, `brown`, `black`, `white`

Extended: `light-red`, `light-green`, `light-blue`, `light-yellow`, `light-purple`, `light-orange`

Or use hex codes: `#3b82f6`

**Text Modes:**

```
Long Text Name              // wrap (default) - wraps to multiple lines
Short [text: fit]           // fit - shrinks font to fit in one line
Very Long Name [text: clip] // clip - truncates with ellipsis
```

**Icons (5000+ Tabler Icons):**

Use any icon name from the [Tabler Icons](https://tabler-icons.io) library:

```
Server [icon: server]
GitHub [icon: brand-github]
AWS Lambda [icon: brand-aws]
Rocket [icon: rocket]
Fingerprint [icon: fingerprint]
Camera [icon: camera]
```

**Filled variants:** Append `-filled` to any icon name to use its filled version:

```
Heart [icon: heart-filled]
Star [icon: star-filled]
Bookmark [icon: bookmark-filled]
Bell [icon: bell-filled]
```

Common icons: `user`, `database`, `server`, `api`, `code`, `lock`, `mail`, `cloud`, `docker`, `check`, `x`, `alert`, `download`, `upload`, `globe`, `star`, `heart`, `settings`

Browse all icons at [tabler-icons.io](https://tabler-icons.io).

---

### Connections

Connect nodes with various arrow types:

```
A > B              // Solid arrow
A > B : label      // Labeled arrow
A > B > C          // Chained connections
A > B, C, D        // Branching (one to many)

A - B              // Line (no arrow)
A -- B             // Dotted line
A --> B            // Dotted arrow
A <> B             // Bidirectional
A < B              // Reverse arrow
```

---

### Groups

Create visual groupings with curly braces:

```
Group Name [color: blue] {
  Node A
  Node B [icon: api]
  Node C
}

// Connect groups
Group Name > Another Group : API calls
```

Groups support the same `color` property as nodes.

**Cross-group flows:** FlowScript automatically handles flows that alternate between groups (e.g., orchestrator calling external services and receiving results back). Satellite groups are positioned beside the primary group, aligned with their connection points.

---

### Dragging (Manual Positions)

The canvas is interactive: drag any **node** to move it, or drag a **group** (its border or label) to move the whole group.

```
Step1 [pos: 120,80]        // node pinned at x:120, y:80
Backend [color: blue, pos: 340,40] { ... }   // group pinned
```

- Dragging writes a `pos: x,y` property into the `.flow` text, so positions live in the file itself — they survive save, share, git, and version history
- Positions snap to a 10px grid for tidy, readable `pos` values
- Edges re-route **live while dragging** and **bend around intervening nodes** instead of cutting through them
- Only dragged elements are pinned; everything else keeps auto-arranging (hybrid layout)
- Groups automatically grow to keep containing their dragged members
- Double-click a dragged node or group to release it back to full auto-layout
- When any manual position exists, edges re-route orthogonally between the final node positions

---

### Direction

Control the flow direction:

```
direction down    // default (top-to-bottom)
direction up      // bottom-to-top
direction right   // left-to-right
direction left    // right-to-left
direction top     // alias for down
direction bottom  // alias for up
```

---

### Complete Example

```
// === User Authentication Flow ===

// Define nodes
Start [shape: oval, color: green, icon: globe]
Enter Credentials [shape: parallelogram, icon: user]
Validate [shape: diamond]
Check Database [shape: cylinder, icon: database]
Generate Token [color: blue, icon: lock]
Show Error [color: red, icon: x]
Success [shape: oval, color: green, icon: check]

// Define connections
Start > Enter Credentials
Enter Credentials > Validate
Validate > Show Error : invalid
Validate > Check Database : valid
Check Database > Show Error : not found
Check Database > Generate Token : found
Generate Token > Success
Show Error > Enter Credentials

// Group backend services
Backend Services [color: purple] {
  Check Database
  Generate Token
}
```

### Re-entrant Flow Example

```
// Pipeline that alternates between groups
direction top

Orchestrator [color: red] {
  Step1 [label: Start Job]
  Step2 [label: Process Result]
  Step3 [label: Next Job]
  Step4 [label: Final Result]
}

Worker [color: purple] {
  TaskA [label: Run Task A]
  TaskB [label: Run Task B]
}

Step1 > TaskA
TaskA > Step2
Step2 > Step3
Step3 > TaskB
TaskB > Step4
```

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+S` | Save version with description |
| `Ctrl+N` | Create new file |
| `Ctrl+O` | Open folder |

---

## Version Management

### Automatic Saves
FlowScript auto-saves your work every 5 minutes to prevent data loss.

### Manual Versions
- Press `Ctrl+S` to save a named version with a description
- Click "History" to view, compare, and restore previous versions
- Each version includes timestamp, description, and full diagram state

### Version Display
The app version is displayed in the header and updates automatically with each release.

---

## Development & Versioning

This project follows [Semantic Versioning](https://semver.org/) with automated version bumping:

### Automated Version Bumps

When you push to the `main` branch:

- **Patch version** (0.0.X) - Bug fixes
  - Use commit prefix: `fix:` or `bugfix:`
  - Example: `fix: correct PNG export clipping issue`

- **Minor version** (0.X.0) - New features
  - Use commit prefix: `feat:` or `feature:`
  - Example: `feat: add dark mode support`

- **Major version** (X.0.0) - Breaking changes
  - Manually update `version.json`

The GitHub Actions workflow automatically:
- Detects conventional commit prefixes
- Bumps the version number
- Updates `version.json` and the displayed version
- Creates a git tag
- Generates a GitHub release

---

## Best Practices

### 1. Node Naming
**Do:**
```
Validate Input [shape: diamond]
Send Email Notification
Save to Database [shape: cylinder]
```

**Don't:**
```
Check1 [shape: diamond]
Process
DB_OP_001
```

### 2. Decision Nodes
Always label both branches clearly:

```
Is Valid? [shape: diamond]

Is Valid? > Process Data : yes
Is Valid? > Show Error : no
```

Common label pairs: `yes/no`, `true/false`, `valid/invalid`, `success/failure`, `found/not found`

### 3. Color Coding Strategy

| Color | Use For |
|-------|---------|
| Green | Success, completion, positive outcomes |
| Red | Errors, failures, warnings |
| Yellow | Pending, waiting, caution |
| Blue | Information, processing, data flow |
| Purple | User actions, inputs |
| Orange | External services, async operations |

### 4. Flow Direction

- **Top-to-bottom** is standard (use `direction top` or `direction down`)
- Start nodes at the top (no incoming edges)
- End nodes at the bottom (no outgoing edges)
- Loops can flow upward when necessary

### 5. Groups Usage

Use groups for:
- Logical separation (Frontend / Backend / Database)
- Service boundaries (AWS / GCP / Local)
- Swim lanes (User / System / Admin)
- Architectural layers
- Cross-service pipelines (Orchestrator / Workers)

---

## File Format

FlowScript uses `.flow` files - plain text files that can be:
- Edited in any text editor
- Version controlled with Git
- Shared with team members
- Imported/exported easily

Example file structure:
```
my-project/
├── user-auth.flow
├── payment-flow.flow
├── data-pipeline.flow
└── error-handling.flow
```

---

## Browser Compatibility

FlowScript requires the File System Access API, currently supported in:
- Chrome/Edge 86+
- Safari 15.2+
- Opera 72+

Firefox support is planned pending File System Access API implementation.

### Offline / Single-File Use

The layout engine (ELK.js) is embedded directly in `index.html`, so the app works fully offline: download the single HTML file, open it locally, and everything functions without a network connection. Fonts and the extended icon library load from CDN as optional enhancements — without a network, system fonts and the built-in icon set are used instead.

---

## Contributing

This is a personal project by [Gun](https://gnkz.net), but feedback and suggestions are welcome!

- **GitHub:** [github.com/gunkaragoz/flowscript](https://github.com/gunkaragoz/flowscript)
- **Issues:** Report bugs or request features via GitHub Issues

---

## License

Personal project - feel free to use and learn from it.

---

## Acknowledgments

- Inspired by [Eraser.io](https://eraser.io)'s clean diagram syntax
- Layout powered by [ELK (Eclipse Layout Kernel)](https://www.eclipse.org/elk/)
- Icons from [Tabler Icons](https://tabler-icons.io) (5000+ MIT-licensed SVG icons)

---

**This is vibe-coded by [Gun](https://gnkz.net) with Claude Code**
