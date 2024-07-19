Here's the formatted explanation for GitHub:

## Docker Image

- **Definition**: A Docker Image is a container blueprint that includes a runtime environment (such as Node.js), application code, dependencies, configurations (environment variables), and commands.
- **Properties**: It is read-only and requires rebuilding after any changes.

## Container

- **Definition**: A Container is a process that runs the application as defined in the Docker Image.
- **Isolation**: Containers are isolated from other processes on the host computer.

## Sharing Docker Images

Docker Images can be shared to create similar applications on different computers.

### Image Composition

Docker Images consist of several layers:
- **Parent Image**: Includes the operating system and sometimes the runtime environment.
- **Source Code**
- **Dependencies**

## Where to Get Docker Images

Several Docker Images are available at [hub.docker.com](https://hub.docker.com) and can be pulled using the terminal command:
```bash
docker pull node
```
After pulling an image, it can be run in the Docker container section and interacted with via the command line interface.

Docker desktop application can be used for these functionalities  on the computer.

## Docker File

A file named Dockerfile is created inside the repo we are containerizing. 
The file contains instructions for Docker to build and run applications.
More information is provided in the comments of the Dockerfile.

### When to use RUN and CMD commands inside Dockerfile?
- **Run is used for commands which are used during the building phase of the application for eg RUN npm install
- **CMD is used for commands which are to be run when we have an instance inside the container and are written as CMD ["node", "app.js"]

Here is the formatted code for your GitHub README:

## How to Interact with a Docker Image?
We specify ports inside the Dockerfile which are then mapped to external ports while creating a container. The `EXPOSE` command is required if we are using the Docker Desktop Application.

## How to Build the Image from Dockerfile?
The `-t` tag is for the name.

Build the image:
```bash
docker build -t myapp .
```

Add a tag to the name:
```bash
docker build -t myapp:v1 .
```
Where `myapp` is the name of the image and `.` is the relative path to the Dockerfile.

## .dockerignore
For all the info we don't want to go into the image, there is a `.dockerignore` file present in the same directory as the Dockerfile, which works similarly to `.gitignore`.

## Running Container from Application
Start it from the images tab, and specify the name and the port number on our system which should be mapped to the port on the container.

## Running Containers from CLI
### See Images in the System
```bash
docker images
```

### Run Image
```bash
docker run --name myapp_container -p 4000:4000 -d myapp_image
```
- `--name`: Specifies name of the container
- `-p local port:container port`: Defines the port links
- `-d`: Detached mode, the current terminal will not be used as container CLI

### List Current Running Containers
```bash
docker ps
```

### List All Containers
```bash
docker ps -a
```

### Stop Container
```bash
docker stop myapp_container
```

### Remove the Image
```bash
docker rmi myapp
```

### Force Removal if Container is Using the Image
```bash
docker rmi myapp -f
```
Or remove the container first:
```bash
docker container rm myapp_container
```

### Start a Container
```bash
docker start myapp_container
```

### Remove All Docker Images, Containers, and Volumes
```bash
docker system prune -a
```
## Layer Caching
Images are immutable and cannot be changed after creation. Docker uses resources each time to download and create images, and these layers are cached inside Docker for performance.

Write Dockerfiles in such a way that the common components of the builds, such as dependencies, are reused from the cache. For example, copying `package-lock.json` first and doing `npm install` before copying all the files would help Docker perform better as up to this point there are no changes from the cached image.

## Volumes
If we change the code in our application and stop-start the container, it will not change in the container as the image is read-only. Volumes is a feature of Docker that helps us specify folders in our working directory that become available to the containers because of frequent changes to code during development.

Volumes map the folder on the local machine to the folders inside the container, so the local changes are reflected inside the container.

**Important Info:** The actual image does not change. When sharing the image, we have to create a new one. This is just for testing and development.

### How to Set Up Volumes
1. Add nodemon to our image globally by adding the command in the Dockerfile:
    ```bash
    RUN npm install -g nodemon
    ```
2. Nodemon watches the file for changes and restarts the server when it detects a change. Add the following script in the Dockerfile: `nodemon app.js`. But we should probably add it to scripts in `package.json` and run the script from the Dockerfile.
    In `package.json`, add the following script:
    ```bash
    "dev": "nodemon -L app.js"
    ```
3. Replace the CMD in Dockerfile to run the dev script:
    ```bash
    CMD ["npm", "run", "dev"]
    ```
    Now nodemon will restart the server when it detects a change in the files.

### Build the Image
```bash
docker build -t myapp:nodemon .
```

### Run the Image
```bash
docker run --name myapp_nodemon -p 4000:4000 --rm -v C:\Users\Shantanu\node_projects\docker_test:/app -v /app/node_modules myapp:nodemon
```
- `myapp_nodemon`: Name of the container
- `-p 4000:4000`: Port mapping
- `--rm`: Tag to delete container after being stopped
- `myapp:nodemon`: Name and tag of the image
- `-v "absolute path of project":"path inside the container"`: Maps the local files to the container files
- `-v /app/node_modules`: Creates a separate volume for the dependencies which are not mapped from local to the container because we do not want to mirror those changes.

## Docker Compose
Docker compose contains configuration for the project like port mappings, volumes, names, etc. Create a file named `docker-compose.yaml` in the root directory of the project.

### Run the Docker Compose File
```bash
docker compose up
```

### Stop and Delete the Container
```bash
docker compose down
```

### Remove All Images, Containers, and Volumes
```bash
docker compose down --rmi all -v
```




