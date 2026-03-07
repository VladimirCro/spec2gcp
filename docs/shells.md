# Shell baselines (Greenfield)

Spec2GCP supports a "shell-first" greenfield approach: start from a predefined baseline ("shell"), then use coding agents to translate natural language requirements into the missing code, wiring, and configuration.

## Available shells

- Agentic Python shell: https://github.com/EmeaAppGbb/agentic-shell-python
- More GCP-specific shells can be created using spec2gcp as the base

## How to use shells with agents

1. Pick a shell repo that matches your language/runtime.
2. Add your requirements in plain language (and/or run the Spec2GCP prompts).
3. Let agents iteratively implement the gaps: endpoints, UI, storage, tests, and deployment.

Back to [docs index](index.md).
