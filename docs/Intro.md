# Introduction

Kegtron Pro uses a REST API to interact with keg monitor hardware ("devices")

#### Interacting with devices is a two-step process:

1. Call the **Customer GET** endpoint to obtain list of devices and respective keys
2. Read from individual devices using the **Device GET** endpoint
3. Write to individual devices useing the **Device POST** or **RPC POST** endpoints



#### Endpoint details can be found by clicking the **Other** tab to the left
- Be sure to check out the "Try It" tab to excercise the API from your browser and/or generate sample code 