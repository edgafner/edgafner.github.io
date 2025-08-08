# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Writerside documentation site for DORKAG JetBrains IDE plugins hosted on GitHub Pages. The repository contains documentation for multiple JetBrains plugin projects:

- **AZD**: Azure DevOps integration plugin
- **GBrowser**: Browser integration plugin  
- **Codecov**: Code coverage plugin
- **QueryFlag**: Query management plugin
- **RunLikeMe**: Run configuration management plugin

## Key Commands

### Build Documentation
```bash
# Documentation is built automatically via GitHub Actions
# Manual build requires Writerside Docker
docker run --rm -v $PWD:/opt/sources jetbrains/writerside-builder:241.18775 /opt/builder.sh
```

### Deploy Documentation
Documentation is automatically deployed to GitHub Pages when pushing to the `main` branch via the GitHub Actions workflow at `.github/workflows/build-docs.yml`.

### Local Preview
To preview documentation locally, use the Writerside IDE plugin or the Writerside Docker container.

## Repository Structure

### Documentation Architecture
- **Writerside Configuration**: `Dorkag/writerside.cfg` - Main configuration file defining instances and resources
- **Build Profiles**: `Dorkag/cfg/buildprofiles.xml` - Defines build settings, variables, and footer configuration
- **Instance Trees**: Each plugin has its own `.tree` file defining documentation structure:
  - `dorkag.tree` - Main documentation tree
  - `azdlib.tree`, `gbrowserlib.tree`, `codecovlib.tree`, `queryflaglib.tree`, `runlikemelib.tree` - Plugin-specific trees

### Content Organization
- **Topics**: `Dorkag/topics/` - Contains all documentation content in `.topic` XML files
  - Each plugin has its own subdirectory with related topics
  - Topics follow Writerside XML schema for structured documentation
- **Images**: `Dorkag/images/` - All documentation images organized by plugin
  - Each plugin has dedicated image directories
  - Includes screenshots, icons, and diagrams

### GitHub Actions Workflow
The `.github/workflows/build-docs.yml` workflow handles:
1. **Build**: Uses JetBrains/writerside-github-action to build documentation
2. **Test**: Validates documentation with writerside-checker-action
3. **Deploy**: Publishes to GitHub Pages
4. **Algolia**: Updates search indexes
5. **Release**: Creates GitHub releases with documentation artifacts
6. **Trigger**: Optionally triggers deployment to external Dorka system

### Key Configuration Variables
- **Algolia Search**: Configured with app ID `0ZSRMCDHQ9` and index `Dorkag`
- **Web Root**: https://edgafner.github.io
- **Docker Version**: 241.18775
- **Primary Color**: Aqua theme

## Working with Documentation

### Adding New Topics
1. Create a new `.topic` file in the appropriate `Dorkag/topics/[plugin]/` directory
2. Follow the Writerside XML schema for topic structure
3. Add the topic reference to the corresponding `.tree` file
4. Place related images in `Dorkag/images/[plugin]/`

### Modifying Existing Documentation
- Edit `.topic` files directly in `Dorkag/topics/`
- Ensure XML validity according to Writerside DTD
- Update images in corresponding directories if needed

### Build Verification
The GitHub Actions workflow automatically:
- Builds documentation on push to main
- Tests documentation validity
- Deploys to GitHub Pages
- Updates Algolia search indexes
- Creates release artifacts

## Important Notes
- Documentation is written in Writerside XML format, not Markdown
- All topics must validate against Writerside DTD schemas
- Images should be placed in appropriate subdirectories under `Dorkag/images/`
- The site uses Algolia for search functionality
- GitHub Pages deployment is automatic on main branch updates