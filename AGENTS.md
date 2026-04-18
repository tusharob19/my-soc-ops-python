# AI Agent Guide: Soc Ops

## Development Checklist (Mandatory Before Commit)

- [ ] `python -m uv run ruff check .` — lint passes
- [ ] `python -m uv run pytest` — all 25 tests pass
- [ ] Dev server starts: `python -m uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000`

---

## Project Overview

**Social Bingo** game for in-person mixers. FastAPI backend + Jinja2 templates + HTMX frontend (no page refreshes).

- **Python**: 3.13+
- **Key file**: [app/main.py](app/main.py) — all routes & session management
- **Tests**: 25 passing tests in [tests/](tests/)

## Quick Setup

```bash
python -m uv sync      # Install dependencies
python -m uv run pytest  # Run tests
python -m uv run ruff check .  # Lint
python -m uv run uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

> Note: Use `python -m uv run` — `uv` is not in PATH on Windows

## Project Structure

```
app/main.py           # FastAPI routes & sessions
app/game_service.py   # GameSession class
app/game_logic.py     # Board generation, marking, win detection
app/models.py         # Pydantic models (GameState, BingoSquareData, BingoLine)
templates/            # Jinja2 templates + HTMX components
tests/                # Pytest tests (game logic + endpoints)
```

## Architecture

### Game State
- `GameSession` — individual player state (in-memory)
- `GameState` enum — `HOME`, `PLAYING`, `WON`
- [app/game_service.py](app/game_service.py)

### Bingo Logic
- `generate_board()` — 5x5 board (24 random questions + free center)
- `toggle_square()` — immutable marking (returns new list)
- `check_win()` — detects 5-in-a-row

### HTMX Frontend
- No full page reloads — forms use `hx-post`/`hx-get`
- Server returns HTML fragments, HTMX swaps in place
- **Must test in real browser** (not Simple Browser)

## Code Conventions

- **Linter**: Ruff (E, F, I, N, W rules; 88-char lines)
- **Types**: Pydantic models + type hints
- **Tests**: Pytest; test files mirror source with `test_` prefix
- **CSS**: Utility classes in [static/css/app.css](app/static/css/app.css)

## Common Tasks

| Task | Steps |
|------|-------|
| **Add endpoint** | 1. Add route in [app/main.py](app/main.py) 2. Return `HTMLResponse` 3. Add test in [tests/test_api.py](tests/test_api.py) 4. `pytest` |
| **Modify game logic** | 1. Edit [app/game_logic.py](app/game_logic.py) 2. Update [tests/test_game_logic.py](tests/test_game_logic.py) 3. `pytest` |
| **UI changes** | 1. Edit [templates/](templates/) 2. Dev server auto-reloads 3. Test in browser |

## Key Pitfalls

1. **No Simple Browser** — HTMX requires full browser
2. **Use `python -m uv run`** — not bare `pytest` or `uvicorn`
3. **Immutable state** — don't mutate game boards; return new lists
4. **Session isolation** — each user gets unique session ID
5. **Template paths** — relative to [app/templates/](app/templates/)

## Documentation

- [README.md](README.md) — project overview
- [workshop/](workshop/) — 5 progressive lab modules (EN/PT/ES)
- [.github/instructions/](​.github/instructions/) — CSS utilities, frontend design, general rules
- [.github/agents/](​.github/agents/) — TDD, UI review, pixel jam agents
- [CONTRIBUTING.md](CONTRIBUTING.md) — contribution guidelines

---

Last updated: 2026-04-18
