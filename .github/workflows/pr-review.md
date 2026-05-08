---
name: Pull Request Review
description: Review pull requests, check for common issues, and provide feedback.
on:
  pull_request:
    types: [opened, reopened, synchronize]
permissions:
  contents: read
  actions: read
  pull-requests: read
engine: copilot
strict: true
timeout-minutes: 10
network:
  allowed: [defaults, github]
tools:
  github:
    mode: gh-proxy
    toolsets: [default]
  bash: [cat, grep, wc, head]
safe-outputs:
  add-labels:
    allowed: [bug, enhancement, documentation, needs-review, ready-to-merge]
  add-comment:
  request-changes:
---

# Pull Request Review

Review the pull request for ${{ github.repository }} (PR #${{ github.event.pull_request.number }}).

Title: "${{ github.event.pull_request.title }}"
Body: "${{ steps.sanitized.outputs.text }}"

1. **Check the PR title** — it should be descriptive and follow conventional commit style (e.g., "fix: resolve X", "add: new feature Y", "docs: update README").

2. **Review the changes** for:
   - **Correctness**: Does the code work as intended?
   - **Style**: Is the code consistent with the existing project style? (This is a single-file HTML app — keep changes minimal and consistent)
   - **Documentation**: Are README or comments updated if needed?
   - **No regressions**: Does the change break existing functionality?

3. **For this specific project**, remember:
   - Everything lives in `index.html` (CSS and JS are embedded)
   - No build tools, no frameworks, no external dependencies
   - Changes should be practical and tested

4. **Add a comment** with your review:
   - If the PR is good: approve and add `ready-to-merge`
   - If there are issues: explain what needs fixing and add `needs-review`
   - If it's a documentation-only change: note that it looks good

5. **Do NOT merge the PR** — leave that to the maintainer.
