---
name: Issue Triage
description: Analyze new issues, add labels, and flag anything needing maintainer attention.
on:
  issues:
    types: [opened, reopened]
permissions:
  contents: read
  actions: read
  issues: read
engine: copilot
strict: true
timeout-minutes: 5
network:
  allowed: [defaults, github]
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
  bash: [cat, grep]
safe-outputs:
  add-labels:
    allowed: [bug, enhancement, documentation, question, housekeeping, needs-info, good-first-issue]
  add-comment:
  close-issue:
---

# Issue Triage

Analyze the issue for ${{ github.repository }} (issue #${{ github.event.issue.number }}).

Content: "${{ steps.sanitized.outputs.text }}"

1. **Classify the issue** using these rules:
   - If it reports a bug (app crashes, broken behavior, errors): add `bug`
   - If it requests a new feature or improvement: add `enhancement`
   - If it's about documentation, README, or examples: add `documentation`
   - If it's a question or asks how to do something: add `question`
   - If it's about repo maintenance, housekeeping, or non-code issues: add `housekeeping`
   - If the issue lacks enough detail to act on: add `needs-info` and ask the author to provide more information

2. **Add relevant labels** based on classification. Add `good-first-issue` if the task is straightforward and well-defined.

3. **If the issue is unclear or incomplete** (e.g., references files that don't exist, has no reproduction steps), add `needs-info` and post a polite comment asking for clarification.

4. **Do NOT close the issue** unless it is clearly spam or a duplicate.

5. Keep comments concise and actionable.
