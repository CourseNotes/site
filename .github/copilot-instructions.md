# Refract Learning Repository - Copilot Coding Agent Instructions

## Repository Overview

**Refract Learning** (formerly CourseNotes) is a free, open-source revision website built with Jekyll that provides educational content for students. The site features clean, structured notes aligned with exam board specifications, curated video resources, and smart study tools powered by Orbit for retrieval practice.

**Repository Size:** ~3MB (80 files excluding vendor/build artifacts)
**Primary Technology:** Jekyll 4.4.1 (Ruby-based static site generator)
**Deployment:** Vercel (configured via vercel.json)
**CI/CD:** GitHub Actions workflow for TODO-to-issue conversion

## Technology Stack

- **Language:** Ruby 3.2.3+
- **Static Site Generator:** Jekyll 4.4.1
- **Package Manager:** Bundler 2.6.9 (or newer)
- **Markup:** Markdown with YAML front matter
- **Styling:** Pure CSS (assets/style.css - 304 lines)
- **Third-party Integrations:** 
  - Orbit (withorbit.com) for retrieval practice
  - Tawk.to for support widget

## Build and Development Commands

### Initial Setup (REQUIRED - Run Once)

**CRITICAL:** Bundler must be installed with user-install flag to avoid permission errors:

```bash
gem install --user-install bundler
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
```

**ALWAYS** add the bundler bin path to PATH before running any bundle commands. This prevents "command not found" errors.

### Install Dependencies (REQUIRED Before Building)

```bash
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
bundle config set --local path 'vendor/bundle'
bundle install
```

**Time Required:** 30-60 seconds  
**Note:** This installs all gems to vendor/bundle (already in .gitignore)

### Build Site

```bash
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
bundle exec jekyll build
```

**Time Required:** <5 seconds  
**Output:** Generates static site in `_site/` directory  
**Success Indicator:** "done in X.XXX seconds"

### Clean Build Artifacts

```bash
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
bundle exec jekyll clean
```

**Removes:** _site/, .jekyll-cache/, .jekyll-metadata

### Serve Locally (Development Server)

```bash
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
bundle exec jekyll serve
```

**Default URL:** http://localhost:4000  
**Auto-rebuild:** Enabled by default (watches for file changes)  
**To disable auto-rebuild:** Add `--no-watch` flag  
**To skip initial build:** Add `--skip-initial-build` flag

### Testing Changes Workflow

1. Make code/content changes
2. Run `bundle exec jekyll build` to verify no build errors
3. Run `bundle exec jekyll serve` to preview locally
4. Visit http://localhost:4000 to manually verify changes
5. Stop server (Ctrl+C)

## Project Architecture

### Directory Structure

```
/
├── _config.yml              # Jekyll configuration (title, encoding, exclude list)
├── _data/                   # YAML data files
│   ├── courses.yml          # Course metadata (id, title, url)
│   └── 8702_B_2.yml         # Course-specific data
├── _includes/               # Reusable HTML components
│   ├── head.html            # HTML head (title, CSS, favicon)
│   ├── header.html          # Site navigation
│   ├── footer.html          # Footer
│   ├── course_library.html  # Course listing component
│   ├── course_content.html  # Course content listing
│   ├── module_resources.html # Resource links
│   ├── orbit_reviewarea.html # Orbit integration
│   └── tawk.html            # Support widget
├── _layouts/                # Page templates
│   ├── index.html           # Homepage layout
│   ├── page.html            # Generic page layout
│   ├── course_home.html     # Course landing page
│   └── 8702_B_2_poem.html   # Poetry analysis layout
├── assets/
│   └── style.css            # Main stylesheet (304 lines)
├── courses/                 # Course content (Markdown files)
│   ├── index.md             # Course catalog page
│   ├── 8702_B_2/            # GCSE English Literature course
│   │   ├── index.md         # Course home
│   │   └── poems/           # 15 poem analysis files
│   └── J277/                # GCSE Computer Science (outdated)
├── img/                     # Images and graphics
├── index.md                 # Homepage content
├── about.md                 # About page
├── Gemfile                  # Ruby dependencies (only Jekyll 4.4.1)
└── Gemfile.lock             # Locked dependency versions
```

### Content Structure

**Courses** are defined in `_data/courses.yml` with:
- `title`: Display name
- `id`: Course identifier (e.g., "AQA 8702/B/2")
- `url`: Course path (e.g., "/courses/8702_B_2")

**Pages** use YAML front matter for metadata:
```yaml
---
title: Page Title
layout: layout_name
course: course_id  # For course content
---
```

### Configuration Files

- **_config.yml**: Jekyll site configuration
  - `title`: "CourseNotes" (Note: Site is rebranding to Refract Learning)
  - `encoding`: "utf-8"
  - `exclude`: README, LICENSE, build artifacts
- **.gitignore**: Excludes _site/, vendor/, .bundle/, .jekyll-cache/
- **vercel.json**: Deployment config (`cleanUrls: true`)
- **renovate.json**: Automated dependency updates

## GitHub Workflows

### TODO-to-Issue Action (.github/workflows/todo.yml)

**Trigger:** Every push to any branch  
**Purpose:** Automatically converts TODO comments to GitHub issues

**Behavior:**
- Scans code for TODO comments
- Creates/updates corresponding issues
- Inserts issue URLs back into code comments
- Commits changes with message: "chore: add issue link to TODO comment"
- Auto-assigns issues to repository members

**Important:** If you add TODO comments, expect automated commits after pushing.

