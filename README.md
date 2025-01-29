# Setting Up a Vite React App with Docker

## Step 1: Create a Vite App

We use Vite to create a React project with the following command:

```sh
npm create vite@latest
```

Follow the prompts to set up your project. Choose React and TypeScript if needed.

## Step 2: Install Dependencies

Navigate into the project folder and install the necessary dependencies:

```sh
cd frontend
npm install
```

## Step 3: Configure Vite Server

Modify the `vite.config.ts` file to set up the development server:

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: {
    host: "0.0.0.0", // Allows access from any IP
    port: 5173, // Runs the development server on port 5173
  },
});
```

## Step 4: Create a Dockerfile

Create a `Dockerfile` in the root directory to containerize the app:

```dockerfile
# Use a lightweight Node.js image
FROM node:alpine

# Set working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies first (for caching efficiency)
COPY package*.json ./
RUN npm install

# Copy the rest of the project files into the container
COPY . .

# Expose the development server port
EXPOSE 5173

# Start the development server
CMD ["npm", "run", "dev"]
```

## Step 5: Build the Docker Image

Run the following command in the terminal to build the Docker image:

```sh
docker build -t blog-frontend .
```

This command:

- `docker build` creates the image.
- `-t blog-frontend` tags the image with the name `blog-frontend`.
- `.` specifies the current directory as the build context.

## Step 6: List Docker Images

After building, verify the image is created successfully:

```sh
docker images
```

This command lists all available Docker images.

## Step 7: Run the Docker Container

To run the container in detached mode and map port 8000 on the host to 5173 in the container:

```sh
docker run -d -p 8000:5173 blog-frontend
```

Explanation:

- `-d` runs the container in detached mode (in the background).
- `-p 8000:5173` maps port 8000 of the host to port 5173 inside the container.
- `blog-frontend` is the name of the image being run.

## Step 8: Access the Application

Once the container is running, open `http://localhost:8000` in your browser to access the Vite app.
