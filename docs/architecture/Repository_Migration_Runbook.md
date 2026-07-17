# Repository Migration Runbook

## Purpose

This document defines the preparation, migration, verification, and archival process for reorganizing the current repository portfolio into a clear product/tool/content architecture.

The primary goal is to migrate safely without losing source code, content, Git history, media assets, reproducibility, or deployment capability.

The migration must not be combined with broad refactoring, schema redesign, UI/UX changes, or feature work.

---

# -1. Prerequisites

This phase is mandatory and must be completed before preparing repositories for migration.

The objective is to establish a stable GitHub, Git, and workspace environment so that the migration can proceed without changing infrastructure midway through the process.

---

## -1.1 Create a GitHub Organization

Create a dedicated GitHub Organization that will own all active repositories.


```text
incusluminis
```

The organization, rather than a personal account, becomes the canonical owner of the portfolio.

---

## -1.2 Configure Organization Ownership

Add both GitHub accounts as Organization Owners.

Verify that both accounts have permission to:

- manage repositories;
- create repositories;
- manage teams;
- transfer repositories;
- manage organization settings.

No repository migration should begin until both accounts have full owner access.

---

## -1.3 Create Initial Teams

Create the initial organization teams.

Recommended minimum:

```text
Owners
Developers
Automation
```

Even if there is currently only one contributor, using teams from the beginning simplifies future growth.

---

## -1.4 Approve Final Repository Names

Before creating or transferring repositories, approve the final repository names.

Current portfolio:

| Repository | Purpose | File Types | Deployment | Team | Comment |
|------------|---------|------------|------------|------|---------|
| `.github` | Organization-wide GitHub configuration | Markdown, YAML | GitHub | Owners | Profile, issue templates, PR templates, workflows, community files |
| `standards` | Architecture, coding standards, ADRs, specifications | Markdown, YAML | GitHub | Core | Engineering standards shared across all projects |
| `docs` | Organization documentation and runbooks | Markdown | GitHub | Core | Onboarding, migration guides, architecture documentation |
| `core` | Shared libraries and reusable components | Python, TypeScript, JSON | Package / GitHub | Core | Common code used by multiple products |
| `tools` | CLI tools, converters, utilities | Python, Shell | GitHub | Core | Standalone developer tools and generators |
| `workflows` | CI/CD pipelines and automation | YAML, Python | GitHub Actions | Core | Shared GitHub Actions and automation workflows |
| `ai` | AI agents, prompts and orchestration | Python, Markdown, YAML | GitHub | AI | Claude Code, Codex, MCP servers, agent definitions |
| `research` | Research notes and knowledge base | Markdown | GitHub | AI | Scientific notes, experiments, reference material |
| `labs` | Experimental prototypes and PoC | Any | GitHub | AI | Disposable experiments before production |
| `assets` | Shared organization branding assets | SVG, PNG, Fonts | GitHub | Core | Logos, icons, fonts, presentation templates |
| `data` | Shared reference datasets | JSON, YAML, CSV | GitHub | Core | Dictionaries, taxonomies, reference data |
| `nebulacast-content` | NebulaCast content production pipeline | Markdown, YAML, JSON | GitHub | Products, AI | Sources, summaries, scripts, prompts, publication manifests |
| `nebulacast-site` | NebulaCast website | HTML, CSS, JS, Python | Production | Products | Website source code only |
| `nebulacast-app` | NebulaCast applications | Source code | Production | Products | Desktop/mobile applications |
| `roadsoftimes-content` | Roads of Times content production pipeline | Markdown, YAML, JSON | GitHub | Products, AI | Museums, exhibits, articles, routes, publication manifests |
| `roadsoftimes` | Roads of Times platform | Source code | Production | Products | Website, widgets, tools, backend/frontend |
| `stellar-attractor` | Stellar Attractor production | Source code, Markdown | Production | Products | Universe, scripts, tools and production pipeline |
| `stellar-attractor-content` | Stellar Attractor content production pipeline | Markdown, YAML, JSON | GitHUB | Products | SA Universe content |
| `visualization-studio` | Visualization and HUD tools | Python, TypeScript | Production | Products | HUD Tracer, editors, visualization utilities |