## Validation and Testing

### No Automated Tests

This repository **does not** have:
- Unit tests
- Integration tests
- Linting tools (no eslint, htmlhint, stylelint)
- Pre-commit hooks

### Manual Validation Steps

1. **Build Verification:** Ensure `bundle exec jekyll build` completes without errors
2. **Visual Inspection:** Run dev server and manually check pages at http://localhost:4000
3. **Link Verification:** Click through navigation and ensure no broken links
4. **Content Verification:** Verify markdown renders correctly, front matter is valid
5. **Layout Verification:** Check that includes/layouts are properly referenced

### Common Build Errors and Solutions

**Error:** "Bundler command not found"  
**Solution:** Run `gem install --user-install bundler` and add to PATH

**Error:** "Permission denied" during gem install  
**Solution:** Always use `--user-install` flag with gem commands

**Error:** "Could not find gem 'jekyll'"  
**Solution:** Run `bundle install` before build commands

**Error:** "Liquid syntax error"  
**Solution:** Check Liquid template syntax in HTML files and markdown content

## Making Changes

### Content Updates

**For Course Notes:**
1. Navigate to `courses/[course_id]/`
2. Edit existing .md files or create new ones
3. Include proper YAML front matter with `course`, `layout`, `title`
4. Follow existing formatting patterns (bullet points, keywords section)
5. Add Orbit review areas when appropriate

**For Site Pages:**
1. Edit .md files in repository root (index.md, about.md)
2. Maintain YAML front matter structure
3. Use appropriate layout from `_layouts/`

### Layout/Template Updates

**Location:** `_layouts/` and `_includes/`  
**Language:** Liquid templates + HTML  
**Testing:** Always rebuild and visually verify changes in browser

### Styling Updates

**File:** `assets/style.css`  
**Note:** Pure CSS, no preprocessors (Sass, SCSS)  
**Testing:** Check responsiveness and visual consistency

### Adding New Courses

1. Add course metadata to `_data/courses.yml`
2. Create directory: `courses/[course_id]/`
3. Create `courses/[course_id]/index.md` with course_home layout
4. Add content files (markdown) in subdirectories
5. Create course-specific data file in `_data/` if needed
6. Create custom layout in `_layouts/` if needed

## Development Best Practices

### File Locations

- **Configuration:** Root directory
- **Content:** `courses/` and root .md files
- **Templates:** `_layouts/` and `_includes/`
- **Data:** `_data/`
- **Static Assets:** `assets/`, `img/`
- **Build Output:** `_site/` (never commit)

### Git Ignore

**Always excluded from commits:**
- `_site/` - Build output
- `vendor/` - Bundler gems
- `.bundle/` - Bundler config
- `.jekyll-cache/` - Jekyll cache
- `.jekyll-metadata` - Jekyll metadata

### Known TODOs in Codebase

1. `_includes/header.html`: Implement Contributors Page & CTA in Header
2. `_includes/header.html`: Create Contact Page

## Key Facts for Efficiency

### When to Search vs. Trust Instructions

**Trust these instructions for:**
- Build commands and their sequence
- Directory structure and file locations
- Jekyll/Ruby version requirements
- Common workflows

**Search/explore only when:**
- Instructions are incomplete or incorrect
- Working with new/undocumented features
- Debugging unexpected errors
- Adding entirely new functionality

### Performance Notes

- Build time: <5 seconds (fast)
- Install time: 30-60 seconds (one-time per environment)
- No compilation needed (pure HTML/CSS/Markdown)
- Hot-reload works well with `jekyll serve --watch`

### Content Conventions

- Use markdown for content (not HTML when possible)
- Follow bullet-point format for notes
- Include "Key Words" section at top of educational content
- Maintain consistent front matter structure
- Align content with exam board specifications

### Repository Root Files

```
Favicon.ico          - Site favicon (193KB)
Gemfile              - Ruby dependencies
Gemfile.lock         - Locked versions
LICENSE              - GNU GPL v3.0 (35KB)
README.MD            - Project documentation
_config.yml          - Jekyll config
about.md             - About page content
index.md             - Homepage content
renovate.json        - Renovate config
vercel.json          - Vercel deployment config
```

## Quick Reference

### Complete Build Workflow (Clean Environment)

```bash
# Setup (one-time)
gem install --user-install bundler
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"

# Install dependencies
bundle config set --local path 'vendor/bundle'
bundle install

# Build and test
bundle exec jekyll build
bundle exec jekyll serve

# Visit http://localhost:4000 to verify
```

### Making and Testing a Change

```bash
# 1. Edit files (content, templates, or styles)

# 2. Test build
export PATH="$HOME/.local/share/gem/ruby/3.2.0/bin:$PATH"
bundle exec jekyll build

# 3. Preview locally
bundle exec jekyll serve

# 4. Verify in browser at http://localhost:4000

# 5. Stop server (Ctrl+C) and commit changes
```

### Dependencies

**Required Gems (from Gemfile.lock):**
- jekyll (4.4.1) - Core static site generator
- jekyll-sass-converter (3.1.0) - Sass support
- kramdown (2.5.1) - Markdown parser
- liquid (4.0.4) - Template engine
- rouge (4.5.2) - Syntax highlighting
- And 28 other dependencies (auto-installed)

**Note:** Only Jekyll 4.4.1 is explicitly specified in Gemfile; all others are transitive dependencies.

---

**Last Updated:** Based on repository state as of November 2025  
**Maintainer Contact:** Via widget at notes.opott.uk
