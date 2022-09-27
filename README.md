# Docker setup for Vite + React
Since Vite is the new kid around the block, I faced multiple issues when setting up with Docker. I will leave my setup configs in this file for future reference and for others if anyone comes looking for it.

## Vite setup
Create a project with vite and select React as framework. Here I will use ```npm``` to create a React project.
```shell
npm create vite@latest
```
After the initialization is complete you will find a vite config file in the project directory. The initial config file will look something like this.
```javascript
// ./vite.config.ts

import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()]
})
```

## Docker config
We need to now create a docker file within the project directory. This file will contain very simple instructions.
```python
# ./Dockerfile

FROM node:18.0.0    # you can pick any node version you want

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .  # we will copy all files from current directory to the app directory

CMD ["npm", "run", "dev"] # start the vite server
```

Now we need to build the docker image.
```shell
docker build -t username/docker-vite-react:1.0
```
Lets go through this command. We are creating an image with name 'docker-vite-react' and giving it a version number 1.0. ```-t``` is used to give the image file a tag. We use username in the tag so that we can store it in dockerhub.

### Run a Container
After the image is generated we need to run the image by creating a container.
```shell
docker run -p 3000:5173 'image_id'
```
Even though we have successfully ran a container, if we open our browser and try to access ```localhost:3000```, we will be faced with an error "This site can not be reached!" This is because even though docker is aware of the port of Vite server, the server is not aware of anyone who is trying to access the server. So, we need to update the Vite config file.

## Update Vite config
```javascript
// ./vite.config.ts

import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    host: true,
    port: 5173
  }
})
```

Vite needs to expose its host so that other servers can connect to it. By adding ```host``` & ```port``` property to config file, we can expose the server port. Now if we build the image again and run it, everything will work!