Repository names should not change during the migration.

## CDN Resources

| Resource | Purpose | Backing Storage | Owner | Comment |
|----------|---------|-----------------|-------|---------|
| `media.roadsoftimes.com` | Public media delivery for Roads of Times | Cloudflare R2 | Products | Museum photos, exhibit images, HUD animations, route illustrations, audio and video assets |
| `media.nebulacast.com` | Public media delivery for NebulaCast | Cloudflare R2 | Products | Article illustrations, infographics, covers, podcasts, narration, videos and social media assets |
| `media.visualization-studio.com` | Public media delivery for Visualization Studio | Cloudflare R2 | Products | Demo projects, UI screenshots, sample animations, documentation assets and downloadable resources |
| `media.stellar-attractor.space` | Public media delivery for Stellar Attractor | Cloudflare R2 | Products | Character artwork, environments, cinematic stills, videos, music, voice assets and production materials |

## Product Storage Model

Every product consists of three independent storage layers.

| Layer | Purpose | Typical Files | Storage |
|--------|---------|---------------|---------|
| **Code** | Product source code and implementation | Python, TypeScript, HTML, CSS, JavaScript, configuration | GitHub |
| **Content** | Structured content and publication data | Markdown, JSON, YAML, prompts, manifests, translations, metadata | GitHub |
| **Media** | Binary assets used by the product | PNG, WebP, JPG, SVG, MP4, WebM, MP3, WAV, PDF | CDN (Cloudflare R2) |

### Storage Rules

- **Code** defines how the product works.
- **Content** defines what the product contains.
- **Media** contains large binary assets referenced by the content layer.

Content references media through stable CDN URLs.
Code consumes content but should never embed publication data directly.
Large binary files must not be stored in Git repositories.

---

## -1.5 Decide Repository Visibility

For every repository, define its visibility before migration.

Possible values:

```text
Public
Private
Internal
```

Visibility changes should not be mixed with repository restructuring.

---

## -1.6 Configure SSH Authentication

Create separate SSH keys for each GitHub account.

Example:

```text
~/.ssh/id_ed25519_github_personal
~/.ssh/id_ed25519_github_work
```

Configure SSH aliases:

```text
github-personal
github-work
```

Verify access:

```bash
ssh -T github-personal
ssh -T github-work
```

The migration must not rely on repeatedly switching GitHub credentials.

---

## -1.7 Configure Git Identity

Verify global Git configuration:

```bash
git config --global --list
```

Configure repository-local identity where required:

```bash
git config --local user.name "Michael Loktionov"
git config --local user.email "your@email"
```

Historical commits must not be rewritten solely to unify author identity.

---

## -1.8 Create the Portfolio Workspace

Choose one root directory for the complete portfolio.

Example:

```text
~/Projects/Portfolio/
```

or

```text
~/Development/
```

All active repositories should reside under this workspace.

---

## -1.9 Configure Shared Workspace Settings

Create the shared workspace configuration:

```text
~/.config/attractor-workspace/repos.yml
```

All production tools should eventually resolve repository locations through this configuration instead of hard-coded absolute paths.

---

## -1.10 Review Git LFS Usage

Determine whether Git LFS is required.

If used:

- install Git LFS;
- verify existing tracked objects;
- identify future file types that should use LFS;
- document LFS rules before migration.

---

## -1.11 Create the Portfolio Migration Project

Create a GitHub Project to track migration progress.

Suggested workflow:

```text
Preparation
Ready
Migrating
Verification
Completed
Archived
```

Every repository migration should be tracked through this board.

---

## -1.12 Define Branch Policies

Approve branch strategy before migration.

Typical branches:

```text
main
develop
```

Decide:

- protected branches;
- merge strategy;
- required reviews;
- linear history;
- squash or merge commits.

