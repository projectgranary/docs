---
description: >-
  This section describes steps required to set up your environment to create a
  project with Firebase Cloud Functions and deploy them.  Cloud Functions is a
  service by Firebase that allows us to trigger
---

# Setting up Firebase Cloud Functions' Project

## Dependencies

1. [snowplow-nodejs-tracker](https://github.com/snowplow/snowplow-nodejs-tracker)
2. firebase-functions

Install the dependencies using npm from function's directory. Assuming npm is installed, call: 

```text
npm install snowplow-tracker firebase-functions
```

## Project Setup

To initialize Firebase SDK for the functions, [Firebase Command Line Interface \(CLI\) tools](https://www.npmjs.com/package/firebase-tools) are needed. To install it, make sure you have installed [Node.js](http://nodejs.org/) and [npm](https://npmjs.org/) and have also signed up for a Firebase account. 

Assuming npm is installed, get Firebase CLI by running following command:

```text
npm install -g firebase-tools
```

Once the CLI is installed, you can start using `firebase` command globally.

To start using Firebase CLI, first log into Firebase and authenticate the tools.  

```text
firebase login
```

At last, run init from project directory and follow the instructions to complete the setup.

```text
firebase init functions
```

Please note that during the project creation process, there will be a prompt to choose whether to enable ESLint to catch probable bugs and/or enforce style, or not. If so, ESLint should be first installed globally.

```text
npm install -g eslint
```

After enabling ESLint, potential problems in your code will appear as errors. Should there be a need to disable ESLint again, edit firebase.json in your project and remove the predeploy script that runs the lint command.

```text
"predeploy": [
  "npm --prefix functions run lint"
],
```

