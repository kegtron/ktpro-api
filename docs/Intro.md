# Introduction

Kegtron Pro uses a REST API to interact with keg monitor hardware ("devices"). All endpoints are hosted at `https://mdash.net`.

## Authentication

All API calls are authenticated via an `access_token` query parameter. There are two token types:

| Token | How to obtain | Used for |
|-------|--------------|----------|
| **CUSTOMER_KEY** | HTTP Basic Auth on the Customer GET endpoint | Customer-level endpoints |
| **DEVICE_PUBLIC_KEY** | Returned in the `pubkeys` map of the Customer object | All device endpoints |

## Getting Started

Interacting with devices is a two-step process:

1. Call the **Customer GET** endpoint (using Basic Auth or CUSTOMER_KEY) to retrieve your account object, which contains a `pubkeys` map of device IDs to DEVICE_PUBLIC_KEYs
2. Use a DEVICE_PUBLIC_KEY as the `access_token` on any device endpoint to read or write that device's configuration and data

## Endpoint Overview

| Category | Description |
|----------|-------------|
| **Device GET / POST** | Read and write device configuration via the device shadow |
| **RPC** | Send commands to a device: reset volume, toggle cleaning mode, read live sensor values, and more |
| **Data queries** | Query historical servings, alerts, and telemetry for a port, device, site, or all devices |
| **Customer GET / POST** | Retrieve your device list and update account-level settings |

## Device Shadow

Device configuration is managed through a shadow — a JSON document stored in the cloud that is synchronized to the device. Writing to the shadow's `desired` state queues the change; if the device is offline, the update is applied automatically when it reconnects.

Configuration keys are split into two groups:

- **`config`** — read/write settings (beverage info, alert thresholds, business hours, WiFi, etc.)
- **`config_readonly`** — read-only status and counters reported by the device (firmware version, keg levels, pulse counts, pressure, temperature, etc.)

## Real-time Notifications

Use the WebSocket endpoint to receive live push notifications instead of polling:

```
wss://mdash.net/api/v2/m/device/notify?access_token=<DEVICE_PUBLIC_KEY>
```

See the **Notifications** page for message format details and a Node.js example.

## Trying the API

Each endpoint page includes a built-in request builder. Enter a valid `access_token` and click **Send API Request** to call the live API directly from your browser. Sample code in multiple languages can be generated from the same page.