---

## -1.13 Prepare Repository Template

Prepare a standard repository template containing at least:

```text
README.md
LICENSE
.gitignore
.github/
docs/
CODEOWNERS
```

New repositories should be created from a consistent baseline.

---

## -1.14 Prepare Backup Storage

Create a dedicated backup location.

Example:

```text
~/PortfolioBackups/
```

It should contain:

```text
Git mirrors
filesystem snapshots
migration reports
checksums
```

Backups must exist before any repository restructuring begins.

---

## -1.15 Declare a Migration Freeze

Once migration starts:

- do not create new projects;
- do not rename repositories;
- do not perform feature development;
- do not perform architecture redesign;
- do not perform UI/UX refactoring;
- do not reorganize directory structures unless required by the migration plan.

During the migration window, only migration-related work, documentation updates, verification, and critical fixes are allowed.


# 0. Target Architecture

## Product repositories

- `roadsoftimes`
- `nebulacast-site`
- `nebulacast-app`
- `stellar-attractor`
- `scientific-visualization-studio`
- `publishing-system`
- `automation-agents`

## Production tools repository

- `production-tools`

Expected internal structure:

```text
production-tools/
├── apps/
├── packages/
├── pipelines/
├── cli/
├── schemas/
├── tests/
├── examples/
└── docs/
```

## Research repository

- `astrolab`

## Future repositories

- `shared-libraries` — create only when shared packages are actually used by multiple product repositories.
- `media-archive` — cold storage for completed projects, legacy renders, large source assets, and inactive material.

---

# 1. Core Migration Rules

1. Code belongs to the component that owns its behavior.
2. Product-specific content belongs to the product repository that owns its meaning.
3. Shared production tools must not own product content.
4. Generated artifacts are not canonical source.
5. Source code must not be duplicated between repositories.
6. Product-owned scripts, notebooks, manifests, assets, and outputs may coexist under the product `content/` tree.
7. Migration and refactoring are separate operations.
8. Existing names and internal paths must be preserved during the initial move unless a path change is required to make the target repository valid.
9. Nothing may be deleted before it is classified and recorded in the migration manifest.
10. Every migrated unit must have an owner, a target, and a verification method.

---

# 2. Pre-Migration Repository Preparation Checklist

This phase is mandatory and must be completed before any repository split, merge, transfer, or path rewrite.

## 2.1 Create a migration inventory

For every source repository, collect:

```text
repository name
GitHub account owner
local path
remote URL
default branch
active branches
Git tags
repository size
working tree status
current deployment dependencies
large file locations
known generated directories
known local-only directories
```

Create:

```text
docs/migration/repository-inventory.yml
```

Suggested structure:

```yaml
repositories:
  - name: ANIM
    local_path: /absolute/path/to/ANIM
    remote: git@github-account:owner/ANIM.git
    default_branch: main
    size_bytes: 0
    status: active
    target_state: archive_after_split
```

## 2.2 Freeze feature development

Before migration:

- finish or explicitly suspend active branches;
- merge critical work;
- record known broken states;
- stop unrelated cleanup;
- stop UI/UX refactoring during repository movement;
- stop path normalization until the move is verified.

Allowed during the migration window:

- migration commits;
- path fixes required for startup;
- dependency fixes required for startup;
- documentation changes;
- migration manifests;
- test and verification fixes.

Not allowed:

- module redesign;
- class renaming;
- schema redesign;
- project-format redesign;
- rendering changes;
- visual output changes;
- feature development.

## 2.3 Create Git baselines

For each repository:

```bash
git status
git branch --show-current
git remote -v
git tag --list
```

Commit all intended tracked changes:

```bash
git add -A
git commit -m "chore: create pre-migration baseline"
git tag pre-portfolio-migration
git push origin --all
git push origin --tags
```

If the repository contains intentional uncommitted work, document it instead of silently discarding it.

## 2.4 Create complete backups

Create both Git and filesystem backups.

