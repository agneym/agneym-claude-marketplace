---
description: Analyze and visualize architecture of a codebase area
argument-hint: [directory-or-area]
allowed-tools: *
---

# Architecture Analysis

Analyze the architecture of: **$ARGUMENTS**

## Your Task

### Setup: Create Visualization Submodule

Before starting the analysis, create a git submodule for storing all visualization outputs:

1. Create a directory called `visualizations/` in the user's repository root
2. Initialize it as a git submodule (if it doesn't exist already)
3. All outputs (HTML files, images, assets) will be stored in this submodule
4. The HTML file will be the main entry point for viewing the visualization

### Workflow

1. **Explore** the codebase area using Glob and Read to understand:
   - Key components and their responsibilities
   - Data flow and interactions
   - Dependencies and relationships

2. **Explain** the architecture concisely:
   - Main components and their roles
   - How they interact
   - Key patterns or design decisions

3. **Visualize** - You have complete freedom to choose the best visualization approach:
   - **HTML + JavaScript**: Create interactive visualizations using any JavaScript libraries (D3.js, Chart.js, Plotly, Three.js, etc.)
   - **Generated Images**: Use gemini-imagen skill scripts to generate diagrams
   - **Hybrid**: Combine both - generate images and embed them in an interactive HTML page

   **CRITICAL - Image Handling Rule:**
   - If you generate images (using gemini-imagen scripts), save them in the `visualizations/` submodule
   - Reference these images in an HTML file (also in the submodule)
   - Images should NOT be standalone - always create an HTML file that displays them
   - The HTML file serves as the entry point for viewing all visualizations

## Critical Visualization Guidelines

### For Image Generation (using gemini-imagen)

**NEVER be vague in image prompts. The image model cannot see the codebase.**

1. **Specify exact positions**: "Component A at top center, Component B at bottom left" (not "components arranged logically")
2. **Label every connection**: "Arrow from A to B labeled 'POST /api/login with JWT'" (not "A calls B")
3. **Include all details**: Methods, parameters, return types, HTTP verbs, data formats
4. **Use specific colors**: "Requests in blue, responses in green, errors in red" (not "color-coded")
5. **State cardinality**: "1 to N (many)" on relationship lines (not "has many")
6. **Complete flows**: List every step sequentially with explicit labels

### For HTML/Interactive Visualizations

You have complete freedom to create any type of interactive visualization. Consider:

**JavaScript Libraries** (load via CDN):
- **D3.js**: Complex data visualizations, force-directed graphs, hierarchies
- **Chart.js**: Simple charts (bar, line, pie, radar)
- **Plotly**: Interactive scientific/statistical charts
- **Three.js**: 3D visualizations
- **Mermaid.js**: Diagrams from text descriptions (flowcharts, sequence diagrams, etc.)
- **Cytoscape.js**: Network/graph visualizations
- **Vis.js**: Timeline, network, and graph visualizations
- Or any other library you find appropriate

**Visualization Types:**
- Interactive architecture diagrams with clickable components
- Animated data flow visualizations
- Filterable/searchable dependency graphs
- Timeline views of execution flows
- Interactive code maps with zoom/pan
- Combined visualizations (images + interactive overlays)

**File Structure:**
- Create `visualizations/index.html` as the main entry point
- Can use multiple HTML files if needed
- External CSS/JS files are allowed
- Reference any generated images with relative paths

**Best Practices:**
- Use multiple files with external references when appropriate
- Include clear navigation if creating multiple pages
- Add interactivity where it enhances understanding (hover tooltips, click to expand, etc.)
- Keep it simple - this is throwaway code, don't over-engineer

## Diagram Templates (for Image Generation)

Choose the appropriate template based on what you discovered:

### Architecture Diagram
```
"Technical architecture diagram: [COMPONENT_1] at top center, [COMPONENT_2] on left middle, [COMPONENT_3] on right middle.
Arrow from [COMPONENT_1] to [COMPONENT_2] labeled '[HTTP_METHOD] [PATH] [PURPOSE]'.
Arrow from [COMPONENT_2] to [COMPONENT_3] labeled '[PROTOCOL] [DATA_TYPE]'.
[Repeat for ALL connections with explicit labels].
Clean labeled boxes, directional arrows, white background."
```

### Data Flow Diagram
```
"Data flow diagram: Step 1: [ENTITY_A] at left. Step 2: Arrow to [ENTITY_B] labeled '[METHOD] [PATH] with [DATA]'.
Step 3: Arrow back labeled '[STATUS] [RESPONSE_TYPE]'. [Continue for all steps].
Number each step, color-code: [TYPE_1] in blue, [TYPE_2] in green. Technical style, 16:9."
```

### Component Relationships (UML)
```
"UML class diagram: [CLASS_1] box at top with attributes '[ATTRS]' and methods '[METHODS]'.
[CLASS_2] box at bottom with '[ATTRS/METHODS]'.
[CLASS_1] to [CLASS_2]: [RELATIONSHIP] shown with [ARROW_TYPE], labeled '1 to N'.
[Repeat for all relationships]. Clean UML style."
```

### Code Execution Flow
```
"Flowchart for [FUNCTION]: Start. Step 1: '[ACTION]' in blue rectangle.
Step 2: Diamond '[CONDITION]' with YES arrow to [NEXT] and NO arrow to [ALT].
[Continue all steps]. Errors in red rounded boxes, success in green. Label all arrows."
```

### Database Schema
```
"Database schema: [TABLE_1] with columns '[COL] [TYPE] [CONSTRAINTS]'.
[TABLE_2] with '[COLUMNS]'. Foreign key: [TABLE_2].[FK] → [TABLE_1].[PK]
shown with line labeled '1 to N'. [Repeat for all tables]. Show PK icons."
```

### API Endpoints
```
"REST API for [SERVICE]: Endpoint 1: [METHOD] [PATH] with body {[FIELDS]} returns {[RESPONSE]} [STATUS].
[Repeat for all endpoints]. Color-code: GET blue, POST green, PUT yellow, DELETE red.
Show full JSON examples."
```

## Educational Approach

**Your goal: Create the best visualization for understanding, not just documentation.**

Consider creative formats when appropriate:
- **Metaphors**: Database transaction as restaurant order system
- **Comics**: Function execution as sequential panels
- **Real-world scenarios**: Authentication as bouncer checking IDs
- **Analogies**: Cache as kitchen pantry with frequently-used items

**Example creative prompt:**
> "Comic strip showing JWT auth: Panel 1: User (detective) at API Gateway (security desk). Panel 2: Gateway calls Auth Service (background check). Panel 3: Auth returns golden badge (JWT). Panel 4: User shows badge to Resource Server (VIP room). Cartoon style."

## Output

### Creating Visualizations

**Option 1: HTML + Interactive JavaScript**
- Create `visualizations/index.html` with your interactive visualization
- Use any JavaScript libraries via CDN
- Can create additional HTML/CSS/JS files as needed
- Reference any generated images with relative paths

**Option 2: Generated Images (via gemini-imagen)**
- Generate diagrams using: `scripts/generate_image.py "YOUR_EXPLICIT_PROMPT" visualizations/architecture-diagram.png --size 4K --aspect 16:9`
- The script has a python and uv shebang - execute it directly instead of using python
- Save all images in the `visualizations/` submodule
- Create `visualizations/index.html` that displays the images
- If complex, create multiple focused diagrams rather than one overwhelming image
- Use descriptive filenames: `component-relationships.png`, `data-flow.png`, etc.

**Option 3: Hybrid Approach**
- Generate images using gemini-imagen scripts (save to `visualizations/`)
- Create interactive HTML that embeds/references the images
- Add JavaScript interactivity on top (zoom, annotations, navigation, etc.)

### Viewing the Visualization

After creating the visualization, start a local server:

```bash
python -m http.server --directory visualizations/
```

Then open `http://localhost:8000` in a browser to view the visualization.
