# DevHealth 🩺

> A unified CLI tool that audits your codebase health in a single command.

[![npm version](https://img.shields.io/npm/v/devhealth-cli)](https://www.npmjs.com/package/devhealth-cli)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)](https://nodejs.org/)

## The Problem

Developers currently stitch together **5+ fragmented tools** to understand their codebase health:

- `npm audit` for dependency vulnerabilities
- Codecov / SonarQube for test coverage
- ESLint / Prettier for code quality
- Custom scripts for outdated packages
- CI dashboards for build health

**There is no single standard.** Teams waste hours configuring, maintaining, and reconciling these tools. Junior developers never get a clear picture. Technical debt grows silently.

## The Solution

**DevHealth** is a zero-config, extensible CLI that unifies all codebase health signals into one command, one score, and one report.

```bash
npx devhealth-cli
plain
┌─────────────────────────────────────────┐
│  DevHealth Report — my-project          │
├─────────────────────────────────────────┤
│  Overall Score: 78/100 ⚠️              │
│                                         │
│  Dependencies    92/100  ✅             │
│  Security        45/100  🔴             │
│  Test Coverage   81/100  ✅             │
│  Code Complexity 65/100  ⚠️             │
│  Documentation   88/100  ✅             │
│  CI/CD Health    95/100  ✅             │
└─────────────────────────────────────────┘
Features
Core (v1.0)
[x] Zero-config scanning — Works out of the box on any repo
[x] Dependency audit — Outdated packages, vulnerability checks (via OSV)
[x] Security scan — Secret detection, insecure patterns
[x] Test coverage — Parse coverage reports (lcov, cobertura, json)
[x] Code complexity — Cyclomatic complexity, file length, nesting depth
[x] Documentation coverage — README presence, JSDoc/TSDoc coverage
[x] CI/CD health — GitHub Actions status, build duration trends
[x] Unified scoring — Weighted health score (0-100)
[x] Multiple output formats — Terminal table, JSON, HTML report, SARIF
Coming Soon
[ ] Language plugins — Python, Go, Rust support
[ ] GitHub Action — Auto-run on every PR
[ ] Trend tracking — Score history over time
[ ] Monorepo support — Per-package health reports
[ ] Custom rules — Define your own health checks via config
[ ] Team dashboards — Aggregate health across org repos
Architecture
plain
devhealth-cli/
├── packages/
│   ├── core/              # Engine, scoring, config
│   ├── cli/               # Terminal UI, command parsing
│   ├── reporter/          # Output formatters (table, json, html, sarif)
│   └── plugins/
│       ├── js/            # JS/TS specific checks
│       ├── python/        # (planned)
│       └── go/            # (planned)
├── rules/                 # Shared rule definitions
└── docs/
Plugin System
DevHealth is built around a plugin architecture. Each language/framework gets its own plugin that registers health checks with the core engine:
TypeScript
// Example: plugins/js/index.ts
import { definePlugin } from '@devhealth/core';

export default definePlugin({
  name: 'javascript',
  checks: [
    dependencyAudit(),
    securityScan(),
    testCoverage(),
    complexityCheck(),
    docCoverage(),
  ],
});
This makes it trivial for the community to add new language support.
Scoring Algorithm
Each check returns a normalized score (0-100). The overall score is a weighted composite based on project type and team priorities:
TypeScript
const weights = {
  dependencies: 0.20,
  security: 0.25,
  testCoverage: 0.20,
  complexity: 0.15,
  documentation: 0.10,
  ciHealth: 0.10,
};
Weights are fully customizable via devhealth.config.js.
Installation
bash
# Run without installing
npx devhealth-cli

# Or install globally
npm install -g devhealth-cli

# Or install as a dev dependency
npm install --save-dev devhealth-cli
Usage
bash
# Basic scan
devhealth

# Scan specific directory
devhealth --path ./packages/web

# Output as JSON
devhealth --format json --output report.json

# CI mode (non-interactive, exits with error code if score < threshold)
devhealth --ci --threshold 70

# Use custom config
devhealth --config ./devhealth.config.js
Configuration
Create a devhealth.config.js in your repo root:
JavaScript
export default {
  threshold: 70,           // Minimum score to pass CI
  weights: {
    security: 0.30,          // Prioritize security
    testCoverage: 0.25,
  },
  ignore: [
    '**/node_modules/**',
    '**/dist/**',
    '**/*.test.ts',         // Exclude test files from complexity
  ],
  plugins: ['@devhealth/plugin-js'],
};
Roadmap
Table
Milestone	Target	Status
v0.1.0 — Core engine + JS plugin	Week 1	🚧 In Progress
v0.2.0 — HTML/SARIF reporters	Week 2	📋 Planned
v0.3.0 — GitHub Action	Week 3	📋 Planned
v1.0.0 — Stable release	Week 4	📋 Planned
v1.1.0 — Python plugin	Month 2	📋 Planned
v1.2.0 — Go plugin	Month 3	📋 Planned
Contributing
We welcome contributors! Check out our Contributing Guide and Good First Issues.
Quick Start for Contributors
bash
git clone https://github.com/YOUR_USERNAME/devhealth-cli.git
cd devhealth-cli
npm install
npm run dev
Why Open Source?
Code health affects every developer, but most teams can't afford enterprise-grade tooling. DevHealth is MIT-licensed because:
Transparency — You should know exactly how your code is scored
Extensibility — Every team has different priorities; customize freely
Community — Language ecosystems evolve fast; plugins keep us current
License
MIT © Mahipal
