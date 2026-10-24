# Fellowdock

An open-source, self-hosted communication platform with a multi-server client.

> **Status:** planning / initial development. This repository does not yet contain a working application.

## Vision

Fellowdock is a Discord-inspired platform where each community can operate an independent server. A single client can connect to multiple server URLs and keep them organized in one interface. Server owners retain control of their own accounts, communities and data.

## Initial principles

- **Independent servers:** No mandatory central Fellowdock service.
- **Multi-server client:** Connect to and switch between multiple servers from one application.
- **Self-hosting first:** Docker Compose deployment with clear documentation.
- **Open protocol:** Versioned API and WebSocket event schemas.
- **Secure by default:** TLS for remote connections, password hashing, permissions, rate limiting, sensible upload limits and no hard-coded secrets.
- **Incremental delivery:** Functional vertical slices, tested before adding features.

## Proposed stack

- Frontend: React, TypeScript, Vite, installable PWA (web-first)
- Desktop: Deferred; no native desktop app in initial releases
- Backend: Node.js, TypeScript, Fastify, WebSocket
- Database: PostgreSQL
- Voice/video (later): WebRTC with a self-hostable SFU such as LiveKit
- Deployment: Ubuntu Server via Docker Compose; Umbrel via packaged app using the same images
- Automation: GitHub Actions

These are architectural starting choices, not final commitments.

## Roadmap

### Phase 1: Foundation
- Monorepo and code quality setup
- Shared protocol schemas and versioned API
- Backend health endpoint and database migrations
- Web client with server URL management and connection diagnostics
- Docker Compose for local development

### Phase 2: First usable chat
- Server-local registration and login
- Communities and text channels
- Realtime messages, message history and basic moderation
- Roles and permission enforcement
- Attachments with file type/size restrictions

### Phase 3: Web client polish and release pipeline
- Installable PWA and cross-browser/mobile testing
- Notifications, unread counts, search, profiles and settings
- Multi-server session storage, reconnect behavior and accessibility

### Phase 4: Voice and community features
- Voice channels and optional video/screen sharing
- Invite links, moderation tools, backups and administrative controls
- Load tests, security review and upgrade documentation

## Planned structure

```text
apps/
  web/        # Browser client
  desktop/    # Optional future Tauri wrapper
  server/     # Self-hosted backend
packages/
  protocol/   # Versioned schemas and shared types
  ui/         # Reusable client components
deploy/
  docker/     # Ubuntu self-hosting manifests
  umbrel/     # Umbrel app packaging
docs/         # Architecture, setup and security
.github/
  workflows/  # CI and release workflows
```

## Architecture notes

The client maintains separate authenticated sessions for each server. Server URLs and credentials must be stored safely. Servers are independent, so there is **no global user account or cross-server messaging in the first release**. Those features would require a separately designed identity/federation model.

Do not expose an unprotected server to the public internet during early development.

## Contributing

The initial codebase is being designed. Contribution guidelines and a license will be added before public releases.

## Releases and automatic updates

- GitHub Actions will build tested, multi-architecture OCI images and publish versioned releases to GHCR.
- Web clients will fetch new immutable assets on refresh; a service worker can show an update-ready banner and request user refresh when needed.
- Ubuntu deployments will support *opt-in* automatic updates with image pinning, health checks, pre-migration backups, monitoring and documented rollback. Do not auto-apply untested major versions or blindly use `latest`.
- Umbrel app-store installations will follow Umbrel's official update flow: Umbrel does **not** automatically install app updates. A separately managed Docker stack may optionally auto-update but must not be advertised as native Umbrel app-store behavior.
- PostgreSQL data remains in durable volumes. Backups and forward-compatible database migrations are required before unattended upgrades are enabled.