### Git mirror

```bash
git clone --mirror <repository-url> backups/<repository>.git
```

### Filesystem snapshot

```bash
rsync -a \
  --exclude='.git' \
  <source-repository>/ \
  backups/<repository>-filesystem/
```

The filesystem snapshot is mandatory because large media, generated exports, databases, and local files may not exist in Git.

## 2.5 Generate file and directory reports

For each repository generate:

```bash
tree -a -L 6 \
  -I '.git|node_modules|__pycache__|.venv|venv|dist|build|.idea|.vscode|.pytest_cache|.mypy_cache' \
  > docs/migration/tree-before.txt
```

Also generate a file-size report:

```bash
find . -type f -print0 \
  | xargs -0 du -h \
  | sort -h \
  > docs/migration/file-sizes-before.txt
```

Generate a top-level disk usage report:

```bash
du -sh ./* ./.??* 2>/dev/null | sort -h \
  > docs/migration/disk-usage-before.txt
```

## 2.6 Automated cleanup candidates

The following may usually be deleted automatically after review:

```text
__pycache__/
*.pyc
*.pyo
.pytest_cache/
.mypy_cache/
.ruff_cache/
.coverage
htmlcov/
node_modules/
dist/
build/
.esbuild/
.next/
.cache/
.DS_Store
Thumbs.db
*.log
*.tmp
*.temp
*.bak
*.swp
*.swo
```

Recommended dry-run command:

```bash
find . \
  \( \
    -name '__pycache__' -o \
    -name '.pytest_cache' -o \
    -name '.mypy_cache' -o \
    -name '.ruff_cache' -o \
    -name 'node_modules' -o \
    -name 'dist' -o \
    -name 'build' -o \
    -name '.DS_Store' \
  \) \
  -print
```

Only after the output is reviewed:

```bash
find . \
  \( \
    -name '__pycache__' -o \
    -name '.pytest_cache' -o \
    -name '.mypy_cache' -o \
    -name '.ruff_cache' -o \
    -name 'node_modules' -o \
    -name 'dist' -o \
    -name 'build' \
  \) \
  -prune -exec rm -rf {} +

find . -name '.DS_Store' -delete
find . -type f \( -name '*.pyc' -o -name '*.pyo' -o -name '*.log' \) -delete
```

Do not delete generated directories if they contain canonical assets that cannot be reproduced.

## 2.7 Automated duplicate detection

Generate checksums for all files above a selected threshold:

```bash
find . -type f -size +1M -print0 \
  | xargs -0 shasum -a 256 \
  | sort \
  > docs/migration/checksums-large-files.txt
```

Find exact duplicates:

```bash
find . -type f -size +1M -print0 \
  | xargs -0 shasum -a 256 \
  | sort \
  | awk '{hash=$1; $1=""; files[hash]=files[hash] ORS $0; count[hash]++} END {for (h in count) if (count[h] > 1) print h files[h] ORS}' \
  > docs/migration/duplicate-large-files.txt
```

Duplicates must not be deleted automatically unless ownership is already clear.

## 2.8 Automated Git hygiene checks

Check tracked generated files:

```bash
git ls-files \
  | grep -E '(^|/)(__pycache__|node_modules|dist|build|\.pytest_cache|\.mypy_cache)(/|$)' \
  > docs/migration/tracked-generated-files.txt || true
```

Check large tracked files:

```bash
git ls-files -z \
  | xargs -0 -I{} sh -c 'test -f "$1" && printf "%s\t%s\n" "$(stat -f%z "$1" 2>/dev/null || stat -c%s "$1")" "$1"' _ {} \
  | sort -n \
  > docs/migration/tracked-file-sizes.txt
```

Review files larger than 25 MB and 100 MB separately.

## 2.9 Secrets and private configuration scan

Before moving repositories or making them public, inspect for:

```text
API keys
Cloudflare tokens
GitHub tokens
service-account JSON
SSH keys
private URLs
passwords
local filesystem paths
personal email exports
cookies
browser profiles
```

