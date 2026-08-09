# TailorFit Intro Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a responsive, Claude-template-inspired static landing page that introduces TailorFit and drives users to scan the time-limited experience QR code.

**Architecture:** Use one self-contained `index.html` with semantic sections, embedded CSS, and a small progressive-enhancement script for the current year and reduced-motion-friendly reveal states. Store the supplied QR image in a local `assets/` folder so the page opens offline without depending on the Downloads directory.

**Tech Stack:** HTML5, CSS3 custom properties/grid/flexbox, vanilla JavaScript, PowerShell static checks.

## Global Constraints

- Use the Claude research-journal aesthetic: warm ivory paper, near-black slate ink, restrained clay accent, hard edges, no shadows or glow.
- Use current TailorFit capabilities only: four-week plans, six builtin plans, local plan creation, check-ins, history, import/export, local-first storage, and DeepSeek disclosure.
- Responsive breakpoint: switch from desktop two-column/grid layouts to single-column layouts below `768px`.
- The page must open directly without a build tool, API, form submission, or user-data storage.
- QR image must remain at least `180px` wide on small screens, use `object-fit: contain`, and have descriptive alt text.
- All interactive controls need visible `:focus-visible` states and reduced-motion handling.

---

### Task 1: Add the local QR asset and page scaffold

**Files:**
- Create: `assets/tailorfit-experience-qr.png` (copy of the supplied experience QR image)
- Create: `index.html` (semantic page sections, content, and initial styles/scripts)

**Interfaces:**
- Consumes: `C:/Users/Rz/Downloads/TailorFit体验版（8月16日前有效）.png`
- Produces: a directly-openable page whose QR reference is `assets/tailorfit-experience-qr.png`.

- [ ] **Step 1: Copy the supplied QR image into the project**

Run:

```powershell
Copy-Item -LiteralPath 'C:/Users/Rz/Downloads/TailorFit体验版（8月16日前有效）.png' -Destination 'assets/tailorfit-experience-qr.png'
```

Expected: `assets/tailorfit-experience-qr.png` exists and has the same non-zero file size as the source.

- [ ] **Step 2: Create the semantic HTML scaffold and accurate copy**

Create `index.html` with these anchors and sections:

```html
<header id="top">...</header>
<main>
  <section id="intro">...</section>
  <section id="features">...</section>
  <section id="flow">...</section>
  <section id="safety">...</section>
  <section id="audience">...</section>
</main>
<footer id="experience">...</footer>
```

The page copy must name the six builtin plans, local creation, check-in fields, history/trend, JSON import/export, local API-key behavior, DeepSeek disclosure, and health disclaimer. The hero and footer must reference the QR image and say that the experience version is valid through August 16.

- [ ] **Step 3: Commit the scaffold and asset**

Run:

```powershell
git add -- index.html assets/tailorfit-experience-qr.png
git commit -m "feat: add TailorFit intro page scaffold"
```

Expected: one commit containing only the page scaffold and local QR asset.

### Task 2: Implement the Claude visual system and responsive behavior

**Files:**
- Modify: `index.html` (CSS custom properties, layout, states, and responsive rules)

**Interfaces:**
- Consumes: semantic anchors from Task 1.
- Produces: desktop and mobile layouts with keyboard-visible states, no horizontal overflow, and reduced-motion support.

- [ ] **Step 1: Define the design tokens and base typography**

Add CSS variables matching the approved direction:

```css
:root {
  --paper: #f4f0e8;
  --paper-deep: #e9e2d6;
  --ink: #171716;
  --slate: #2b2b28;
  --muted: #66645f;
  --line: #cfc8bb;
  --clay: #bd5b3e;
}
```

Use a system sans stack for UI/body copy and a serif fallback for feature headlines. Use `box-sizing: border-box`, no default shadows, `scroll-behavior: smooth`, visible focus outlines, and selection color from the clay token.

- [ ] **Step 2: Build the desktop editorial composition**

Implement a max-width `1180px` shell, sticky header, two-column hero, dark feature band, six-card feature grid, four-step process row, dark safety band, audience tags, and QR CTA footer. Keep cards square-edged and use borders, rules, labels, numbering, and typography for hierarchy.

- [ ] **Step 3: Add mobile behavior and interaction states**

At `max-width: 767px`, collapse columns and grids to one column, keep the QR card after the hero copy, allow header links to wrap or hide without overflow, and keep QR width at `min(100%, 220px)`. Add `@media (prefers-reduced-motion: reduce)` to remove transitions and reveal transforms.

- [ ] **Step 4: Commit visual implementation**

Run:

```powershell
git add -- index.html
git commit -m "feat: style TailorFit page with Claude editorial system"
```

Expected: the page is visually complete and responsive from a local file.

### Task 3: Add progressive enhancement and verify the page

**Files:**
- Modify: `index.html` (small script and final accessibility details)

**Interfaces:**
- Consumes: existing sections and CTA anchors.
- Produces: graceful reveal states, current year text, and a verified static artifact.

- [ ] **Step 1: Add a minimal progressive-enhancement script**

Use a script that only adds a `js` class, populates `[data-current-year]`, and reveals marked blocks through `IntersectionObserver` when available. When JavaScript is disabled, all content remains visible by default.

- [ ] **Step 2: Run static structural checks**

Run:

```powershell
$html = Get-Content -Raw 'index.html'
@('id="intro"','id="features"','id="flow"','id="safety"','id="audience"','id="experience"','assets/tailorfit-experience-qr.png','prefers-reduced-motion','aria-label','alt=') | ForEach-Object { if ($html -notmatch [regex]::Escape($_)) { throw "Missing: $_" } }
if ($html -match '[Tt][Oo][Dd][Oo]|[Tt][Bb][Dd]|[Ll]orem ipsum') { throw 'Placeholder copy found' }
if ($html -match 'box-shadow|radial-gradient|linear-gradient') { throw 'Disallowed Claude-style visual primitive found' }
Write-Output 'Static checks passed'
```

Expected: `Static checks passed`.

- [ ] **Step 3: Check file paths, QR size, and Git diff**

Run:

```powershell
Get-Item 'assets/tailorfit-experience-qr.png' | Select-Object FullName, Length
git diff --check HEAD~2..HEAD
git status --short
```

Expected: QR file is non-empty, diff check has no whitespace errors, and only intended project files are present.

- [ ] **Step 4: Commit verification updates**

Run:

```powershell
git add -- index.html
git commit -m "test: verify TailorFit intro page accessibility hooks"
```

Expected: the working tree is clean after the verification commit.
