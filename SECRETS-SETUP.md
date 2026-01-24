# GitHub Secrets Setup

This document describes the required GitHub secrets for this repository.

## Required Secrets

### 1. `ALGOLIA_KEY`
**Purpose**: API key for Algolia search indexing
**Where to get it**: 
- Log in to [Algolia Dashboard](https://www.algolia.com/apps/0ZSRMCDHQ9/dashboard)
- Go to Settings → API Keys
- Copy the "Search API Key" (for frontend) or "Admin API Key" (for indexing)
**Used in**: 
- `build-docs.yml` - Passed to Writerside builder and Algolia indexer

### 2. `DORKA_UPLOAD_TOKEN` (Optional)
**Purpose**: Personal Access Token for uploading documentation to dorka repository
**How to create**:
1. Go to GitHub Settings → Developer settings → Personal access tokens
2. Generate new token (classic) with `repo` scope
3. Copy the token value
**Used in**: 
- `build-docs.yml` - Upload documentation artifacts to dorka releases

## How to Add Secrets

1. Go to repository Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Add each secret with its name and value

## Usage in Workflows

The Algolia API key is used in two ways:

1. **During Documentation Build**: 
   - Passed as environment variable to Writerside Docker builder
   - Writerside replaces `${ALGOLIA_KEY}` in `buildprofiles.xml` during build

2. **During Index Update**:
   - Used by the Algolia publisher to update search indexes

## Security Notes

- Never commit API keys directly in code
- The `buildprofiles.xml` uses `${ALGOLIA_KEY}` placeholder which is replaced at build time
- Secrets are masked in GitHub Actions logs
- Use minimal required permissions for tokens