# Repository Portfolio, Architecture and Migration Specification
Version 1.1

---

# Part I. Executive Summary

## Purpose

This document defines the long-term architecture of the entire project ecosystem.

Instead of considering repositories as independent code bases, the ecosystem is treated as a collection of products, production tools, research laboratories and automation services working together.

The objective is to establish clear ownership boundaries between repositories, simplify future maintenance, reduce duplication of code and assets, and make every repository responsible for exactly one concern.

This specification also serves as the migration plan from the current organically grown structure toward a modular architecture.

---

## Vision

The ecosystem consists of four major layers.

### Products

Products are what users actually use.

Each product owns:

- its public interface
- its content
- its business logic
- its deployment
- its publication pipeline

Products never become generic libraries.

Examples:

- RoadsOfTimes
- Nebulacast Site
- Nebulacast App
- Stellar Attractor

---

### Production Tools

Production tools create assets used by products.

They do not own published content.

Instead they generate:

- graphics
- animations
- HUDs
- SVG
- videos
- collages
- scientific visualizations

Examples:

- Scientific Visualization Studio
- HUD Visual Composer
- HUD Playground

---

### Research

Research repositories contain experiments.

They are intentionally unstable.

Their purpose is exploration rather than production.

Research repositories may later produce code that is promoted into either

- Products
- Production Tools
- Shared Libraries

Examples:

- Attractor Lab
- AstroLab

---

### Automation

Automation repositories contain AI agents and orchestration workflows.

Their purpose is operating the ecosystem rather than publishing content.

Examples:

- Automation Agents

---

## Architectural Principle

The entire portfolio follows one simple rule.

> Products own content.
>
> Production tools produce content.
>
> Research discovers new methods.
>
> Automation moves information between them.

This principle determines where every new file belongs.

---

# Part II. Repository Classification

Repositories are divided into four categories.

---

## 1. Products

Repositories directly delivering value to end users.

Characteristics:

- deployable
- published
- contain content
- own business logic
- versioned independently

Products may consume multiple production tools simultaneously.

---

Current products:

| Repository | Purpose |
|------------|---------|
| RoadsOfTimes | Historical museums, routes, exhibits and publication platform |
| Nebulacast Site | Public astronomy website |
| Nebulacast App | APIs, weather, astronomy services, widgets |
| Stellar Attractor | Fictional universe, stories, media and publications |

---

## 2. Production Tools

Repositories that generate assets.

Characteristics:

- reusable
- independent
- contain algorithms
- no product-specific business logic

Examples:

- SVG generation
- HUD rendering
- animation generation
- infographic creation

Current repositories:

| Repository | Purpose |
|------------|---------|
| Scientific Visualization Studio | Scientific animations and visualizations |
| HUD Visual Composer | Interactive HUD generation framework |
| HUD Playground | Experimental HUD layout editor |

---

## 3. Research

Repositories intended for experimentation.

Characteristics:

- notebooks
- prototypes
- experiments
- ML research
- reproducible pipelines

Research repositories are allowed to contain temporary code.

Current repositories:

| Repository | Purpose |
|------------|---------|
| Attractor Lab | Scientific research laboratory |
| AstroLab | Experimental playground |

---

## 4. Automation

Repositories operating the ecosystem.

Characteristics:

- AI agents
- orchestration
- scheduled tasks
- monitoring
- publication

Current repositories:

| Repository | Purpose |
|------------|---------|
| Automation Agents | AI agents and orchestration platform |

---

## 5. Shared Libraries (optional)

Shared libraries appear only when multiple repositories require identical code.

They should never become a dumping ground.

Possible future responsibilities:

- common schemas
- shared configuration
- cloud utilities
- reusable parsers
- common formats

---

## 6. Archive (future)

Cold storage for completed projects.

Contains:

- obsolete renders
- historical sources
- old datasets
- archived projects

Archive is never imported as a dependency.

---

# Part III. Core Architectural Principles

## Principle 1

Every repository has one primary responsibility.

No repository should simultaneously act as

- product
- library
- research
- archive

---

## Principle 2

Products own content.

Every product stores its own published assets.

Example:

RoadsOfTimes

```
content/
    exhibits/
    articles/
    images/
```

Nebulacast

```
content/
    articles/
    animations/
    figures/
```

Stellar Attractor

```
content/
    episodes/
    scenes/
    graphics/
```

The product is always the source of truth for published material.

---

## Principle 3

Production tools never own published content.

They own only:

- source code
- rendering engines
- templates
- algorithms

