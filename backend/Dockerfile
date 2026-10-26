# Backend Dockerfile - builds an image that runs the Express app

# Start from an official image that already has Node.js installed.
# "alpine" is a very small Linux, so the image downloads and starts faster.
FROM node:24-alpine

# Create a folder called /app inside the image and work from there.
# Every command below runs inside this folder.
WORKDIR /app

# Copy package.json and package-lock.json into /app.
# These list the libraries the app needs (express, cors, randomcolor).
# They are copied on their own first so Docker can cache the next step:
# if only index.js changes, npm install doesn't need to run again.
COPY package*.json ./

# Download and install the libraries listed in package.json.
RUN npm install

# Copy the app code into /app.
COPY index.js .

# Document that the app listens on port 5000 inside the container.
# (This is a note for people and tools; it doesn't open the port by itself.)
EXPOSE 5000

# The command that runs when a container starts: start the Express app.
CMD ["node", "index.js"]
