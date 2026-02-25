# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an **Umbrel Community App Store** repository — a collection of YAML and Docker Compose files that package apps for distribution on [umbrelOS](https://umbrel.com). There is no build system, runtime code, or test suite.

## Repository Structure

- `umbrel-app-store.yml` — Defines the app store `id` (used as a prefix for all app IDs) and `name`.
- `<app-id>/umbrel-app.yml` — App listing metadata shown in the umbrelOS UI (name, description, icon, port, etc.).
- `<app-id>/docker-compose.yml` — Docker services that run the app on the device.

## Key Conventions

**App ID naming:** Every app ID must start with the app store ID from `umbrel-app-store.yml`. Example: store ID `poste` → app ID `poste` → folder name `poste/`.

**`docker-compose.yml` structure:**
- Always include an `app_proxy` service with `APP_HOST` set to `<app-id>_<service-name>_1` and `APP_PORT` matching the service's internal port.
- Services should run as a non-root user (`user: "1000:1000"`), unless the app explicitly requires root (e.g. mail servers like poste.io that manage internal daemons).
- For user-configurable settings (e.g. hostname, timezone), use environment variables with defaults in `docker-compose.yml` (`${VAR:-default}`) and provide a `.env.example` file.

**`umbrel-app.yml` fields:**
- `manifestVersion: 1` is required.
- `port` must match `APP_PORT` in `docker-compose.yml`.
- `id` must match the folder name exactly.
- `gallery` requires at least one image URL.

## Adding a New App

1. Update `umbrel-app-store.yml` with the desired store `id` and `name`.
2. Create a folder named `<store-id>-<app-name>/`.
3. Add `umbrel-app.yml` with all required fields.
4. Add `docker-compose.yml` with `app_proxy` and the app's service(s).

## Testing / Validation

There are no automated tests. To validate, add the GitHub repository URL as a Community App Store in umbrelOS. Refer to the [Umbrel App Store guidelines](https://github.com/getumbrel/umbrel-apps) for submission requirements if publishing to the official store.
