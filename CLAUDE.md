# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This is a greenfield project. There is no source code, `go.mod`, or git repository yet — only a project blueprint at `.claude/blueprint.md` and a set of custom slash commands at `.claude/commands/`. Treat the blueprint as the source of truth for what should be built.

## What is being built

A Personal Task Manager REST API in **Go + Gin + PostgreSQL + GORM**, following clean architecture (Controller → Service → Repository). Full spec — entity shape, validation rules, endpoints, response envelope, target folder layout — lives in `.claude/blueprint.md`. Read it before designing or generating code.

Key constraints from the blueprint that aren't obvious from the code (since none exists):
- **Layering is strict**: controllers must not touch the DB directly; only repositories use GORM. Services own business logic.
- **Validation**: `go-playground/validator` with the exact rules in the blueprint (e.g. `Title` min=3 max=100, `Status` `oneof=pending completed`).
- **Response envelope**: every response uses the `{success, message, data}` / `{success, message, error}` shape from the blueprint — keep it consistent across all handlers.
- **DB**: PostgreSQL database `task_manager`, schema managed via `db.AutoMigrate(&Task{})`.
- **Target layout** (from blueprint): `cmd/`, `config/`, `models/`, `controllers/`, `services/`, `repositories/`, `routes/`, `middleware/`, `utils/`. Match this when scaffolding.

The user is a software engineering student learning full-stack/backend — when explaining changes, favor showing how layers interact over terse diffs.

## Custom slash commands in this repo

These live in `.claude/commands/` and define the user's preferred workflow. Prefer them over ad-hoc equivalents:

- `/spec <short idea>` — creates a feature spec under `_specs/<slug>.md` and a `feature/<slug>` branch. Aborts on a dirty working tree. Note: the command references `.claude/commands/blue-print.md` and `.claude/skills/node/SKILL.md`, but the actual blueprint lives at `.claude/blueprint.md` and the `node` skill directory does not exist — warn and continue with what's available rather than failing.
- `/create-plan <description>` — saves the most recent in-conversation implementation plan to `_plan/YYYY-MM-DD-<slug>.md`. Its template includes Foundry verification steps (`forge build`, `forge test`); those are Solidity-specific and don't apply to this Go project — substitute Go equivalents (`go build ./...`, `go test ./...`, `go vet`, `gofmt -l .`) when filling out the verification checklist.
- `/create-sprints`, `/run-sprint`, `/commit-message` — sprint breakdown, sprint execution, and commit-message helpers respectively.

## Once code exists

The blueprint does not pin commands, so use the standard Go toolchain:
- Build: `go build ./...`
- Run API: `go run ./cmd`
- Test all: `go test ./...`
- Single test: `go test ./services -run TestTaskService_Create`
- Format / vet: `gofmt -w .` · `go vet ./...`

If/when a `Makefile`, `docker-compose.yml`, or `.env.example` lands, update this file to point at them.
