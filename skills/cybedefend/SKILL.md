---
name: cybedefend
description: One security check for the repository you are working in. Runs every scanner through CybeDefend, applies the team's security policies, fixes what blocks and says if the code can ship. Use when the user types /cybedefend, asks for a security check, or asks "can I ship this".
---

# CybeDefend

Work only on the repository open in this session. Never target a system the user does not own.

## /cybedefend

1. Run the six scanners on the current branch with their pinned images (Opengrep, Trivy fs, Gitleaks, Trivy config, Trivy image, zizmor).
2. Send each SARIF to the CybeDefend dashboard with `cybedefend import --format sarif --type <type> --branch <branch> --wait <file>`. The dashboard matches past scans of the branch, auto-resolves what is gone, reopens what is back and evaluates the security policies.
3. If no Block rule matches, print SHIP and stop.
4. Otherwise read each blocking finding over MCP (list and get vulnerabilities).
5. For each one, write a test that reproduces the flaw. If it does, fix the code until the test passes and keep the test in the suite. If it does not, set it as not exploitable with your reason (`set_exploitability`, `update_vulnerability`) and leave the decision to the user.
6. Run steps 1 and 2 again. Print SHIP when nothing blocks, otherwise the list of what is left and why.

## Rules

- The verdict comes from the team's policies, never from your own judgment alone.
- Tests stay in the user's test suite. No network calls to third-party systems.
- Never print a secret you found. Show its location and type only.
