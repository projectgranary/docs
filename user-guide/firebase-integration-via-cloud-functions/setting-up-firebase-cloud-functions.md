# Setting up Firebase Cloud Functions

**About**

This project contains JavaScript functions for for Firebase Cloud Functions to pass events recorded by Firebase Analytics to Snowplow collector\(s\).

Dependencies

snowplow-nodejs-tracker

Run npm from your functions directory

```text
npm install snowplow-tracker
```

firebase-functions

```text
npm install firebase-functions
```

Setup

Initialize Firebase SDK for the functions, by first installing Node.js, npm and also firebase-tools

```text
npm install -g firebase-tools
```

also you might need to install eslint globally.

Login to Firebase and authenticate the tool

```text
firebase login
```

Run init from project directory and follow the instructions

```text
firebase init functions
```



