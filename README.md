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
