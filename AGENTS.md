# Repository Guidelines

## Initial Agent Workflow
- Ask the repository owner to clarify the repository's purpose and identify which documentation or institutional knowledge needs to be collected before starting other tasks.
- Use Crawl4AI to scrape the requested documentation sources and save the gathered references in this project (e.g., under `knowledge/` or `docs/`) with clear source attribution.
- Establish and document a strategy for keeping the collected knowledge resources up to date, including check-in cadence, tooling, and responsible parties.
- Stage a commit with the onboarding work and push it to the configured remote; if no remote exists, ask the owner to set one up before proceeding.

## Project Structure & Module Organization
- Workflows live under `Alfred.alfredpreferences/workflows/user.workflow.<UUID>/`; keep the UUID directory names and co-locate scripts (`media.py`, `openurls.py`), `info.plist`, icons, and the workflow README.
- Vendored assets such as `mako/`, `templates/`, gifs, or custom HTML stay inside the workflow directory; update `info.plist` when adding new files so Alfred sees them.
- `resources/` holds PNGs for custom web searches. `themes/` and `preferences/` mirror Alfred UI settings, while `clipboard/` and `remote/` contain binary exports—avoid hand-editing and strip personal content before committing.

## Build, Test, and Development Commands
- `plutil -lint Alfred.alfredpreferences/workflows/<uuid>/info.plist` validates workflow metadata.
- `python3 Alfred.alfredpreferences/workflows/<uuid>/<script>.py -t "movie" -q "Inception"` (adjust args as needed) smoke-tests script filters outside Alfred.
- `python3 -m compileall Alfred.alfredpreferences/workflows/<uuid>` catches syntax errors; package shareable builds with `ditto -c -k --keepParent <dir> <name>.alfredworkflow`.

## Coding Style & Naming Conventions
- Target Python 3 with four-space indentation, `snake_case` identifiers, and guard entry points with `if __name__ == "__main__":`.
- Document Alfred variables (e.g., `include_letterboxd`) in each workflow README and keep defaults in `info.plist` scrubbed of private data or API keys.
- Use 512×512 `icon.png` files; place supplementary imagery in `img/` or `templates/` and reference them relatively from `info.plist`.

## Testing Guidelines
- Test new logic both via the CLI command above and through Alfred using the keywords defined in `info.plist` (e.g., `movie`, `tv`); note API prerequisites and sample queries.
- External API workflows should run with personal keys during development but commit only placeholder values, backed by setup steps in the README.
- There is no CI; record manual verification notes or short gifs in the workflow README.

## Commit & Pull Request Guidelines
- Write imperative commit subjects (`Add OMDb key prompt`) and mention affected workflow UUIDs when clarity helps; split unrelated changes.
- Pull requests need a workflow summary, manual test evidence, and updated screenshots or gifs when the Alfred UI changes.
- Flag any bundled dependencies and explain why they are vendorized instead of installed dynamically.

## Security & Configuration Tips
- Delete Finder-generated duplicates such as `info (1).plist` before committing to keep the tree deterministic.
- Never commit snippet databases, cached HTML, or secrets; instead document how to regenerate them locally.
- After edits, export the workflow (`File > Export > Workflow`) and confirm the archive matches the directory so Alfred sync stays healthy.
