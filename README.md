# Structure React Express Demo

This is a short demo to walk you through the deployment of a React and Express application on Structure.

## Environment Setup

Let's start out by making sure we have all of the requirements for this demo. All we will need is [Structure](https://structure.sh) and [create-react-app](https://github.com/facebook/create-react-app). Go ahead and run:

```
sudo pip install structure
npm install -g create-react-app
```

## Express

Great! Now let's set up our Express application.

```
mkdir hello-react
cd hello-react
touch server.js
```

Now, copy and paste this code into server.js. All this does is return the static files from your built React app. We'll create those next!

_Note:_ Structure requires your port to be **80**.

```
const express = require("express");
const path = require("path");
const app = express();
const port = process.env.PORT || 80;

// Serve any static files built by React
app.use(express.static(path.join(__dirname, "client/build")));

app.get("/", function(req, res) {
  res.sendFile(path.join(__dirname, "client/build", "index.html"));
});

app.listen(port, () => console.log(`Listening on port ${port}`));
```

## React

Now that we have an Express server ready to go, let's build a React app. To do this, simply run:

```
create-react-app client
```

## Build Script

Create a `package.json` using the command `touch package.json` inside your /hello-react folder.

Paste the code below into package.json.

```
{
  "name": "hello-world",
  "version": "1.0.0",
  "scripts": {
    "start": "npm run build-client && node server",
    "start-local": "npm run build-client && PORT=8080 node server",
    "build-client": "cd client && npm run build && cd .."
  },
  "dependencies": {
    "express": "^4.16.2"
  }
}
```

Let's look at this a little more closely. When we upload this application to [Structure](https://structure.sh), Structure will recursively install all `package.json` dependencies throughout our entire project. It will then run `npm start`. If we look at our `package.json` here, we can see that the start script will run a `build-client` and `node server` command. The `build-client` command moves into our /client folder, builds the React application, and then moves back to the /hello-react folder. The `node server` command then starts the Express server.

## Structure

This is the hard part so pay close attention. While inside the /hello-react folder, run the command

```
structure deploy hello-react
```

**That's it!** Not hard at all, thanks to [Structure](https://structure.sh). Don't believe me? Check out `https://hello-react-<your-structure-user-name>.structure.sh`

There it is. A fully functioning Express/React app deployed and live on [Structure](https://structure.sh).

If you have any comments or suggestions for us, please reach out at [help@structure.sh](mailto:help@structure.sh)
