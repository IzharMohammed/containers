# =========================================================================
# Stage 1: The Build Stage
# This stage uses a full Node.js image to install dependencies.
# It's a temporary environment that will be discarded later.
# =========================================================================

# Start from the official Node.js 20 image. This image contains the full
# Node.js SDK, npm, and all necessary build tools.
# 'AS node-builder' gives this stage a name we can reference later.
FROM node:20 AS node-builder

# Set the working directory inside the build stage. All subsequent
# commands will run from within the /build directory.
WORKDIR /build

# Copy the package manager files. This is done first to leverage Docker's
# layer caching. If these files don't change, 'npm ci' won't rerun.
COPY package-lock.json package.json ./

# Install all dependencies, including development dependencies, using 'npm ci'.
# 'ci' provides faster, more reliable, and reproducible builds.
RUN npm ci

# Copy the rest of the application's source code into the build stage.
COPY . .


# =========================================================================
# Stage 2: The Runtime Stage
# This is the final stage that creates the lean, production-ready image.
# It only includes the bare essentials needed to run the application.
# =========================================================================

# Start fresh from a minimal Alpine Linux image. This makes the final
# image significantly smaller and more secure.
FROM alpine:3.19

# Install only the Node.js runtime, not the full SDK or npm.
# This is all that's needed to execute the pre-built application.
RUN apk add --update nodejs

# Create a dedicated, non-root user and group for security.
# Running as a non-root user is a critical security best practice.
RUN addgroup -S node && adduser -S node -G node

# Switch to the non-root user for all subsequent commands.
USER node

# Create and set the application's home directory.
RUN mkdir /home/node/code
WORKDIR /home/node/code

# This is the key step: copy files FROM the 'node-builder' stage.
# It copies the application source code and the pre-installed 'node_modules'
# folder from '/build' in the first stage to the current directory.
# --chown ensures the 'node' user owns these files.
COPY --from=node-builder --chown=node:node /build .

# Set the default command to run when the container starts.
# This executes the application using Node.js.
CMD ["node", "index.js"]

# To build it :- docker build -t my-multi .
# TO run it:- docker run -it -p 8080:8080 --name my-app --rm --init my-multi

# sudo docker run: The command to create and start a container.

# -d: Runs the container in detached mode (in the background).

# -p 8080:8080: Maps port 8080 on your host machine to port 8080 inside the container 

# --name my-app: Assigns a readable name to your running container.

# --rm: Automatically removes the container when it stops, keeping your system clean.

# my-multi: The image to run.