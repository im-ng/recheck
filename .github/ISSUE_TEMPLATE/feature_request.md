name: Feature request
about: Suggest an improvement or new test for recheck
title: "[Feature] "
labels: enhancement
assignees: ""
---

**Is your feature request related to a problem? Please describe.**
A clear and concise description of what the problem is. E.g. "I wish I could verify X before buying."

**Proposed solution**
What you'd like to happen. Describe the test, check, or UI change.

**Browser/OS feasibility check**
GitHub Pages + a single static HTML file means everything must run client-side in a browser:
- Is the data actually exposed by a web API (e.g. WebGL, Battery Status, Web Worker)?
- Or does it require the OS terminal (in which case an Info-dialog command, like the existing Battery/CPU/RAM ones, may be the right approach)?
- Note: anything needing a backend, network calls, or native access can't be added (privacy/offline promise).

**Alternatives considered**
Any alternative solutions or features you've considered.

**Additional context**
Add any other context, mockups, or references here.
