# Code Break Predictor

A Claude Code plugin that analyzes changes to predict future failure points and scalability bottlenecks.

It is designed to surface the rare failures that only show up as a system grows: pagination that collapses at 10k rows, queries that become bottlenecks under real traffic, abstractions that create long-term debt, and hidden coupling that slows future work.

## Why this exists

Most review workflows catch immediate defects. Far fewer catch future failure points while the code is still cheap to fix. This plugin is designed to make those risks visible during review.

## What it looks for

- Pagination and list handling that may fail at scale
- Query patterns that will become expensive under real traffic
- Abstractions that create long-term technical debt
- Caching and consistency issues that may appear later
- Hidden coupling that can slow future changes
- Reliability and operational risks introduced by the change

## Install

```bash
/plugin marketplace add fenil210/code-break-predictor
/plugin install code-break-predictor@code-break-predictor
```

## How to Use

1. Open Claude Code in the root of the project you want to review.
2. Install this plugin using the commands above.
3. Run the predictor command on a diff, commit, branch, or changed files:

```bash
/predictor
```

4. Provide the change set you want analyzed.
5. Review the forecasted failure modes, scalability risks, and mitigation ideas that Claude Code returns.

Typical inputs:

- a pull request diff
- a commit range
- one or more changed files
- a feature branch with surrounding context

## How it works

1. A `break-analyzer` agent scans the change set and identifies likely future failure points.
2. The command layer keeps the workflow simple and focused on the change being reviewed.
3. The analysis emphasizes scale risk, reliability risk, hidden coupling, and technical debt.
4. The final output is a concise, decision-grade summary with concrete mitigations or tests.

## Expected output

The analyzer should produce:

- what changed
- the most likely future failure points
- why those failures are likely
- confidence and severity
- concrete mitigation ideas or tests

## Plugin structure

```
code-break-predictor/
├── .claude-plugin/
│   └── plugin.json         # manifest
├── commands/
│   └── predictor.md        # /predictor orchestrator
└── agents/
    └── break-analyzer.md   # future-break analysis specialist
```

## Extending the plugin

The modular structure is meant to grow. Additional specialist agents can be added later for areas like:

- performance profiling
- API contract review
- schema evolution risk
- frontend state and rendering bottlenecks
- operational and reliability review

The orchestrator can route to those specialists without changing the overall workflow.

## Requirements

- Claude Code latest version
- Run from your project root directory