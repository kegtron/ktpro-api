# Introduction

Kegtron Pro uses a REST API to interact with keg monitor hardware ("devices")

#### Interacting with devices is a two-step process:

1. Call the **Customer** GET endpoint to obtain list of devices and their respective keys
2. Read or write from individual devices or ports using other GET/POST endpoints

#### Device Notifications
- Use WebSocket notifications to push live device updates
- See the Notifications tab to the left

#### Endpoint details can be found by clicking the tab to the left
- You can excercise API endpoints directly from your browser. Click on the desired endpoint, enter a valide access_token (e.g. device or customer key) and click "Send API Request"
- Generate sample code from the endpoint details page 