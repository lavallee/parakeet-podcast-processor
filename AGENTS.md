# Repository Guidelines

## Project Structure & Module Organization
- `p3/`: Core package (`cli.py`, `downloader.py`, `transcriber.py`, `cleaner.py`, `exporter.py`, `database.py`, `writer.py`). Entry point installs `p3` via `p3.cli:main`.
- `config/`: YAML config (`feeds.yaml.example` → copy to `feeds.yaml`).
- `data/`: Runtime state (`p3.duckdb`, `audio/`). Not for source control.
- `exports/` and `blog_posts/`: Generated digests and articles.
- `logs/`: Runtime logs; safe to ignore.
- `demo.py`: Example workflow runner.

## Build, Test, and Development Commands
- Install (dev): `pip install -e .[dev]`
- Initialize: `p3 init` (creates folders, DB, copies config)
- Fetch: `p3 fetch [--max-episodes N]`
- Transcribe: `p3 transcribe [--model base|small|…] [--episode-id ID]`
- Digest: `p3 digest [--provider openai|anthropic|ollama] [--model MODEL]`
- Export: `p3 export --date YYYY-MM-DD --format markdown --format json`
- Write: `p3 write --topic "Your Angle" [--date YYYY-MM-DD]`
- Quick demo: `python demo.py`

## Coding Style & Naming Conventions
- Python 3.9+. Use 4-space indents, type hints, and module/function `snake_case`; class `PascalCase`.
- Formatting: Black (88 cols) and isort (profile "black").
  - Format: `black . && isort .`
  - Types: `mypy p3`

## Testing Guidelines
- Framework: pytest. Place tests in `tests/` using `test_*.py`.
- Run tests: `pytest -q`
- Prefer small, deterministic units. Inject `P3Database` and use temp dirs for `data/` and `config/` when testing.

## Commit & Pull Request Guidelines
- Conventional Commits style (seen in history): `feat: …`, `docs: …`, `chore: …`.
- Commits: imperative mood, concise subject; add body for context when needed.
- PRs include: clear description, scope, CLI commands run (with sample output), linked issues, and any screenshots of generated artifacts.
- Pre-submit: `black .`, `isort .`, `mypy p3`, `pytest -q`.

## Security & Configuration Tips
- API keys: set via env vars (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`) when not using Ollama.
- Do not commit `config/feeds.yaml`, database files, audio, or logs. Use `feeds.yaml.example` as the template.
- Large audio processing can be resource intensive; test with small episode limits (`--max-episodes 1`).
