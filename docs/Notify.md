# Notifications

When the state of a device changes, it is possible to obtain receive real-time notifications as an alternative to polling a Device GET enpoint. To do this, a WebSocket connection is used to send notifications. 

# Endpoint Details
wss://mdash.net/api/v2/m/device/notify?access_token=<DEVICE_PUBLIC_KEY>

# Message Details
All notifications messages are sent as JSON objects with the following keys:
- name: Notification name, e.g. "online", "offline", "updated", "rpc.in.GetInfo", "rpc.out.ping"
- id: The device ID of a device that generated the event
- data: Notification-specific data (optional) 
  - See the /api/v2/m/device GET endpoint for device object definitions