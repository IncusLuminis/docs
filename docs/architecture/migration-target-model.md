# IncusLuminis Migration Target Model

## Status

Accepted

## Purpose

This document defines the target repository layout for the migration of existing projects into the IncusLuminis GitHub organization.

Its primary purpose is to answer one question:

> Where should every existing project and asset be moved?

This document serves as the migration blueprint.

---

# Migration Principles

The migration follows several principles.

## Preserve history

Whenever practical, existing Git history should be preserved.

Repositories should be migrated rather than recreated.

---

## Separate responsibilities

Software, editorial content and media should be managed independently.

The canonical model is:

```text
Code
Content
Media
```

---

## Avoid unnecessary restructuring

Migration should minimize changes to working software.

Large refactoring efforts should happen after migration, not during it.

---

## Migrate incrementally

Projects should become operational as early as possible.

Perfect organization is not the initial goal.

---

# Target Repository Inventory

## Governance

```text
.github
standards
docs
```

---

## Platform

```text
repo-template
tools
core
workflows
ai
```

---

## Shared Resources

```text
assets
data
research
labs
```

---

## Products

```text
roadsoftimes
roadsoftimes-content

nebulacast-site
nebulacast-content

stellar-attractor

visualization-studio
```

---

# RoadsOfTimes

Current project:

```text
RoadsOfTimes
```

Target repositories:

```text
roadsoftimes
roadsoftimes-content
```

Responsibilities:

## roadsoftimes

Contains:

- application code
- Blogger templates
- widgets
- HUD components
- JavaScript
- CSS
- build scripts
- deployment configuration
- tooling

Does not contain:

- article texts
- museum descriptions
- large media collections

---

## roadsoftimes-content

Contains:

- museum metadata
- exhibit descriptions
- translations
- JSON content
- manifests
- editorial material

Does not contain:

- application code
- generated media

---

## Media

All large media moves to object storage.

Typical content:

```text
museum photographs
PNG
WEBM
preview images
background images
animations
```

Target:

```text
Cloudflare R2
```

Published through:

```text
media.roadsoftimes.com
```

---

# NebulaCast

Current project:

```text
NebulaCast
```

Target repositories:

```text
nebulacast-site
nebulacast-content
```

---

## nebulacast-site

Contains:

- website
- templates
- JavaScript
- styles
- deployment

---

## nebulacast-content

Contains:

- articles
- scientific summaries
- episode scripts
- translations
- publishing metadata

---

## Media

Stored in:

```text
Cloudflare R2
```

Published through:

```text
media.nebulacast.com
```

---

# Stellar Attractor

Current project:

```text
Stellar Attractor
```

Target repository:

```text
stellar-attractor
```

Contains:

- source code
- scripts
- project documentation
- prompts
- production pipeline
- AI workflow definitions
- animation tooling

Large generated media is not stored in Git.

Media destination:

```text
Cloudflare R2
```

Published through:

```text
media.stellar-attractor.space
```

---

# Visualization Studio

Target repository:

```text
visualization-studio
```

Contains:

- reusable visualization components
- HUD framework
- rendering tools
- animation utilities
- shared graphics tooling

This repository is expected to become reusable across multiple projects.

---

# Media Storage

Git repositories should not contain production media.

Canonical storage:

```text
GitHub
    Code
    Content

Cloudflare R2
    Images
    Audio
    Video
    Animations
    Generated assets
```

---

# Migration Order

Repositories should be migrated in the following order.

```text
1. repo-template

2. tools

3. standards

4. docs

5. RoadsOfTimes

6. NebulaCast

7. Stellar Attractor

8. Visualization Studio

9. Shared repositories
```

Each migration should complete before the next begins.

---

# Definition of Done

A repository migration is complete when:

- Git history is preserved;
- the repository builds successfully;
- documentation has been updated;
- large media has been moved to object storage;
- obsolete files have been removed;
- the project is operational from its new location.

---

# Future Work

After all repositories have been migrated, the organization may begin:

- shared library extraction;
- workflow consolidation;
- AI-agent integration;
- documentation improvements;
- infrastructure automation.

These activities are intentionally outside the scope of the migration itself.