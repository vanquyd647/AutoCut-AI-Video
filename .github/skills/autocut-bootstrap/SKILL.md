---
name: autocut-bootstrap
description: "Use when scaffolding, extending, or refactoring the AutoCut AI Video project, including FastAPI backend work, Vite React frontend work, Gemini integration, FFmpeg editing flow, project storage, or upload/analyze/edit/export APIs."
---

# AutoCut Bootstrap

## Purpose

This skill keeps work aligned with the repository architecture for AutoCut AI Video.

## Architecture Rules

- Keep the backend in `backend/` with FastAPI entrypoints under `backend/api/` and domain logic under `backend/core/`.
- Keep API schemas in `backend/models/schemas.py` and preserve request/response compatibility with the frontend types.
- Keep the frontend in `frontend/` with reusable UI in `frontend/src/components/`, transport logic in `frontend/src/services/`, and shared types in `frontend/src/types/`.
- Accept the Gemini API key from the frontend request payload. Do not hardcode it.
- Prefer filesystem-backed project state under `backend/storage/` so uploads, analyses, plans, and exports survive process restarts.

## Backend Workflow

1. Save uploaded files into a per-project folder.
2. Persist project metadata before long-running work starts.
3. Emit progress updates for `upload`, `analyze`, `plan`, `render`, and `export` stages.
4. Keep Gemini access isolated in `backend/core/gemini_client.py`.
5. Keep FFmpeg execution isolated in `backend/core/video_editor.py`.

## Frontend Workflow

1. Use a three-step flow: upload, analyze, export.
2. Keep the selected style, aspect ratio, duration, and API key in local UI state.
3. Use `frontend/src/services/api.ts` for HTTP and WebSocket transport.
4. Render backend progress updates without duplicating server-side state logic in the client.

## Implementation Notes

- Treat Gemini responses as untrusted input and normalize JSON before validation.
- Keep video editing methods composable so no-op implementations can be replaced incrementally.
- Prefer small, verifiable additions over broad rewrites.
- When a feature is not fully implemented yet, keep the API contract stable and return deterministic fallback data instead of leaving the route unusable.

## Validation

- Validate Python syntax after backend edits.
- Validate the frontend with a Vite production build after UI or API-client edits.
- Update `README.md` when commands, ports, or setup steps change.