Generated results are copied into products.

Production tools therefore remain reusable.

---

## Principle 4

Research never depends on products.

Research may create

- prototype code
- notebooks
- experimental datasets

Successful work is promoted into

- Production Tools
- Shared Libraries
- Products

Research repositories are intentionally disposable.

---

## Principle 5

Automation never owns business logic.

Automation coordinates repositories.

Typical workflow:

Research

↓

Production Tool

↓

Product

↓

Publishing

↓

Monitoring

Automation orchestrates this process but does not replace any participant.

---

## Principle 6

Repositories communicate through artifacts rather than source code whenever practical.

Examples:

Animation

```
Visualization Studio
        ↓
generated SVG
        ↓
RoadsOfTimes
```

HUD

```
HUD Visual Composer
        ↓
generated HTML/SVG
        ↓
Nebulacast
```

Scientific figure

```
Visualization Studio
        ↓
PNG/SVG/WebM
        ↓
Stellar Attractor
```

This minimizes dependencies.

---

# Part IV. Repository Portfolio

## RoadsOfTimes

Category:

Product

Purpose:

Primary repository of the Roads of Times project.

Responsibilities:

- website
- museum database
- historical routes
- exhibit cards
- publication
- CMS
- generated content
- deployment

Owns:

- all published articles
- museum assets
- historical content
- final exhibit cards

Consumes:

- HUD Visual Composer
- Scientific Visualization Studio
- Automation Agents

Does not contain:

- reusable rendering engines
- generic visualization libraries

---

## Nebulacast Site

Category:

Product

Purpose:

Public astronomy website.

Responsibilities:

- articles
- pages
- media
- presentation
- SEO
- publication

Owns:

- published articles
- figures
- illustrations
- embedded animations

Consumes:

- Scientific Visualization Studio
- HUD Visual Composer
- Nebulacast App

---

## Nebulacast App

Category:

Product

Purpose:

Backend platform powering Nebulacast.

Responsibilities:

- APIs
- Workers
- weather
- heliophysics
- astronomy
- widgets
- frontend application
- data aggregation

Owns:

- backend services
- API contracts
- processed datasets
- application frontend

Consumes:

- Scientific Visualization Studio
- Automation Agents

---

## Stellar Attractor

Category:

Product

Purpose:

Complete repository of the Stellar Attractor universe.

Responsibilities:

- lore
- episodes
- scripts
- production assets
- generated media
- publication

Owns:

- story canon
- episodes
- renders
- dialogues
- publications

Consumes:

- Scientific Visualization Studio
- HUD Visual Composer
- Automation Agents

---

## Scientific Visualization Studio

Category:

Production Tool

Purpose:

Universal engine for scientific visualizations.

Generates:

- scientific animations
- diagrams
- graphs
- infographics
- animagraphics
- educational visualizations

Must remain independent of any individual product.

Products import generated outputs.

---

## HUD Visual Composer

Category:

Production Tool

(formerly HUD Tracer)

Purpose:

Universal framework for creating interactive HUD interfaces.

Responsibilities:

- SVG HUD generation
- responsive HUD layouts
- HTML generation
- interactive widgets
- collage generation
- reusable HUD components

No product-specific content should be stored here.

Only rendering logic.

---

## HUD Playground

Category:

Production Tool

Purpose:

Experimental environment for designing and testing HUD layouts before integrating them into HUD Visual Composer.

Responsibilities:

- rapid prototyping
- layout experiments
- responsive testing
- visual validation
- sandbox development

The Playground is intentionally disposable.

Successful ideas migrate into HUD Visual Composer.

```
# Part V. Content Ownership Model

## Fundamental Rule

The ownership of content is the most important architectural rule in the ecosystem.

Every published asset has exactly one owner.

That owner is always a Product repository.

Production Tools generate assets.

Products own assets.

---

## Product Content

Each product contains its own content directory.

Example:

RoadsOfTimes

```text
content/
    museums/
    exhibits/
    periods/
    places/
    articles/
    media/
```

Nebulacast

```text
content/
    articles/
    figures/
    animations/
    datasets/
```

Stellar Attractor

```text
content/
    episodes/
    scenes/
    dialogues/
    concept_art/
    media/
```

Products never reference unpublished assets inside production tools.

Everything required for publication must exist inside the product repository.

---

## Production Tool Outputs

Production tools produce temporary outputs.

Typical structure:

```text
output/
    svg/
    png/
    webm/
    html/
