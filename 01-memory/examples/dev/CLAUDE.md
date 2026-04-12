# Inventory API

A REST API for managing warehouse inventory, built with FastAPI and PostgreSQL.

## Tech Stack

- Python 3.12, FastAPI, SQLAlchemy
- PostgreSQL 16 with Alembic migrations
- pytest for testing

## Commands

- Run server: `uvicorn app.main:app --reload`
- Run tests: `pytest -v`
- Run linter: `ruff check .`
- New migration: `alembic revision --autogenerate -m "description"`

## Rules

- Always run `pytest -v` before suggesting a commit
- Use type hints on all function signatures
- Follow existing patterns in app/routers/ for new endpoints
- Never modify alembic/versions/ files directly