At minimum search for:

```bash
grep -RInE \
  'api[_-]?key|secret|token|password|private[_-]?key|BEGIN [A-Z ]*PRIVATE KEY|cloudflare|github_pat_' \
  . \
  --exclude-dir=.git \
  --exclude-dir=node_modules \
  > docs/migration/possible-secrets.txt || true
```

Do not assume the absence of grep matches means the repository is safe.

## 2.10 Absolute path scan

Search for hard-coded machine-specific paths:

```bash
grep -RInE \
  '/Users/|/home/|[A-Za-z]:\\\\|PycharmProjects|Desktop|Downloads' \
  . \
  --exclude-dir=.git \
  --exclude-dir=node_modules \
  > docs/migration/absolute-paths.txt || true
```

Do not rewrite all paths yet. Classify them first:

```text
required runtime path
example path
test fixture
legacy path
documentation only
safe to remove
```

## 2.11 Manual cleanup checklist

The following require manual review:

- `archive/`, `old/`, `backup/`, `backup2/`, `legacy/`, `_tmp/`, `suspicious/`;
- multiple versions such as `_v2`, `_final`, `_final2`, `_new`, `_old`;
- duplicated project folders in multiple repositories;
- duplicated `assets/`, `exports/`, `output/`, `videos/`, `previews/` trees;
- abandoned prototypes;
- obsolete Blogger experiments;
- superseded UI screenshots;
- old database backups;
- incomplete projects with unknown owners;
- one-off generated renders;
- copied third-party libraries;
- fonts without clear redistribution rights;
- temporary notebooks and notebook backups;
- local virtual environments;
- broken symbolic links;
- stale deployment output;
- old release packages;
- unused example assets;
- files with inconsistent Unicode normalization in names.

For every reviewed item, assign one action:

```text
keep
migrate
archive
delete
unresolved
```

## 2.12 Normalize ignore rules

Each repository must have an updated `.gitignore` before migration.

Minimum Python/Node/media-production baseline:

```gitignore
.DS_Store
__pycache__/
*.py[cod]
.pytest_cache/
.mypy_cache/
.ruff_cache/
.venv/
venv/
node_modules/
dist/
build/
coverage/
htmlcov/
*.log
*.tmp
*.temp
.env
.env.*
!.env.example
```

Product-specific generated directories must be added only after their canonical status is confirmed.

## 2.13 Identify canonical vs generated assets

Every large asset directory must be classified as one of:

```text
source asset
generated intermediate
publication artifact
cache
temporary output
archive
```

Examples:

```text
PNG cutout master           → source asset
800 px derivative           → generated intermediate or publication artifact
WebM export                 → publication artifact
preview image               → generated artifact
render cache                → cache
old export                  → archive
```

No migration decision may be based solely on directory names such as `output`, `exports`, or `assets`.

## 2.14 Build a cleanup report

For each repository create:

```text
docs/migration/CLEANUP_REPORT.md
```

Required sections:

```text
Removed automatically
Removed manually
Retained despite being generated
Archived
Unresolved
Large files
Duplicate files
Secrets reviewed
Absolute paths reviewed
Open risks
```

## 2.15 Cleanup acceptance criteria

A repository is ready for migration only when:

- the working tree is clean;
- backups exist;
- baseline tag exists;
- generated junk is removed;
- `.gitignore` is current;
- large files are classified;
- duplicates are reviewed;
- secrets are reviewed;
- absolute paths are inventoried;
- canonical data is identified;
- unresolved directories are listed;
- cleanup changes are committed separately.

Recommended commit:

```text
chore: prepare repository for portfolio migration
```

---

# 3. GitHub Account and Ownership Preparation

## 3.1 Create a GitHub Organization

All active target repositories should be owned by one GitHub Organization.

Both personal GitHub accounts should be organization owners.

Personal accounts must not determine the architectural ownership of repositories.

## 3.2 Configure separate SSH identities