```

or

```text
exports/
```

These directories are considered generated artifacts.

Once approved, they are copied into the destination product.

---

## Source Assets

Production tools own source materials.

Typical examples:

```text
assets/
templates/
fonts/
icons/
svg/
animations/
```

These assets remain inside the production tool because they belong to the rendering engine rather than the final publication.

---

## Workflow

A typical lifecycle looks like this:

```text
Research

↓

Production Tool

↓

Generated Assets

↓

Product Content

↓

Publishing

↓

Website
```

Ownership changes only once:

Production Tool → Product

Never in the opposite direction.

---

## Versioning

Generated assets are versioned together with the product.

The production tool is versioned independently.

This guarantees reproducible publications.

---

# Part VI. Production Workflow

The ecosystem follows a common production pipeline regardless of project.

---

## Stage 1 — Research

Performed inside:

- Attractor Lab
- AstroLab

Activities:

- scientific investigation
- notebooks
- experiments
- ML
- algorithm prototyping

Output:

knowledge

No production assets are created here.

---

## Stage 2 — Tool Development

Performed inside Production Tools.

Examples:

Scientific Visualization Studio

HUD Visual Composer

HUD Playground

Activities:

- rendering
- visualization
- SVG generation
- animation
- UI generation

Output:

generated media

---

## Stage 3 — Product Assembly

Performed inside product repositories.

Activities:

- article writing
- museum descriptions
- story writing
- metadata
- publication structure

Output:

publishable content

---

## Stage 4 — Publishing

Performed automatically.

Possible targets:

- Blogger
- Cloudflare
- Static Sites
- Social Networks
- YouTube
- Telegram
- VK
- Future CMS

Automation performs publication but never edits source material.

---

## Stage 5 — Monitoring

Automation Agents monitor:

- publications
- news
- weather
- astronomy
- alerts
- statistics

Results are delivered back into products or services.

---

# Part VII. Migration Strategy

Repository migration should be evolutionary.

Large-scale rewrites are explicitly prohibited.

---

## Rule 1

Move repositories before moving code.

Repository boundaries are more important than directory layout.

---

## Rule 2

Rename repositories before restructuring internals.

Example:

HUD Tracer

↓

HUD Visual Composer

Only afterwards should internal folders be reorganized.

---

## Rule 3

Keep products functional during migration.

Each migration step must leave every product buildable.

---

## Rule 4

Avoid simultaneous refactoring and migration.

Migration changes location.

Refactoring changes implementation.

Never combine both.

---

## Rule 5

Promote reusable code gradually.

When identical code appears in multiple repositories:

1. identify duplication
2. stabilize implementation
3. extract into shared library (if justified)

Do not create shared libraries prematurely.

---

## Recommended Migration Order

### Phase 1

Repository cleanup.

- remove obsolete folders
- remove caches
- remove generated files
- remove legacy experiments

---

### Phase 2

Create new repositories.

- RoadsOfTimes
- Nebulacast Site
- Nebulacast App
- Stellar Attractor
- Scientific Visualization Studio
- HUD Visual Composer
- HUD Playground
- Automation Agents

---

### Phase 3

Move production tools.

No behavioral changes.

Only relocation.

---

### Phase 4

Move products.

Transfer content ownership.

Verify publication pipelines.

---

### Phase 5

Extract shared code only where duplication is proven.

---

### Phase 6

Archive historical repositories.

Nothing should be deleted immediately.

Everything should remain recoverable.

---

# Part VIII. Long-Term Vision

The final architecture forms a clean ecosystem with clearly separated responsibilities.

```text
                   Research
          +-----------------------+
          |  Attractor Lab        |
          |  AstroLab             |
          +----------+------------+
                     |
                     v
          +-----------------------+
          | Production Tools      |
          |                       |
          | Scientific            |
          | Visualization Studio  |
          |                       |
          | HUD Visual Composer   |
          |                       |
          | HUD Playground        |
          +----------+------------+
                     |
                     v
          +-----------------------+
          | Products              |
          |                       |
          | RoadsOfTimes          |
          | Nebulacast Site       |
          | Nebulacast App        |
          | Stellar Attractor     |
          +----------+------------+
                     |
                     v
          +-----------------------+
          | Automation            |
          |                       |
          | Automation Agents     |
          +----------+------------+
                     |
                     v
          +-----------------------+
          | Publishing            |
          | Websites              |
          | APIs                  |
          | Social Networks       |
          +-----------------------+
