# Generative AI Project Template

This repository is the starting point for new agent-based engagements. Launch a fresh GitHub repo from this template, then follow the Quickstart below to get Crawl4AI knowledge capture and agent scaffolding in place.

## Quickstart
1. **Create a repo from the template**  
   Use GitHub’s “Use this template” ➜ “Create a new repository.” Configure visibility, branch protections, and project metadata before anyone clones the repo.  
   → See `docs/bootstrap.md#step-1--create-a-repository-from-the-template` for launch checklist items.
2. **Clone locally and enable Crawl4AI**  
   Clone your new repo, survey the workspace, and spin up Python 3.11 tooling plus Crawl4AI/Playwright inside a dedicated virtual environment. Seed `knowledge/` using `scripts/crawl_docs.py`.  
   → Follow `docs/bootstrap.md#step-2--clone-locally-and-stand-up-crawl4ai`.
3. **Design the agent architecture**  
   Document the initial agent roles in `AGENTS.md`, capture research in `knowledge/`, and set up evaluation loops so the system can evolve safely.  
   → Reference `docs/bootstrap.md#step-3--design-the-agent-architecture` for best practices.

## Repository Highlights
- `docs/bootstrap.md` — full bootstrap guide for template adopters.
- `AGENTS.md` — living design document for the agent network and responsibilities.
- `knowledge/` — structured storage for scraped docs, metadata, and research artifacts.
- `scripts/` — utilities for Crawl4AI workflows and other automation helpers.
- `templates/` — reusable snippets and scaffolds (e.g., `templates/genai-project/`).

When you customise this template for a project, update this README with context about the engagement, environment setup, and deployment targets so new collaborators can ramp quickly.

## Co-Authors

This project is developed collaboratively with AI assistance. Commit attribution follows the workspace conventions in `/Users/mriechers/Developer/workspace_ops/conventions/COMMIT_CONVENTIONS.md`.

| Agent | Role | Recent Commits |
|-------|------|----------------|
| Main Assistant | Template evolution and automation | `git log --grep="Agent: Main Assistant"` |
| code-reviewer | Code review and guardrails | `git log --grep="Agent: code-reviewer"` |

Run `git log --grep="Agent:" --oneline` to see the full agent history for this repository. See the workspace conventions document for additional filters.
