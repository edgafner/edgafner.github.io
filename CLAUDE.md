# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Writerside documentation site for DORKAG JetBrains IDE plugins hosted on GitHub Pages. The repository contains documentation for multiple JetBrains plugin projects:

- **AZD**: Azure DevOps integration plugin
- **GBrowser**: Web browser in an IDE tool window (free, open source)
- **Codecov**: Code coverage plugin
- **QueryFlag**: Query templates run on the selected text
- **JirAI**: Jira Cloud integration plugin

## Key Commands

### Build Documentation
Documentation is built automatically via GitHub Actions. To build locally with the same Docker builder, work on a copy
of the repository (the builder writes into the source directory, and the output directory must not be a mount point):

```bash
rsync -a --delete --exclude .git ./ /tmp/wrs-src/
docker run --rm --platform linux/amd64 -v /tmp/wrs-src:/github/workspace \
  jetbrains/writerside-builder:2026.09.0357 /bin/bash -c '
    export DISPLAY=:99; Xvfb :99 &
    /opt/builder/bin/idea.sh helpbuilderinspect -source-dir /github/workspace/ \
      -product Dorkag/dorkag --runner github -output-dir /github/workspace/artifacts/'
# Result: /tmp/wrs-src/artifacts/webHelpDORKAG2-all.zip and report.json (inspection results)
```

### Deploy Documentation
Documentation is automatically deployed to GitHub Pages when pushing to the `main` branch via the GitHub Actions workflow at `.github/workflows/build-docs.yml`.

### Local Preview
To preview documentation locally, use the Writerside IDE plugin. The Docker builder above produces the full site as a zip, plus report.json.

## Repository Structure

### Documentation Architecture
- **Writerside Configuration**: `Dorkag/writerside.cfg` - Main configuration file defining instances and resources
- **Build Profiles**: `Dorkag/cfg/buildprofiles.xml` - Defines build settings, variables, and footer configuration
- **Instance Trees**: Each plugin has its own `.tree` file defining documentation structure:
  - `dorkag.tree` - Main documentation tree
  - `azdlib.tree`, `gbrowserlib.tree`, `codecovlib.tree`, `queryflaglib.tree`, `jirailib.tree` - Plugin-specific trees

### Content Organization
- **Topics**: `Dorkag/topics/` - Contains all documentation content in `.topic` XML files
  - Each plugin has its own subdirectory with related topics
  - Topics follow Writerside XML schema for structured documentation
- **Images**: `Dorkag/images/` holds all images (screenshots, icons, diagrams) in one flat folder, with no per-plugin subfolders
  - Writerside resolves images by bare file name, so every file name must be unique
  - Every new screenshot has a `_dark` twin (`name.png` and `name_dark.png`); Writerside switches to it automatically

### GitHub Actions Workflow
The `.github/workflows/build-docs.yml` workflow handles:
1. **Build**: Uses JetBrains/writerside-github-action to build documentation
2. **Test**: Validates documentation with writerside-checker-action
3. **Deploy**: Publishes to GitHub Pages
4. **Upload**: Uploads artifacts to external Dorka repository

### Key Configuration Variables
- **Web Root**: https://edgafner.github.io
- **Docker Version**: 2026.09.0357
- **Primary Color**: Aqua theme

## Working with Documentation

### Adding New Topics
1. Create a new `.topic` file in the appropriate `Dorkag/topics/[plugin]/` directory
2. Follow the Writerside XML schema for topic structure
3. Add the topic reference to the corresponding `.tree` file
4. Place related images in `Dorkag/images/` with a unique file name

### Modifying Existing Documentation
- Edit `.topic` files directly in `Dorkag/topics/`
- Ensure XML validity according to Writerside DTD
- Update images in `Dorkag/images/` if needed

### Writing conventions
The full rules are in `docs/superpowers/specs/2026-10-02-docs-overhaul-design.md`. The binding ones:
- Topic titles follow `<Product>: <Sentence-case phrase>` (no second colon, no repeated product name). Plugin start pages use the bare product name.
- `toc-title` values in `.tree` files are sentence case, without the product prefix. Chapter and procedure titles are sentence case.
- Every topic has `<link-summary>`, `<card-summary>` and `<web-summary>`.
- Screenshots that are not captured yet appear as `<!-- planned-image src=... alt=... -->` comments. Keep them.
- Referenced images: `width` is the pixel width capped at 706, `thumbnail="true"` when the image is wider than 706 px, and `border-effect="rounded"`.
- Inside `<section-starting-page>`, link entries use `<a href type summary>`.
- End topics with `<seealso>`, using the `related` and `external` categories from `c.list`.
- Never link to `common-support.topic`; it is a library topic. Include its `support-content` snippet instead.
- Document only released versions. Do not present `[Unreleased]` changelog items as available.

### Build Verification
The GitHub Actions workflow automatically:
- Builds documentation on push to main
- Tests documentation validity
- Deploys to GitHub Pages
- Uploads artifacts to external repositories

## Important Notes
- Documentation is written in Writerside XML format, not Markdown
- All topics must validate against Writerside DTD schemas
- Images go in the flat `Dorkag/images/` folder, with unique file names
- GitHub Pages deployment is automatic on main branch updates