```

## Final Principle

The ecosystem is organized around responsibilities rather than technologies.

Every repository should answer one simple question:

**"What is my responsibility?"**

If that answer cannot be expressed in one sentence, the repository probably has more than one responsibility and should be split.

This principle should guide all future architectural decisions.

# Part IX. Repository Lifecycle

Repositories should evolve naturally as the ecosystem grows.

Creating a new repository is a significant architectural decision and should only happen when a clear responsibility boundary exists.

---

## Evolution Path

Every new idea follows the same lifecycle.

```text
Idea
    ↓
Research
    ↓
Prototype
    ↓
Production Tool
    ↓
Product Feature
```

If multiple products begin using the same implementation, it may eventually become a shared library.

```text
Research
      ↓
Prototype
      ↓
Production Tool
      ↓
Shared Library (only if justified)
```

---

## When NOT to Create a Repository

Do not create a new repository merely because:

- a directory becomes large;
- a project reaches several thousand files;
- a feature appears interesting;
- a team member prefers separation.

Large repositories are acceptable when they still represent a single responsibility.

---

## When TO Create a Repository

A new repository is justified when all of the following are true:

- it has a clearly defined responsibility;
- it can be versioned independently;
- it has its own release cycle;
- it may be reused by other repositories;
- it has its own documentation.

---

## Repository Promotion

Repositories may evolve.

Example:

```text
Notebook
      ↓
AstroLab

Algorithm stabilizes
      ↓
Scientific Visualization Studio

Used by products
      ↓
RoadsOfTimes
Nebulacast
Stellar Attractor
```

The reverse path should never occur.

Production code should never migrate back into research repositories.

---

# Part X. Naming Conventions

Consistency is preferred over creativity.

Repository names should immediately communicate their role.

---

## Product Repositories

Use:

```text
<product-name>
```

Examples:

```text
roadsoftimes
nebulacast-site
nebulacast-app
stellar-attractor
```

---

## Production Tools

Use:

```text
<tool-name>
```

Examples:

```text
scientific-visualization-studio
hud-visual-composer
hud-playground
```

---

## Research

Use:

```text
<domain>-lab
```

Examples:

```text
attractor-lab
astrolab
```

---

## Automation

Use:

```text
automation-agents
```

Future examples:

```text
automation-publisher
automation-monitor
automation-workflows
```

---

## Standard Directory Names

The following names are preferred across all repositories.

```text
assets/
content/
configs/
data/
docs/
exports/
output/
scripts/
spec/
tests/
tools/
```

Avoid inventing alternative names for the same concept.

For example, prefer one standard rather than mixing:

```text
outputs/
generated/
renders/
build/
render/
result/
```

---

## Content Directories

Every product should expose a predictable structure.

Example:

```text
content/
    articles/
    animations/
    datasets/
    figures/
    media/
```

RoadsOfTimes extends this model with:

```text
content/
    museums/
    exhibits/
    periods/
    places/
    media/
```

---

## Production Tool Structure

Recommended layout:

```text
assets/
docs/
examples/
output/
scripts/
src/
tests/
```

Generated files belong only in `output/`.

---

# Part XI. Migration Checklist

Repository migration should always begin with cleanup.

The objective is to migrate only valuable history.

---

## General Cleanup

Remove:

```text
□ __pycache__
□ .pytest_cache
□ node_modules
□ .DS_Store
□ temporary outputs
□ backup folders
□ obsolete screenshots
□ duplicate exports
```

---

## Review Legacy Content

Evaluate:

```text
□ archive/
□ legacy/
□ sandbox/
□ experiments/
□ backup/
```

Move historical material into an Archive repository if needed.

---

## Verify Repository Health

Check:

```text
□ README
□ LICENSE
□ .gitignore
□ pyproject.toml
□ requirements.txt
□ package.json
□ CI configuration
□ documentation
```

---

## Remove Sensitive Information

Verify that no repository contains:

```text
□ API keys
□ passwords
□ Cloudflare tokens
□ local paths
□ personal credentials
```

---

## Large Files

Identify:

```text
□ videos
□ PSD
□ GIMP files
□ Blender files
□ large datasets
```

Decide whether they belong in Git, Git LFS, external storage or an Archive repository.

---

## Validate Ownership

Before moving any directory, ask one question:

> Who owns this content?

If the answer is:

- Product → move into Product repository.
- Production Tool → move into Production Tool repository.
- Research → move into Research repository.
- Archive → move into Archive.

Ownership determines location.

---

## Migration Rule

Migration should always preserve a working system.

After every completed phase:

- repositories build successfully;
- products remain deployable;
- documentation reflects the new structure;
- old repositories can be archived without loss of information.