Use separate SSH aliases for both accounts.

Example:

```sshconfig
Host github-primary
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_primary
    IdentitiesOnly yes
    AddKeysToAgent yes
    UseKeychain yes

Host github-secondary
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_github_secondary
    IdentitiesOnly yes
    AddKeysToAgent yes
    UseKeychain yes
```

## 3.3 Configure repository-local author identity

Set `user.name` and `user.email` locally per repository when needed.

```bash
git config --local user.name "Michael Loktionov"
git config --local user.email "verified-email@example.com"
```

Do not rewrite historical commits solely to unify account attribution.

---

# 4. Migration Manifest

Create one central manifest:

```text
portfolio-migration/MIGRATION_MANIFEST.yml
```

Required fields:

```yaml
items:
  - id: anim-hud-panel-tracer
    source_repository: ANIM
    source_path: Infographics/tools/hud_panel_tracer
    target_repository: production-tools
    target_path: apps/visual-composer
    category: production_tool
    owner: production-tools
    action: move
    preserve_history: true
    verification:
      - application_starts
      - baseline_project_opens
      - baseline_export_matches
    status: planned
```

Allowed categories:

```text
product_code
product_content
production_tool
shared_package
pipeline
research
source_asset
generated_artifact
archive
unresolved
```

Allowed actions:

```text
move
copy_then_archive
archive
delete
keep_in_place
review
```

Allowed statuses:

```text
planned
prepared
migrating
migrated
verified
archived
blocked
```

Every source path must appear exactly once in the manifest at the selected migration granularity.

---

# 5. Target Repository Preparation

Create the target repositories before splitting source repositories.

Each target repository must contain:

```text
README.md
.gitignore
docs/architecture/
docs/migration/
```

Do not create speculative deep directory structures before actual components are migrated.

Create only the minimum required skeleton.

---

# 6. Migration Waves

## Wave 1 — Transfer intact repositories

Candidates:

- current `RoadsOfTimes` → `roadsoftimes`;
- current `nebulacast-app` → `nebulacast-app`;
- another repository only if its responsibility already matches the target repository.

Use GitHub repository transfer when the repository remains conceptually intact.

After transfer:

```bash
git remote set-url origin git@github-primary:<organization>/<repository>.git
git fetch --all --prune
```

Do not reorganize internal directories during the transfer commit.

## Wave 2 — Create `production-tools`

Initial candidates:

```text
ANIM/Infographics/tools/hud_panel_tracer
ANIM/Infographics/tools/info_compositor
ANIM/Infographics/tools/info_picker
ANIM/Infographics/tools/ui_inspector
HUD/widgets/staging.html and related frame-editing code
RoadsOfTimes/tools/museums
image processing scripts
video processing scripts
preview generators
validation scripts
```

Initial target layout:

```text
production-tools/
├── apps/
│   ├── visual-composer/
│   ├── content-workbench/
│   ├── frame-editor/
│   └── ui-inspector/
├── pipelines/
│   ├── museum-ingestion/
│   ├── image-processing/
│   ├── video-processing/
│   ├── preview-generation/
│   ├── content-validation/
│   └── publication-packaging/
└── packages/
```

Important:

- first move tools as-is;
- do not merge applications during migration;
- do not rename internal modules during migration;
- `HUD Tracer` may be renamed to `Visual Composer` at the repository/path level, but internal code renaming is a separate milestone;
- `staging` must remain an environment name, not an application name;
- product projects must be removed from tool-owned directories only after their product targets exist.

## Wave 3 — Move product-owned content

### Roads of Times

Move:

```text
museum projects
historical HUD projects
exhibit cards
historical collages
Zorndorf-related production material
Bolkow-related production material
Panzermuseum-related production material
Marinemuseum-related production material
```

Target pattern:

```text
roadsoftimes/content/
├── museums/
├── visual-projects/
├── collages/
├── maps/
├── source-assets/
├── generated/
└── published/
```

### Nebulacast

Move:

```text
article-specific animations
DOI-specific graphs
article-specific notebooks
article-specific datasets
article-specific infographic projects
Nebulacast publishing media
```

Target pattern:

```text
nebulacast-site/content/
├── articles/
├── papers/
├── visual-projects/
├── source-assets/
├── generated/
└── published/
```

If content production becomes too large, split it later into `nebulacast-content`. Do not introduce that repository during the first migration unless necessary.

### Stellar Attractor

Create:

```text
stellar-attractor/
├── canon/
├── episodes/
├── characters/
├── locations/
├── ships/
├── content/
├── production/
└── docs/
```

Move all product-specific Stellar Attractor material there.

## Wave 4 — Create Scientific Visualization Studio

Move only reusable or standalone scientific visualizations.

Suitable candidates:

```text
Lorenz attractor
Klein bottle
gyroid
Hopf fibration
HR diagram
gravity visualizations
spectroscopy visualizations
pulsar demonstrations
magnetosphere demonstrations
orbital mechanics demonstrations
gravitational lensing demonstrations
```

Do not move:

- article-specific datasets;
- article-specific conclusions;
- Stellar Attractor narrative assets;
- Roads of Times historical compositions;
- HUD UI components;
- unfinished experiments without a stable interface.

Each visualization should eventually follow:

```text
scientific-visualization-studio/visualizations/<domain>/<name>/
├── src/
├── data/
├── presets/
├── examples/
├── tests/
├── outputs/
├── metadata.yml
└── README.md
```

During initial migration, preserve existing internal layout where practical.

## Wave 5 — Automation Agents

Move agent and orchestration code into:

```text
automation-agents/
├── agents/
├── orchestrators/
├── workflows/
├── connectors/
├── configs/
├── schemas/
├── tests/
└── docs/
```

`AG_amateur_alerts` must be split by responsibility:

- reusable agent/orchestration logic → `automation-agents`;
- operational astronomy service logic → `nebulacast-app`;
- site-specific assets → owning product;
- generated outputs → owning product or archive.

## Wave 6 — Research consolidation

Review current `attractor-lab` and `astrolab`.

Target principle:

```text
unfinished experiment           → astrolab
validated reusable visualizer   → scientific-visualization-studio
operational service             → nebulacast-app
publication workflow            → publishing-system
article-specific work           → product content
shared low-level utility        → production-tools/packages or future shared-libraries
```

Do not merge research repositories until ownership of each major topic is classified.

## Wave 7 — Archive ANIM and HUD

After all active components are migrated:

```text
ANIM
HUD
```

must become read-only archives.

Required archive contents:

```text
MIGRATION_MANIFEST.md
MIGRATION_MAP.yml
UNRESOLVED.md
checksums/
```

Nothing should remain actively developed there.

---

# 7. Preserving Git History

Use history preservation selectively.

Preserve history for:

```text
active application code
shared packages
important pipelines
long-lived reusable visualizations
```

History preservation is optional for:

```text
product content snapshots
binary media
old renders
archived experiments
temporary outputs
```

Use `git filter-repo` when extracting a subdirectory from a large source repository.

Example:

```bash
git clone <source-url> production-tools-migration
cd production-tools-migration

git filter-repo \
  --path Infographics/tools/hud_panel_tracer/ \
  --path-rename Infographics/tools/hud_panel_tracer/:apps/visual-composer/
```

Never run destructive history rewriting against the only local clone.

---

# 8. Workspace and Path Configuration

No migrated tool may depend on hard-coded absolute paths.

Create user-local workspace configuration:

```text
~/.config/attractor-workspace/repos.yml
```

Example:

```yaml
repositories:
  roadsoftimes: /Users/<user>/Projects/portfolio/roadsoftimes
  nebulacast_site: /Users/<user>/Projects/portfolio/nebulacast-site
  nebulacast_app: /Users/<user>/Projects/portfolio/nebulacast-app
  stellar_attractor: /Users/<user>/Projects/portfolio/stellar-attractor
  scientific_visualization_studio: /Users/<user>/Projects/portfolio/scientific-visualization-studio
  production_tools: /Users/<user>/Projects/portfolio/production-tools
  astrolab: /Users/<user>/Projects/portfolio/astrolab
```

