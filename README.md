# DORKAG JetBrains IDE Plugins Documentation

[![Build documentation](https://github.com/edgafner/edgafner.github.io/actions/workflows/build-docs.yml/badge.svg)](https://github.com/edgafner/edgafner.github.io/actions/workflows/build-docs.yml)
[![Pages](https://img.shields.io/badge/docs-live-brightgreen)](https://edgafner.github.io)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

> Comprehensive documentation for the DORKAG suite of JetBrains IDE plugins

## 📚 Documentation

Visit the documentation site: [https://edgafner.github.io](https://edgafner.github.io)

## 🔌 Plugins Suite

### [AZD - Azure DevOps Integration](https://edgafner.github.io/azd.html)

Complete Azure DevOps integration for JetBrains IDEs. Manage pull requests, pipelines, and work items without leaving
your IDE.

### [GBrowser - Browser Integration](https://edgafner.github.io/gbrowser.html)

An embedded web browser inside your JetBrains IDE, with bookmarks, history and DevTools.

### [JirAI - Jira Cloud Integration](https://edgafner.github.io/jirai.html)

Complete Jira Cloud experience for JetBrains IDEs. Browse, filter, edit issues, and track discussions with native Split Mode and Remote Development support.

### [Codecov - Code Coverage](https://edgafner.github.io/codecov.html)

Visualize and track code coverage directly in your IDE.

### [QueryFlag - Query Management](https://edgafner.github.io/queryflag.html)

Reusable query templates that you run on the text selected in the editor.

## 🚀 Quick Start

### For Plugin Users

1. Open your JetBrains IDE (IntelliJ IDEA, WebStorm, PyCharm, etc.)
2. Go to **Settings/Preferences** → **Plugins**
3. Search for the plugin name (e.g., "AZD", "GBrowser")
4. Click **Install** and restart your IDE

### For Contributors

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

## 📖 Documentation Structure

```
Dorkag/
├── topics/           # Documentation content (.topic XML files)
│   ├── azd/         # AZD plugin documentation
│   ├── gbrowser/    # GBrowser plugin documentation
│   ├── codecov/     # Codecov plugin documentation
│   ├── queryflag/   # QueryFlag plugin documentation
│   └── jirai/       # JirAI plugin documentation
├── images/          # Documentation images and screenshots
├── writerside.cfg   # Writerside configuration
└── cfg/            # Build profiles and configuration
```

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Report Issues**: Found a bug or have a
   suggestion? [Open an issue](https://github.com/edgafner/edgafner.github.io/issues)
2. **Improve Documentation**: Submit a pull request with your improvements
3. **Add Examples**: Share your use cases and examples

### Documentation Guidelines

- Write in clear, concise language
- Include screenshots for UI-related features
- Follow the existing Writerside XML structure
- Test your changes locally before submitting

## 🔧 Technology Stack

- **Documentation Engine**: [JetBrains Writerside](https://www.jetbrains.com/writerside/)
- **Hosting**: GitHub Pages
- **CI/CD**: GitHub Actions
- **Format**: XML-based topics with semantic markup

## 📊 Build Status

The documentation is automatically built and deployed on every push to the main branch. The workflow includes:

- Building documentation with Writerside
- Validating content structure
- Deploying to GitHub Pages
- Uploading artifacts to external repositories

## 📝 License

This documentation is licensed under the MIT License. See [LICENSE](LICENSE) file for details.

## 🔗 Links

- **Main Plugin Repository**: [github.com/edgafner/dorkag](https://github.com/edgafner/dorkag)
- **Documentation Site**: [edgafner.github.io](https://edgafner.github.io)
- **JetBrains Marketplace**: [AZD](https://plugins.jetbrains.com/plugin/22319-azd),
  [GBrowser](https://plugins.jetbrains.com/plugin/14458-gbrowser),
  [JirAI](https://plugins.jetbrains.com/plugin/33954-jirai),
  [Codecov](https://plugins.jetbrains.com/plugin/23390-codecov),
  [QueryFlag](https://plugins.jetbrains.com/plugin/18269-queryflag)

## 👥 Support

- **Plugin issues**: [github.com/edgafner/dorkag/issues](https://github.com/edgafner/dorkag/issues)
- **Documentation issues**: [github.com/edgafner/edgafner.github.io/issues](https://github.com/edgafner/edgafner.github.io/issues)
- **Twitter**: [@Jongafner](https://twitter.com/Jongafner)
- **LinkedIn**: [Connect with us](https://www.linkedin.com/in/jonathan-gafner-3415974b/)
- **Blue sky**: [Contact us](https://bsky.app/profile/jgafner.bsky.social)

---

**Made with ❤️ by DORKAG Team**