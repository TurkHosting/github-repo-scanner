# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file bash script (`gh-dep-scanner.sh`) that scans GitHub repositories (org or personal) to find which repos use a specific package below a given version. Supports Composer (PHP) and Node (JavaScript) dependencies.

## Requirements

- `gh` (GitHub CLI) - must be authenticated
- `jq` (JSON parser)

## Running the Script

```bash
# Interactive mode
./gh-dep-scanner.sh

# With CLI flags
./gh-dep-scanner.sh --url "https://github.com/orgs/ORG_NAME/repositories" \
                    --type composer \
                    --package livewire/livewire \
                    --below 4

# Show help
./gh-dep-scanner.sh --help
```

## Script Architecture

The script is organized into sections marked by comment blocks:

1. **HELPER FUNCTIONS** - Color output utilities (`print_error`, `print_success`, etc.)
2. **PREREQUISITES CHECK** - Verifies `gh` and `jq` are installed
3. **ARGUMENT PARSING** - Handles `--url`, `--type`, `--package`, `--below`, `--branch`, `--delay` flags
4. **INTERACTIVE MODE** - Prompts user for missing required inputs
5. **URL PARSING** - Extracts owner/org from various GitHub URL formats
6. **REPOSITORY FETCHING** - Uses `gh repo list` to get all repos
7. **DEPENDENCY FILE FETCHING** - Fetches `composer.json` or `package.json` via GitHub API
8. **VERSION EXTRACTION** - Parses package version from dependency file using `jq`
9. **VERSION COMPARISON** - Extracts major version from constraints (`^3.2` → `3`)
10. **MAIN SCANNING LOGIC** - Iterates repos with rate limiting
11. **OUTPUT** - Colored table output with summary

## Key Functions

- `get_major_version()` - Parses version constraints (handles `^`, `~`, `>=`, OR constraints, etc.)
- `fetch_dependency_file()` - Returns base64-decoded file content or `NOT_FOUND`/`PARSE_ERROR`
- `extract_version()` - Uses `jq` to extract package version from require/dependencies

## Commit Guidelines

Do not include AI-generated signatures or co-author attributions in commits.