Project manifests must reference:

```text
repository alias + relative path
```

not machine-specific absolute paths.

---

# 9. Verification Protocol

Every migrated item must pass a declared verification checklist.

## Applications

```text
[ ] dependencies install
[ ] application starts
[ ] primary window opens
[ ] baseline project opens
[ ] assets resolve
[ ] export completes
[ ] no source path references remain
```

## Pipelines

```text
[ ] CLI starts
[ ] help command works
[ ] sample input is processed
[ ] output is generated
[ ] output location is correct
[ ] errors are actionable
```

## Product content

```text
[ ] source files exist
[ ] manifests resolve
[ ] scripts/notebooks run where required
[ ] generated artifact can be reproduced or provenance is documented
[ ] published copy exists where required
```

## Websites and applications

```text
[ ] local build succeeds
[ ] staging deploy succeeds
[ ] public assets resolve
[ ] existing URLs remain valid or redirects exist
[ ] no production secret is committed
```

## Visual output

Save baseline exports before migration.

Compare:

```text
dimensions
duration
frame count
file type
file size
visual preview
checksum when deterministic
```

---

# 10. Commit Strategy

Use small commits with one responsibility.

Good examples:

```text
chore(migration): import visual composer from ANIM
chore(migration): move PzII project to roadsoftimes content
fix(paths): resolve roadsoftimes content roots
chore(archive): mark legacy HUD repository read-only
```

Bad examples:

```text
move everything
cleanup and refactor
new architecture
final migration
```

Do not mix:

```text
migration + refactor
migration + feature
migration + schema redesign
migration + visual redesign
```

---

# 11. Rollback Strategy

Every wave must be independently reversible.

Rollback options:

1. restore from Git mirror;
2. restore filesystem snapshot;
3. reset target repository to pre-wave tag;
4. restore old remote configuration;
5. re-enable old deployment;
6. reopen old repository only if verification fails.

Create a tag before every wave:

```text
pre-wave-1
pre-wave-2
pre-wave-3
```

Do not delete source repositories until all waves are verified.

---

# 12. Definition of Done

The portfolio migration is complete when:

- all active repositories are owned by the GitHub Organization;
- both GitHub accounts have owner access;
- `roadsoftimes`, `nebulacast-site`, `nebulacast-app`, `stellar-attractor`, `scientific-visualization-studio`, `publishing-system`, `automation-agents`, `production-tools`, and `astrolab` have explicit responsibilities;
- product-specific content lives under the owning product repository;
- production tools no longer own product projects;
- no active code remains in `ANIM` or `HUD`;
- all migrated applications start;
- all required products build and deploy;
- migration manifests are complete;
- unresolved content is explicitly listed;
- source repositories are archived, not silently deleted;
- no hard-coded source repository paths remain;
- cleanup and migration reports are committed;
- subsequent refactoring can begin as a separate program.

---

# 13. Claude Execution Instructions

Claude must follow these rules during implementation:

1. Never move a directory without first locating it in the migration manifest.
2. Never delete a file unless it is backed up and classified.
3. Always show a dry-run list before automated deletion.
4. Never assume that `output`, `exports`, `build`, or `assets` are disposable.
5. Preserve current behavior during migration.
6. Avoid broad renaming.
7. Do not normalize all project layouts during initial movement.
8. Do not create new abstractions unless required to make migrated code run.
9. Record every source-to-target path mapping.
10. Update status in the migration manifest after each step.
11. Run verification after every migrated unit.
12. Stop the current migration unit if verification fails; do not continue cascading changes.
13. Commit each verified unit separately.
14. Keep source repositories available until final acceptance.
15. Treat unresolved ownership as a blocking classification issue, not as permission to place content in a generic repository.

