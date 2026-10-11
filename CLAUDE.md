# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A recipe management application. Users photograph paper recipes, a vision model extracts structured data (title, prep time, instructions, ingredients), and recipes are stored in PostgreSQL. Users can also photograph fridge contents to get recipe suggestions via an agentic LLM pipeline.

## Tech Stack

- **Language**: Python 3.12+ (managed with [uv](https://docs.astral.sh/uv/))
- **Backend**: FastAPI (async)
- **Database**: PostgreSQL with SQLAlchemy (async via asyncpg)
- **LLM**: OpenAI Python SDK as unified interface, switching providers via `base_url` config:
  - Local: Ollama (`http://localhost:11434/v1`) with Qwen3.5:9b for dev/testing
  - Cloud: OpenRouter (`https://openrouter.ai/api/v1`) for OpenAI/Anthropic/other models
  - Direct: OpenAI API (default base URL)
- **Metrics**: Custom structured logging + DB tables for pipeline tracking (latency, cost, prompt versioning)
- **Deployment**: Docker Compose (PostgreSQL + FastAPI)
- **Frontend**: React.js (future, not in MVP)

## Architecture

**Agentic pipeline**: The LLM uses tool-calling to interact with the system — searching recipes, filtering by time/difficulty, refining suggestions based on user preferences (e.g., "something quicker", "more fancy").

Key flows:
1. **Recipe ingestion**: Photo -> vision model -> structured recipe data -> PostgreSQL
2. **Recipe discovery**: Fridge photo -> vision model -> ingredient list -> agentic LLM with DB query tools -> interactive recipe suggestions

## Commands

- **Run**: `uv run main.py`
- **Add dependency**: `uv add <package>`
- **Sync environment**: `uv sync`
