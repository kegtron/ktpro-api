# Kegtron Pro API Specification

Customer-facing REST API specification for [Kegtron Pro](https://kegtron.com/pro) keg monitors, published at **[api-docs.kegtron.com](http://api-docs.kegtron.com)**.

## Overview

The spec is written in [OpenAPI 3.0](https://swagger.io/specification/) and hosted via [Stoplight](https://stoplight.io). It documents the full customer API for interacting with Kegtron Pro devices — reading device state, updating configuration, issuing commands via RPC, querying historical data, and receiving real-time notifications.

## Repo Structure

```
reference/
  kegtron-pro.v2.yaml   # OpenAPI 3.0 specification (main file)
docs/
  Intro.md              # Getting started guide
  Notify.md             # WebSocket notifications reference
```

## API Summary

| Endpoint | Description |
|----------|-------------|
| `GET /customer` | Retrieve account and device list |
| `POST /customer` | Update account settings |
| `GET /api/v2/m/device` | Read device shadow (config + status) |
| `POST /api/v2/m/device` | Write device configuration |
| `POST /api/v2/m/device/rpc/*` | Issue device commands (reset volume, clean mode, sensor reads, etc.) |
| `POST /api/v2/m/device/data/*` | Query historical servings, alerts, and telemetry |
| `wss://mdash.net/api/v2/m/device/notify` | Real-time device notifications |

All endpoints are hosted at `https://mdash.net` and authenticated via an `access_token` query parameter.

## Editing

The spec is maintained in this repo and synced to Stoplight automatically on push to `master`. The recommended editor is [Stoplight Studio](https://stoplight.io/studio).

To lint locally:

```bash
npx @stoplight/spectral-cli lint reference/kegtron-pro.v2.yaml
```

## Contact

[support@kegtron.com](mailto:support@kegtron.com) · [kegtron.com](https://kegtron.com)
