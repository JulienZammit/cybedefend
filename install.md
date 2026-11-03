# Install CybeDefend (instructions for coding agents)

You are a coding agent setting up CybeDefend for the repository open in this session. Follow the steps in order. Before each step, tell the user what you are about to do and wait for a yes. Never type a password for the user and never print a secret or a token you see.

## 1. Install the skill

Run `npx skills add CybeDefend/cybedefend`. This adds the `/cybedefend` command to your agent.

## 2. Start the dashboard

Ask the user where the dashboard should run: this machine (default) or a server they name.

1. Check that Docker is running (`docker info`). If it is not, stop and ask the user to start it.
2. Clone the deploy folder: `git clone --depth 1 https://github.com/CybeDefend/cybedefend ~/.cybedefend && cd ~/.cybedefend/deploy`.
3. `cp .env.example .env`, then `docker compose up -d`. Wait until every container reports healthy.
4. Tell the user to open the dashboard URL printed by the compose output, create their admin account, then create a personal access token in their account settings. Ask them to paste the token back to you. Store it only as `CYBEDEFEND_TOKEN` in the agent's MCP configuration and, if they agree, as a CI secret.

## 3. Connect the MCP server

Add the CybeDefend MCP server to your own MCP configuration with the dashboard URL and the header `Authorization: Bearer <CYBEDEFEND_TOKEN>`. Check the connection with the `list_organizations` tool.

## 4. Create the project

Use `create_project` for this repository, in the team the user picks.

## 5. Add the scanners to CI

GitHub: propose a workflow that runs `CybeDefend/appsec-scanners@v1` once per scanner (`opengrep`, `gitleaks`, `trivy-fs`, `trivy-config`, `trivy-image` if the repo builds an image, `zizmor`) with `token: ${{ secrets.CYBEDEFEND_TOKEN }}`. GitLab: propose the CybeDefend GitLab CI template. Show the file to the user before writing it. Do not commit unless they ask.

## 6. Run the first check

Run `/cybedefend` and show the user the result and the dashboard link.
