# Copilot Instructions for shape-maker

## Repository Overview

**shape-maker** is a single-page web application for creating polygons interactively on HTML5 canvas, with PNG/SVG/JSON export capabilities. Written entirely in vanilla JavaScript with embedded HTML/CSS in one file.

### Key Stats
- **Type**: Static web app (single HTML file, 630 lines)
- **Languages**: HTML, CSS, JavaScript (all in index.html)
- **Dependencies**: None (no build tools, no npm packages)
- **Runtime**: Modern web browsers

### Features
- Interactive polygon creation/editing via mouse (drag points, double-click to remove)
- Export as PNG, SVG, or JSON
- Auto-generated shape names with filename sanitization

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Pages deployment workflow
├── index.html                   # Complete application (HTML/CSS/JS)
└── README.md                    # Project description
```

### File Details

**index.html** (630 lines) - Complete application with embedded CSS (lines 7-159) and JavaScript (lines 194-628). Main class: `PolygonCreator`.

**README.md** - Project description: "Create a polygon then export image and json data file of points"

**.github/workflows/deploy.yml** - GitHub Pages auto-deployment. Triggers on push to main. No build step; deploys repository root as-is.

## Build and Deployment Instructions

### Important: No Build Step Required

This is a static web application with **no build, compilation, or dependency installation**. Works directly in browsers.

### Local Testing

1. **Start HTTP server** (required for browser security): `python3 -m http.server 8000`
2. **Open browser**: http://localhost:8000/
3. **Validate HTML** (optional): `python3 -c "from html.parser import HTMLParser; parser = HTMLParser(); parser.feed(open('index.html').read()); print('Valid')"`

### Deployment

**GitHub Pages** (automatic on push to `main`)
- Workflow: `.github/workflows/deploy.yml`
- Duration: ~20-30 seconds
- Process: Repository root uploaded as-is (no build)
- Recent runs: All successful

### Common Issues

1. **Local testing fails**: Use HTTP server, not file:// URLs (browser security blocks canvas operations)
2. **Cached changes**: Hard refresh browser (Ctrl+Shift+R)

## Continuous Integration

### GitHub Actions

**File**: `.github/workflows/deploy.yml` ("Deploy to GitHub Pages")
**Triggers**: Push to `main` or manual dispatch
**Steps**: checkout@v4 → configure-pages@v4 → upload-pages-artifact@v3 → deploy-pages@v4
**Duration**: ~20-30 seconds
**Status**: All recent runs successful (no build failures possible - no build step)
**Deployed site**: https://johnbradley.github.io/shape-maker/

## Code Architecture

### Main Application Flow

1. **Initialization** (line 627): `new PolygonCreator('canvas')` - sets up canvas and event listeners

2. **PolygonCreator Class** (lines 219-624):
   - **Properties**: `points[]` (coordinates), `draggingPoint`, `hoveredPoint`, `minPolygonPoints: 3`
   - **Key Methods**:
     - Mouse handlers: `handleMouseDown/Move/DoubleClick()` 
     - `draw()`: Render polygon and control points
     - Export methods: `exportAsImage/Svg/Json()` - PNG, SVG, JSON exports
     - `getShapeName()`: Sanitize filenames (removes `/\:*?"<>|%` etc.)
     - `getBoundingBox()`: Calculate polygon bounds for cropping

3. **Shape Names** (lines 195-217): `generateShapeName()` creates random names like "Cosmic-Polygon"

### Critical Code Sections

**Drawing** (lines 390-419): Canvas rendering, purple gradient colors (#667eea, #764ba2)
**Exports**: PNG (lines 478-534, cropped), SVG (lines 536-581, translated coords), JSON (lines 583-611, 2 decimals)
**Security** (lines 435-456): Filename sanitization removes `/\:*?"<>|%`, control chars, leading/trailing dots

## Making Code Changes

### Testing Changes

1. Edit index.html
2. Refresh browser (Ctrl+Shift+R)
3. **Manual testing required** (no automated tests):
   - Add/drag/remove points
   - Test all exports (PNG, SVG, JSON)
   - Test edge cases (0-2 points, special chars in name)

### Code Style

- Indentation: 4 spaces; camelCase variables, PascalCase classes; single quotes

### Common Modifications

**New export format**: Add button (line ~182), listener (line ~251), implement method (follow export pattern), use `getShapeName()`, disable if `points.length < 3`
**Appearance**: Fill (line 372), stroke (376-377), point colors (411-412)
**Behavior**: Point radius (226), hover radius (227), min points (228)

## Key Facts for AI Agents

### What to Trust

✅ **Trust these instructions** - they are based on thorough analysis of the codebase, successful workflow runs, and manual testing.

✅ **The application works as-is** - no build step, no dependencies, no setup required beyond a local HTTP server.

✅ **GitHub Actions workflow is reliable** - all recent deployments succeeded; the workflow is simple and stable.

### What to Avoid

❌ **Do not add build tools** (webpack, npm, etc.) unless explicitly requested - this is intentionally a simple, dependency-free project.

❌ **Do not add package.json** unless adding actual dependencies - the project has zero dependencies by design.

❌ **Do not create test files** unless explicitly asked - there is no testing infrastructure and manual testing is the established pattern.

❌ **Do not assume standard web project structure** - this is a single-file application, not a multi-file project.

### When to Search Further

Only search the codebase if:
- These instructions contradict what you observe in the actual code
- You need to understand implementation details of specific functions
- The user requests features that may already exist but aren't documented here

### Quick Reference

**To make changes**:
1. Edit index.html only (no other files needed)
2. Test locally with `python3 -m http.server 8000`
3. Commit changes
4. GitHub Actions will auto-deploy to Pages on merge to main

**File locations**:
- Application code: `/index.html` (all code in one file)
- Deployment config: `/.github/workflows/deploy.yml`
- Documentation: `/README.md`

**No files for**:
- Build configuration (none exists)
- Dependencies (none exist)
- Tests (none exist)
- Linting configuration (none exists)

**Key constraints**:
- Keep everything in index.html (single-file architecture)
- No external dependencies (vanilla JS only)
- Manual testing required (no test automation)
- Browser compatibility: Modern browsers only (ES6+ features used)
