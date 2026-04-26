# WebSocket Notifications

Instead of polling the Device GET endpoint, you can open a WebSocket connection to receive real-time push notifications whenever a device's state changes — for example, when a serving is recorded, keg levels update, or the device goes offline.

## Endpoint

```
wss://mdash.net/api/v2/m/device/notify?access_token=<DEVICE_PUBLIC_KEY>
```

One connection covers one device. To monitor multiple devices, open one connection per DEVICE_PUBLIC_KEY.

## Message Format

All messages are JSON objects with the following fields:

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Notification type (see table below) |
| `id` | string | Device ID that generated the event |
| `data` | object | Event-specific payload (may be absent) |

## Notification Types

| `name` | When fired | `data` content |
|--------|-----------|----------------|
| `online` | Device connects to the cloud | none |
| `offline` | Device disconnects from the cloud | none |
| `updated` | Device shadow state changes | Updated shadow keys (see below) |
| `rpc.in.<method>` | An RPC call is dispatched to the device | RPC request arguments |
| `rpc.out.<method>` | The device responds to an RPC call | RPC response value |

### The `updated` notification

This is the most useful notification for live monitoring. It fires whenever the device reports new state — including after every serving. The `data` field contains only the keys that changed, in the same shape as the Device GET response.

Example: after a pour on port 0, you would receive something like:

```json
{
  "name": "updated",
  "id": "device2",
  "data": {
    "shadow": {
      "state": {
        "reported": {
          "config_readonly": {
            "port0": {
              "lastServing": 473,
              "volDisp": 7880,
              "pulseCnt": 3723
            },
            "temp": 4.1,
            "humidity": 67.2
          }
        }
      }
    }
  }
}
```

See the **Device GET** endpoint for full field definitions.

## Connection Behavior

- The server does not send a heartbeat — connections are kept alive by the mDash platform
- If the connection drops, reconnect with an exponential back-off and resume normally; no messages are queued while disconnected so poll Device GET once on reconnect to resync state

## Examples

### Node.js

```js
const WebSocket = require('ws');  // npm install ws

const pubkey = '<DEVICE_PUBLIC_KEY>';
const url = `wss://mdash.net/api/v2/m/device/notify?access_token=${pubkey}`;
const ws = new WebSocket(url, { origin: url });

ws.on('open', () => console.log('Connected'));

ws.on('message', (data) => {
  const msg = JSON.parse(data);
  console.log(`[${msg.name}] device: ${msg.id}`);

  if (msg.name === 'updated') {
    const ports = msg.data?.shadow?.state?.reported?.config_readonly;
    if (ports?.port0) {
      console.log('Port 0 last serving:', ports.port0.lastServing, 'mL');
    }
  }
});

ws.on('close', () => console.log('Disconnected — reconnecting...'));
ws.on('error', (err) => console.error('WebSocket error:', err.message));
```

### Browser (JavaScript)

```js
const pubkey = '<DEVICE_PUBLIC_KEY>';
const url = `wss://mdash.net/api/v2/m/device/notify?access_token=${pubkey}`;
const ws = new WebSocket(url);

ws.addEventListener('message', (event) => {
  const msg = JSON.parse(event.data);

  if (msg.name === 'updated') {
    const reported = msg.data?.shadow?.state?.reported;
    // Update your UI with the latest reported state
  }

  if (msg.name === 'offline') {
    console.warn(`Device ${msg.id} went offline`);
  }
});
```
