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

Seamlessly integrate browser functionality into your development workflow.

### [Codecov - Code Coverage](https://edgafner.github.io/codecov.html)

Visualize and track code coverage directly in your IDE.

### [QueryFlag - Query Management](https://edgafner.github.io/queryflag.html)

Efficient query management and execution tools.

### [RunLikeMe - Run Configuration Management](https://edgafner.github.io/runlikeme.html)

Share and manage run configurations across your team.

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

# Documentation is built with Writerside
# View locally using Writerside IDE plugin or Docker
docker run --rm -v $PWD:/opt/sources jetbrains/writerside-builder:241.18775 /opt/builder.sh
```

## 📖 Documentation Structure

```
Dorkag/
├── topics/           # Documentation content (.topic XML files)
│   ├── azd/         # AZD plugin documentation
│   ├── gbrowser/    # GBrowser plugin documentation
│   ├── codecov/     # Codecov plugin documentation
│   ├── queryflag/   # QueryFlag plugin documentation
│   └── runlikeme/   # RunLikeMe plugin documentation
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
- **JetBrains Marketplace**: [AZD Plugin](https://plugins.jetbrains.com/plugin/22319-azd)

## 👥 Support

- **Issues**: [GitHub Issues](https://github.com/edgafner/edgafner.github.io/issues)
- **Twitter**: [@Jongafner](https://twitter.com/Jongafner)
- **LinkedIn**: [Connect with us](https://www.linkedin.com/in/jonathan-gafner-3415974b/)
- **Blue sky**: [Contact us](https://bsky.app/profile/jgafner.bsky.social)

---

**Made with ❤️ by DORKAG Team**