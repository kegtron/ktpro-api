# Introduction

Kegtron Pro uses a REST API to interact with keg monitor hardware ("devices")

#### Interacting with devices is a two-step process:

1. Call the **Customer** GET endpoint to obtain list of devices and their respective keys
2. Read or write from individual devices or ports using other GET/POST endpoints

#### Device Notifications
- Enable WebSockets to get live push notifications of device updates
- See the Example tab to the left

#### Endpoint details can be found by clicking the tab to the left
- Be sure to check out the "Try It" tab to excercise the API from your browser and/or generate sample code 