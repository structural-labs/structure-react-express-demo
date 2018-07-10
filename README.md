# Structure React Express Demo

This is a short demo to walk you through the deployment of a React and Express application on Structure

## Environment Setup

Let's start out by making sure we have all of the requirements for this demo. All we will need is [Structure](https://structure.sh) and [create-react-app](https://github.com/facebook/create-react-app). Go ahead and run

```
pip install structure
npm install -g create-react-app
```

## Express

Great! Now let's setup our express application.

```
mkdir server
cd server
touch server.js
```

Now copy and paste this code into server.js. All this does is return the static files from your built react app. We'll create those next!

_Note:_ Structure requires your port to be **80**.

```
// server.js
const express = require("express");
const path = require("path");
const app = express();
const port = 80;

// Serve any static files
app.use(express.static(path.join(__dirname, "client/build")));

app.get("/", function(req, res) {
  res.sendFile(path.join(__dirname, "client/build", "index.html"));
});

app.listen(port, () => console.log(`Listening on port ${port}`));
```

## React

Now that we have an express server ready to go, let's build a react app. To do this, simply run

```
create-react-app client
```

## Build Script

Create a `package.json` using the command `touch package.json` inside your /server folder.

Paste this into package.json. Let's look at this a little more closely. When we upload this application to [Structure](https://structure.sh), Structure will recursively install all package.json dependencies throughout our entire project. It will then run `npm start`. If we look at our package.json here, we can see that the start script will run a client and server command. The `client` command moves into our /client folder, builds the React application, and then moves back to the /server folder. The `server` command then starts a node server.

```
{
  "name": "server",
  "version": "1.0.0",
  "scripts": {
    "start": "npm run client && npm run server",
    "client": "cd client && yarn install && npm run build && cd ..",
    "server": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.2"
  },
}
```

## Structure

This is the hard pard so pay close attention. While inside the /server folder, run the command

```
structure deploy my-awesome-app
```

**That's it!** Don't believe me? Check out `https://my-awesome-app-<your-structure-user-name>.structure.sh`

There it is. A fully functioning Express/React app deployed and live on [Structure](https://structure.sh).

https://medium.freecodecamp.org/how-to-make-create-react-app-work-with-a-node-backend-api-7c5c48acb1b0
