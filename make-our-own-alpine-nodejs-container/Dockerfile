# Use the official Alpine Linux version 3.19 as the base image.
# Alpine is chosen for its small size, which results in a smaller final image.
FROM alpine:3.19

# Update the package index and install Node.js and npm using Alpine's package manager (apk).
# --update: Refreshes the local package index before adding new packages.
# nodejs npm: The packages to be installed.
RUN apk add --update nodejs npm

# Create a dedicated user and group for the application.
# This is a critical security best practice to avoid running the application as the root user.
# -S: Creates a system user/group (no password, no home directory created by default).
# -G node: Adds the 'node' user to the 'node' group.
RUN addgroup -S node && adduser -S node -G node

# Switch the context to the newly created 'node' user.
# All subsequent commands in the Dockerfile will be executed by this user.
USER node

# Create a directory inside the 'node' user's home directory to store the application code.
# This keeps the project files organized and separate from the system files.
RUN mkdir /home/node/code

# Set the working directory for subsequent commands.
# This means commands like COPY and RUN will execute relative to this path.
WORKDIR /home/node/code

# Copy the package manifest and lock file into the working directory.
# --chown=node:node: Sets the ownership of these files to the 'node' user and group.
# This is done in a separate step to leverage Docker's layer caching. If these files don't change,
# Docker won't need to re-run the 'npm ci' step on subsequent builds, making them much faster.
COPY --chown=node:node package-lock.json package.json ./

# Install project dependencies using 'npm ci'.
# 'npm ci' (clean install) is preferred for CI/CD environments because it installs dependencies
# exactly as specified in the package-lock.json, ensuring a reproducible build.
RUN npm ci

# Copy the rest of the application's source code into the working directory.
# The first '.' refers to the source directory on the host machine (where the Dockerfile is).
# The second '.' refers to the current working directory inside the container (/home/node/code).
# --chown=node:node: Ensures the application files are owned by the 'node' user.
COPY --chown=node:node . .

# Specify the default command to run when a container is started from this image.
# This will execute 'node index.js', starting the Node.js application.
# The "exec form" (using a JSON array) is the preferred syntax for CMD.
CMD ["node", "index.js"]

# To build it :-  sudo docker build -t complicated-app:apline2 .
# TO run it:- sudo docker run -it -p 8080:8080 --name my-app  --rm --init complicated-app:apline2
