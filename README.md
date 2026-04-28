# Code Break Predictor

Code Break Predictor is a Claude Code plugin for predicting where code changes are likely to break later, not just whether they pass today.

The goal is to surface the rare, extremely valuable failures that show up only when a system grows: pagination that collapses at 10k rows, queries that become bottlenecks under real traffic, abstractions that create long-term debt, and hidden coupling that slows future work.

## Why this exists

Most review workflows catch immediate defects. Far fewer catch future failure points while the code is still cheap to fix. This plugin is designed to make those risks visible during review.

## Plugin architecture

This repo uses a modular Claude Code layout:

- .claude-plugin/plugin.json: plugin metadata and file registration
- commands/predictor.md: the orchestrator command
- agents/break-analyzer.md: the specialist analysis agent

The flow is intentionally split into analyzer -> specialist agent layers so the orchestration stays simple and the deep reasoning stays focused.

1. A command starts the workflow.
2. The orchestrator hands the change set to the break-analyzer specialist.
3. The specialist evaluates scale risk, future debt, and failure mechanisms.
4. The orchestrator turns that into a concise decision-grade summary.

## File layout

.claude-plugin/
  plugin.json
commands/
  predictor.md
agents/
  break-analyzer.md
README.md

## How to use it

After installing this repository as a Claude Code plugin, run the predictor command against a git diff, commit, or changed files.

Typical inputs:

- a pull request diff
- a commit range
- one or more changed files
- a feature branch with surrounding context

The plugin will focus on questions like:

- Will this paginate poorly once the data set grows?
- Will this query pattern become expensive at scale?
- Will this caching approach produce stale or inconsistent behavior?
- Will this abstraction become technical debt in the next few iterations?

## Expected output

The analyzer should produce:

- what changed
- the most likely future failure points
- why those failures are likely
- confidence and severity
- concrete mitigation ideas or tests

## Extending the plugin

The modular structure is meant to grow. Additional specialist agents can be added later for areas like:

- performance profiling
- API contract review
- schema evolution risk
- frontend state and rendering bottlenecks
- operational and reliability review

The orchestrator can route to those specialists without changing the overall workflow.

## Running locally

1. Clone the repo.
2. Install it into your Claude Code plugin setup.
3. Invoke the predictor command on a change set.
4. Review the forecasted failure modes and suggested mitigations.

## Project description

AI-powered tool that analyzes code changes to predict future failure points and scalability bottlenecks.

## Tags

ai, software-engineering, predictive-analysis, code-quality, agentic-ai
