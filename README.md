# Sample App
Sample app for Identity Vault 5 testing.

## Issue - Unhandled error on getValue
On an Android device
`npm install`
`npm run build`
`ionic cap run android` (on a real android device)

In the app
- Enter some data for session data and press `set session data`
- Press `lock vault`
- Press `restore session data` 
- Passcode will be requested 
- The call to `getValue` will fail with `{"code":0,"message":"Unhandled Error: "}`
