# Nodejs

## Create the Node.js app

First, create a new directory where all the files would live. In this directory
create a `package.json` file that describes your app and its dependencies:

```json
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}

```

run `npm install`. If you are using `npm`

Then, create a `server.js` file that defines a web app using the
[Express.js](https://expressjs.com/) framework:

```jsx
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST, () => {
  console.log(`Running on http://${HOST}:${PORT}`);
});

```

## Creating a Dockerfile

Create an empty file called `Dockerfile`:

```markup
touch Dockerfile

```

Your `Dockerfile` should now look like this:

```docker
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --omit=dev

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]

```

## .dockerignore file

Create a `.dockerignore` file in the same directory as your `Dockerfile`
with following content:

```
node_modules
npm-debug.log

```

# **Building your image**

```bash
docker build . -t alisystem/node-web-app
```

# **Run the image**

```bash
docker run -p 49160:8080 -d alisystem/node-web-app
```
![Screenshot from 2023-08-23 08-00-56](https://github.com/alinedam/Sitech-Internship/assets/108859223/1e5d7f36-5799-44f2-aa01-a2e9898be492)

## Test

To test your app, get the port of your app that Docker mapped:

```bash
$ docker ps

# Example
ID            IMAGE                                COMMAND    ...   PORTS
ecce33b30ebf  <your username>/node-web-app:latest  npm start  ...   49160->8080
```
![Screenshot from 2023-08-23 08-06-55](https://github.com/alinedam/Sitech-Internship/assets/108859223/e176ca0f-4d6a-4caa-abf6-c7825590a00c)

Now you can call your app using `curl` (install if needed via: `sudo apt-get install curl`):
![Screenshot from 2023-08-23 08-08-14](https://github.com/alinedam/Sitech-Internship/assets/108859223/2a9f8167-f5c3-430b-93cb-d448247a24a1)
