# CHANGELOG.md

---

## Phase 1 — Foundation
- Initialized Node.js + Express project with ESM modules
- Set up folder structure: config, db, middleware, utils, modules, jobs, docs
- Implemented unified response format (sendSuccess / sendError)
- Implemented traceId middleware — UUID generated per request, attached to logs and responses
- Implemented Winston structured logger
- Implemented global error handler
- Added /health endpoint

## Phase 2 — Database Schema
- Designed and migrated all Prisma models: User, Meeting, TranscriptChunk, ContextLog, Analysis, ActionItem, ReminderLog
- Chose SQLite for zero-dependency local and deployed database

## Phase 3 — Auth
- Implemented register and login endpoints
- Passwords hashed with bcryptjs
- JWT issued on login, verified via auth middleware

## Phase 4 — Meetings
- Create, get, list meetings
- Pagination on list endpoint
- Participants stored as JSON array

## Phase 5 — Live Transcript System
- Implemented live buffer using sentinel chunkIndex (-1)
- Chunk flush triggers at TRANSCRIPT_CHUNK_SIZE lines
- Each flush: archives raw chunk, calls AI for summary, writes phased context log entry
- Context log carries phase time ranges and action/decision signals

## Phase 6 — AI Analysis
- Groq integration via fetch (OpenAI-compatible endpoint)
- Two prompts: chunk summary and full meeting analysis
- Two-tier citation system: live (exact timestamp) and phase (time range)
- Citation validation logs warnings on missing citations
- Analysis stored in DB, action items auto-created from AI output

## Phase 7 — Action Items
- Create, list, update status endpoints
- Filtering by status, assignee, meetingId
- Overdue detection endpoint

## Phase 8 — Reminder Scheduler
- node-cron job running hourly
- Resend email integration
- 24-hour deduplication window
- ReminderLog records every send attempt with status

## Phase 9 — Docs + Wiring
- Swagger/OpenAPI setup at /api/docs
- Evaluation endpoint at /api/evaluation
- All routes wired in app.js
- README, DECISIONS, AI_APPROACH, TESTING, CHANGELOG, CHECKLIST written

## Phase 10 — Polish & Fixes
- Fixed route order bug: GET /action-items/overdue moved before /:id to prevent Express path collision
- Fixed reminder deduplication query: replaced vacuous-truth `every` clause with `NOT { some }` pattern
- Added optional transcript seeding on POST /meetings to match spec example request
- Added full OpenAPI JSDoc annotations to all route files — Swagger now documents all endpoints
- Fixed traceId middleware to log response statusCode on res.finish (spec requirement)
- Standardized all logger calls to structured objects (no more plain strings)
- Updated GROQ_MODEL from decommissioned llama3-70b-8192 to llama-3.3-70b-versatile
- Updated DECISIONS.md and AI_APPROACH.md to reflect model change
- Added unit tests: chunker appendLines logic + overdue detection predicate
- Updated TESTING.md with full manual test scenario documentation
