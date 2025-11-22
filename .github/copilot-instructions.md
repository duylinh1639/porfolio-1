# Repo-specific Copilot instructions — porfolio-1

This is a small, static single-page portfolio. The goal of these instructions is to help an AI coding agent make focused, low-risk changes that match the project's conventions.

- **Big picture:** Single static `index.html` (no build system). HTML, CSS and JS are colocated in `index.html` (styles in a `<style>` block in `<head>`, behavior in an inline `<script>` at the end of `<body>`). The site is served as static files — edits are visible by opening `index.html` in a browser or serving the directory via a simple static server.

- **Key files:** `index.html` (single source of truth). `README.md` contains only the repo title.

- **Architecture & patterns:**
  - Presentation and behavior are implemented in one file. Preserve ordering: fonts & cdn links -> styles -> body markup -> modal elements -> inline scripts.
  - Custom cursor: elements `.cursor-dot` and `.cursor-outline` are required and rely on `* { cursor: none; }` in CSS. When modifying cursor logic or CSS, ensure these elements remain present or the site will lose pointer affordances.
  - Hover affordances: interactive elements use the `hover-target` class. Add that class to any new interactive element so the custom cursor hover style and event listeners work.
  - Scroll reveal: use the `reveal` class on sections/elements to trigger the IntersectionObserver-based entrance animation and to initialize components (e.g., progress bars).
  - Skill/progress bars: elements have `.progress-fill` with a `data-width` attribute (e.g., `data-width="85%"`) — the script reads this value and sets inline `width` on reveal. Do not hardcode the `width` in CSS if you expect the JS to animate it.
  - Masonry layout: `.masonry-grid` uses CSS columns (no JS). Keep project items as `.portfolio-item` inside `.masonry-grid` and avoid changing DOM structure that breaks CSS column flow (use `break-inside: avoid` as present).
  - Modal API: global functions `openModal(title, cat, desc)` and `closeModal()` are used by portfolio items via `onclick`. If refactoring to eventListeners, keep backward-compatible wrappers (preserve global names) to avoid breaking markup-based handlers.
  - Theme toggle: toggles `body.light-mode` class and swaps the icon classes. Keep that class toggle behavior if changing theme variables.

- **Developer workflows & commands:**
  - No build step. For local testing, open `index.html` directly or run a simple static server. Example PowerShell commands:

    ```powershell
    # Python 3 simple server (serves current directory at http://localhost:8000)
    python -m http.server 8000

    # or (if Node.js installed) using http-server
    npx http-server -p 8080
    ```

  - For live reload while editing, use a VS Code Live Server extension or an IDE live preview plugin.

- **Editing rules and safe refactors:**
  - Keep CSS variables in `:root` and the `body.light-mode` overrides to preserve dark/light color semantics.
  - When extracting CSS/JS to separate files, update `index.html` to reference them and keep the same load order (fonts -> CSS -> body -> JS) to avoid race conditions.
  - Avoid removing inline `on*` attributes (e.g., `onclick="openModal(...)"`) unless you provide equivalent event wiring and maintain the same function names for compatibility.
  - When adding interactive elements, include `hover-target` to get the custom cursor behavior and accessible hover feedback.

- **Network & dependency notes:**
  - Google Fonts and FontAwesome are loaded via CDN; offline dev requires network or local copies.

- **Examples from the codebase:**
  - Cursor: `<div class="cursor-dot"></div><div class="cursor-outline"></div>` at top of `<body>`
  - Hover targets: `<a class="hero-cta hover-target">...` and portfolio items use `hover-target` on the clickable container.
  - Progress bars: `<div class="progress-fill" data-width="90%"></div>` — script sets `style.width` on reveal.
  - Modal call: `onclick="openModal('Project One', 'Branding', '...')"` — keep or wrap this API.

If anything here looks incorrect or you want me to expand an example (for instance, extract JS to a separate file or add a small dev script), tell me which area to update and I will iterate. 
