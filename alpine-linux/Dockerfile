FROM node:20-alpine

USER node

RUN mkdir /home/node/code

WORKDIR /home/node/code

COPY --chown=node:node package-lock.json package.json ./

RUN npm ci

COPY --chown=node:node . .

CMD ["node", "index.js"]

# FROM node:20-alpine
# This line sets the base image for the build.

# node:20-alpine means it starts with the official Node.js image, specifically version 20, built on the Alpine Linux distribution. Alpine is a very small, security-focused Linux distribution, which results in a much smaller final image size compared to other variants like ubuntu or debian.

# USER node
# This is a crucial security best practice. The node base image comes with a pre-configured, non-root user called node. This instruction switches the context so that all subsequent commands run as this node user instead of root. This limits the container's permissions, reducing potential damage if your application is ever compromised.

# RUN mkdir /home/node/code
# This command runs inside the container during the image build. It creates a directory named code inside the node user's home directory (/home/node/). This directory will hold the application's source code.

# WORKDIR /home/node/code
# This sets the working directory for all subsequent instructions (COPY, RUN, CMD). From this point on, any relative paths will be based inside /home/node/code, so you don't have to type the full path repeatedly.

# COPY --chown=node:node package-lock.json package.json ./
# This is a key step for build optimization.

# It copies only package.json and package-lock.json into the working directory.

# --chown=node:node sets the owner of these files to the node user and group, preventing permission issues.

# By copying these files separately from the rest of the code, you leverage Docker's layer caching. If you only change your application code but not the dependencies, Docker will reuse the already-built layer from the next step, making subsequent builds much faster.

# RUN npm ci
# This command installs the project dependencies.

# npm ci ("clean install") is used instead of npm install. It installs dependencies directly from the package-lock.json file, ensuring a clean, reproducible, and often faster installation, which is ideal for automated environments like Docker builds.

# COPY --chown=node:node . .
# This line copies the rest of your application's source code (e.g., index.js, and other files) from your local directory (.) into the container's working directory (.).

# The --chown flag is used again to ensure the node user owns all the application files.

# CMD ["node", "index.js"]
# This specifies the default command to run when a container is started from this image. It will execute node index.js, starting your Node.js application. This is the final step that brings your application to life inside the container.


# command to build image:-
 #  sudo docker build -t complicated-app:apline . 

# command to run container:-
# sudo docker run -it -p 8080:8080 --name my_app_instance_1 --rm --init  more-complicated-app:latest

# Explanation of command
# sudo
# Executes the command with superuser (root) privileges. This is often required for the Docker command to communicate with the Docker daemon, which runs as root.

# docker run
# The fundamental command to create and start a new container from a specified image.

# -it
# This is a combination of two separate flags:

# -i or --interactive: Keeps the standard input (STDIN) open, allowing you to type commands into the container.

# -t or --tty: Allocates a pseudo-terminal. This gives you a proper command prompt and makes the interactive session behave like a normal terminal.
# You almost always use these two flags together to get an interactive shell inside a container.

# -p 8080:8080
# This is the --publish flag. It maps a port from your host machine to a port inside the container.

# The format is [HOST_PORT]:[CONTAINER_PORT].

# In this case, it forwards all traffic from port 8080 on your host machine (e.g., localhost:8080) to port 8080 inside the container. This is how you access a web server or application running within the container from your own machine.

# --name my_app_instance_1
# Assigns a custom, human-readable name to the container. Without this flag, Docker would assign a random name (like pensive_mcnulty). Naming containers makes them easier to manage (e.g., docker stop my_app_instance_1).

# --rm
# This is a cleanup flag. It automatically removes the container (including its file system) when the container exits or is stopped. This is extremely useful for temporary tasks or development, as it prevents your system from getting cluttered with stopped containers.

# --init
# This flag ensures a tiny init process is used as the main process inside the container. This init process correctly handles signals and reaps any "zombie" processes that your application might create. It's a good practice that makes your container behave more predictably, like a lightweight virtual machine.

# more-complicated-app:latest
# This is the last part of the command and specifies the image to use for creating the container.

# more-complicated-app is the repository name.

# latest is the tag, which typically points to the most recent version of the image.

# ## In Summary
# When you execute this entire command, you are telling Docker to:

# Start a new container from the more-complicated-app:latest image.

# Connect your terminal directly to it for an interactive session (-it).

# Name it my_app_instance_1 for easy reference.

# Forward port 8080 from your computer to the container's port 8080.

# Ensure proper process management inside the container (--init).

# Completely delete the container automatically as soon as you exit the interactive session (--rm).