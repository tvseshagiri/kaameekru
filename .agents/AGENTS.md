# Quarto CLI Quick Reference & Presentation Deck Standards

## Core Repository Standards & Presentation Guidelines

Follow these standards whenever creating, modifying, or refactoring presentation decks and course curricula in this repository.

---

### 1. Modularity & Componentization (Mandatory)

- **Dedicated Directory**: Every presentation deck must live in its own dedicated directory (e.g., `tech/python-ai/`, `tech/genai/intro/`, `tech/reactjs/`).
- **Orchestrator `index.qmd`**: The root presentation file contains only the YAML frontmatter and `{{< include slides/_XX-topic.qmd >}}` statements. Never place entire presentations in a single monolithic file.
- **Granular Slide Modules**: Author individual slide groups in a local `slides/` directory prefixed with sequential numbering:
  - `slides/_00-overview.qmd`
  - `slides/_01-tooling.qmd`
  - `slides/_XX-topic.qmd`
- **Preserve Existing Decks**: Existing decks (e.g., `tech/python/`, `tech/reactjs/`) must remain intact and functional unless explicitly directed to modify them.

---

### 2. Quarto Reveal.js Formatting & Slide Delimiters

- **Explicit Slide Separators (`---`)**:
  - **Always** place a horizontal rule `---` before each slide heading (`### Slide Title`).
  - Take special care after closing column divs (`::: :::`) or callouts; failing to add `---` causes Reveal.js to render subsequent headings and tables on the same slide.
- **Slide Heading Levels**:
  - `# Section Header`: Generates a full-screen module/topic divider slide.
  - `### Slide Title` (or `##`): Standard content slide.
- **Two-Column Layouts**:
  - Use `::: columns` with `::: {.column width="55%"}` and `::: {.column width="45%"}` for side-by-side code + concept boxes.
- **Callout Containers**:
  - Use standard Quarto callout blocks (`callout-note`, `callout-tip`, `callout-important`, `callout-caution`) for high-contrast takeaways.

---

### 3. Code Snippet Quality & Syntax Integrity

- **Executable & Error-Free**: Every code snippet presented as working code must be syntactically valid and runnable.
- **No Unexpected Indentations**: Ensure multi-column code blocks do not carry orphaned class or function indentations that cause `IndentationError` when copied.
- **Accurate Syntax Highlighting**:
  - Use ```` ```python ```` only for syntactically valid Python code.
  - Use ```` ```text ```` for pseudo-code, shorthand keyword summaries, or symbol cheat sheets.
  - Use ```` ```bash ````, ```` ```toml ````, or ```` ```dockerfile ```` for configuration and terminal commands.
- **Python Package Management**: Always use **`uv`** as the default Python package manager (`uv init`, `uv add`, `uv sync --frozen`, `uv run`).
- **FastAPI Best Practices**: Whenever writing API backend code, structure it using modular `APIRouter`, `lifespan` context managers for pooled resources, streaming SSE responses, and custom timing/tracing middleware.

---

### 4. Comprehensive Reference Implementations (Labs & Capstones)

- **Pair Labs with Solutions**: For every hands-on lab or capstone exercise slide, immediately follow it with a **Complete Reference Implementation** slide providing copy-pasteable, production-grade code.
- **Realistic Agent Framework Patterns**:
  - State reducers (`operator.add` with `Annotated`)
  - Boundary data validation via Pydantic v2 `BaseModel` & `TypeAdapter`
  - Structured concurrency with `asyncio.TaskGroup` and `asyncio.timeout`
  - DAG dependency resolution using Python stdlib `graphlib.TopologicalSorter`
  - Atomic persistence with SQLite transactions and `contextvars` request tracing

---

### 5. Portal Navigation & Course Catalog Integration

Whenever a new deck is created:
1. **Top Navbar (`_quarto.yml`)**:
   - Register the presentation under `website.navbar.left` -> `Presentations` menu.
   - Register the syllabus under `website.navbar.left` -> `Courses` menu (if applicable).
2. **Portal Home Page (`index.qmd`)**:
   - Add a row to the `## Presentations` table with a descriptive summary.
   - Add a row to the `## Course Curricula & Syllabi` table when a companion syllabus is created.
3. **Course Syllabus Page (`courses/<topic>.qmd`)**:
   - Provide an HTML-formatted syllabus with course description, daily schedule breakdown, prerequisites, and a direct link to the interactive presentation.

---

### 6. Local Verification Workflow

Before committing and merging:
1. **Validate Presentation Build**:
   ```bash
   quarto render tech/<topic>/index.qmd
   ```
2. **Validate Full Site Build**:
   ```bash
   quarto render
   ```
   Ensure all documents compile cleanly into `docs/` with exit code `0`.
3. **Verify Code Syntax**:
   Run an AST parse or execute sample snippets with `uv run python` to catch syntax or import issues.
4. **Test Local Serving**:
   ```bash
   python3 -m http.server 8088 --directory docs
   ```
   Verify `http://localhost:8088/` and the new presentation return `HTTP 200 OK`.

---

## Common Quarto Commands Quick Reference

- **Render Site/Docs**: `quarto render` (compiles source `.qmd` files into the `docs/` folder).
- **Live Preview**: `quarto preview` (launches local development server with live reload).
- **Environment Check**: `quarto check` (verifies Quarto, Pandoc, TinyTeX, Python, and Chrome dependencies).
- **Inspect Metadata**: `quarto inspect` (inspects project or input file metadata).
- **Publish Site**: `quarto publish [provider]` (publishes rendered site to GitHub Pages, Netlify, Posit Connect, etc.).
