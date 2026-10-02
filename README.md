# DORKAG JetBrains IDE Plugins Documentation

[![Build documentation](https://github.com/edgafner/edgafner.github.io/actions/workflows/build-docs.yml/badge.svg)](https://github.com/edgafner/edgafner.github.io/actions/workflows/build-docs.yml)
[![Pages](https://img.shields.io/badge/docs-live-brightgreen)](https://edgafner.github.io)
[![License](https://img.shields.io/badge/license-proprietary-lightgrey.svg)](LICENSE)

> Documentation for the DORKAG plugins for JetBrains IDEs

## 📚 Documentation

Visit the documentation site: [https://edgafner.github.io](https://edgafner.github.io)

## 🔌 Plugins

### [AZD: Azure DevOps](https://edgafner.github.io/azd.html)

Azure DevOps pull requests, pipelines and boards in JetBrains IDEs.

### [JirAI: Jira Cloud](https://edgafner.github.io/jirai.html)

Jira Cloud issues, boards and AI workflows in JetBrains IDEs, with Split Mode and Remote Development support.

### [Codecov: code coverage](https://edgafner.github.io/codecov.html)

Codecov line coverage and pull request impact in the editor.

### [QueryFlag: query templates](https://edgafner.github.io/queryflag.html)

Reusable query templates that you run on the text selected in the editor.

### [GBrowser: web browser](https://edgafner.github.io/gbrowser.html)

An embedded web browser inside your JetBrains IDE, with bookmarks, history and DevTools.

## 🚀 Quick start

### For plugin users

1. Open your JetBrains IDE (IntelliJ IDEA, WebStorm, PyCharm and others).
2. Open **Settings | Plugins** and select the **Marketplace** tab.
3. Search for the plugin name, for example AZD or GBrowser.
4. Click **Install**, then restart the IDE if prompted.

AZD, JirAI, Codecov and QueryFlag are paid plugins with a free trial; GBrowser is free.

### For contributors

```bash
# Clone the repository
git clone https://github.com/edgafner/edgafner.github.io.git
cd edgafner.github.io

# Documentation is built with Writerside. Preview with the Writerside IDE plugin,
# or build locally with the same Docker builder CI uses (work on a copy: the builder
# writes into the source directory, and the output directory must not be a mount point):
rsync -a --delete --exclude .git ./ /tmp/wrs-src/
docker run --rm --platform linux/amd64 -v /tmp/wrs-src:/github/workspace \
  jetbrains/writerside-builder:2026.09.0357 /bin/bash -c '
    export DISPLAY=:99; Xvfb :99 &
    /opt/builder/bin/idea.sh helpbuilderinspect -source-dir /github/workspace/ \
      -product Dorkag/dorkag --runner github -output-dir /github/workspace/artifacts/'
# Result: /tmp/wrs-src/artifacts/webHelpDORKAG2-all.zip and report.json
```

## 📖 Documentation structure

```
Dorkag/
├── topics/                   # Documentation content (.topic XML files)
│   ├── Dorkag.topic          # Home starting page
│   ├── common-support.topic  # Shared support snippet (library, never linked)
│   ├── azd/                  # AZD plugin documentation
│   ├── jirai/                # JirAI plugin documentation
│   ├── codecov/              # Codecov plugin documentation
│   ├── queryflag/            # QueryFlag plugin documentation
│   └── gbrowser/             # GBrowser plugin documentation
├── images/                   # All images in one flat folder; unique file names; _dark twin for each new screenshot
├── *.tree                    # TOC per instance (dorkag.tree includes the five plugin trees)
├── labels.list               # Plugin and version labels
├── c.list                    # See also categories
├── v.list                    # Variables
├── writerside.cfg            # Writerside configuration
└── cfg/                      # Build profiles and configuration
```

## 🤝 Contributing

Contributions are welcome:

1. **Report documentation issues**: [open an issue in this repository](https://github.com/edgafner/edgafner.github.io/issues).
   For plugin bugs, use [edgafner/dorkag](https://github.com/edgafner/dorkag/issues),
   or [edgafner/GBrowser](https://github.com/edgafner/GBrowser/issues) for GBrowser.
2. **Improve documentation**: submit a pull request with your improvements.
3. **Add examples**: share your use cases and examples.

### Documentation guidelines

- Write in clear, concise language
- Include screenshots for UI-related features
- Follow the existing Writerside XML structure
- Test your changes locally before submitting

## 🔧 Technology stack

- **Documentation engine**: [JetBrains Writerside](https://www.jetbrains.com/writerside/)
- **Hosting**: GitHub Pages
- **CI/CD**: GitHub Actions
- **Format**: XML-based topics with semantic markup

## 📊 Build status

The documentation is built and deployed on every push to the main branch. The workflow includes:

- Building documentation with Writerside
- Validating content structure
- Deploying to GitHub Pages
- Uploading artifacts to external repositories

## 📝 License

Copyright (c) 2023-2026 Dorkag. All rights reserved. See [LICENSE](LICENSE) for details.

## 🔗 Links

- **Issue tracker (all plugins except GBrowser)**: [github.com/edgafner/dorkag](https://github.com/edgafner/dorkag/issues)
- **Documentation site**: [edgafner.github.io](https://edgafner.github.io)
- **JetBrains Marketplace**: [AZD](https://plugins.jetbrains.com/plugin/22319-azd),
  [JirAI](https://plugins.jetbrains.com/plugin/33954-jirai),
  [Codecov](https://plugins.jetbrains.com/plugin/23390-codecov),
  [QueryFlag](https://plugins.jetbrains.com/plugin/18269-queryflag),
  [GBrowser](https://plugins.jetbrains.com/plugin/14458-gbrowser)

## 👥 Support

- **Plugin issues**: [github.com/edgafner/dorkag/issues](https://github.com/edgafner/dorkag/issues)
  (GBrowser: [github.com/edgafner/GBrowser/issues](https://github.com/edgafner/GBrowser/issues))
- **Documentation issues**: [github.com/edgafner/edgafner.github.io/issues](https://github.com/edgafner/edgafner.github.io/issues)
- **Twitter**: [@Jongafner](https://twitter.com/Jongafner)
- **LinkedIn**: [Jonathan Gafner](https://www.linkedin.com/in/jonathan-gafner-3415974b/)
- **Bluesky**: [@jgafner.bsky.social](https://bsky.app/profile/jgafner.bsky.social)

---

Built and maintained by Jonathan Gafner.
