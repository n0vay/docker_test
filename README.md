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

### How to interact with a Docker Image?
We specify ports inside the dockerfile which are then mapped to external ports while creating a container. The expose command is required if we are using the Docker Desktop Application.

###How to build the image from dockerfile? -t tag is for name.
```bash
docker build -t myapp .
```
Add tag to name by:
```bash
docker build -t myapp:v1 .
```
Where my app is the name of the image and . is the relative path to docker file

For all the info we don't want to go into the image there is a .dockerignore file present in the same directory as dockerfile which works similarly to .gitignore.

Layer Caching:
Images are immutable and cannot be changed after creation.
Docker uses resources each time to download and create images, these layers are cached inside Docker for performance.
Write docker files in such a way that the common components of the builds such as dependencies are reused from the cache.
eg copying packagelock.json first and doing npm install first before copying all the files would help docker perform better as up to this point there are no changes from the cached image.

Volumes:
If we change the code in our application and stop-start the container, it will not change in the container as the image is read-only.
Volumes is a feature of docker that helps us to specify folders in our working directory that becomes available to the containers because of frequent changes to code while development.
Volumes map the folder on the local machine to the folders inside the container so the local changes are reflected inside the container.
Important info: The actual image does not change, when sharing the image we have to create a new one. Its just for testing and development.

How to setup  volumes:

Add nodemon to our image globally by adding the command in the docker file
```bash
dRUN npm install -g nodemon
```
Nodemon watches the file for change and restarts server when it detects a change, add the following script in the docker file: nodemon app.js.
But we should probably add it to scripts in packagelock.json and run the script from the dockerfile.
In the packagelock.json add the following script, -L is for windows:
```bash
"dev": "nodemon -L app.js"
```
and replace the CMD in docker file by to run the dev script:
```bash
CMD ["npm", "run", "dev"]
```
now nodemon will restart the server when it detects a change in the files.

Build the image by using, just used nodemon for tag:
```bash
docker build -t myapp:nodemon
```
Run the image using the following command:
```bash
docker run --name myapp_nodemon -p 4000:4000 -rm -v C:\Users\Shantanu\node_projects\docker_test:/app -v /app/node_modules myapp:nodemon
```
where my app_nodemon is name of conatiner
-p 4000:4000 is port mapping
-rm is tage to delete container after being stopped
myapp:nodemon is the name and tag of the image
-v "absolute path of project":"path inside the container" maps the local files to the container files
-v /app/node_modules creates a separate volume for the dependencies which are not mapped from local to the container because we do not want to mirror those changes. 
It maps the changes from the container to a folder managed by docker. As the second volume has a more specific path it does not interfere with the first volume.

Docker Compose:
Docker compose contains configuration for the project like port mappings, volumes, names etc.
Create a file named docker-compose.yaml in the root directory of the project.

### Running Container from Applcation
Start it from the images tab, and specify the name and the port number on our system which should be mapped to the port on the container.

### Running Containers from CLI
See images in the system by using docker images
```bash
docker images
```
Run image by using docker run command
```bash
docker run --name myapp_container -p 4000:4000 -d myapp_image
```
where 
--name specifies name of container
-p local port: container port defines the port links
-d means detached mode and the current terminal will not be used as container cli. The current terminal is detached.

list current running containers by using 
```bash
docker ps
```
list all containers by using 
```bash
docker ps -a
```
stop by
```bash
docker stop myapp_container
```
remove the image by:
```bash
docker images rm myapp
```
what if the container is using that image?
force the removal by adding -f tag
```bash
docker images rm myapp -f
```
or remove the container by using:
```bash
docker container rm myapp
```
for all the containers using the image and then remove the image.
To start a container, it will start in detached mode by default:
```bash
docker start myapp_container
```
To remove all the docker images, containers and volumes use:
```bash
docker system prune -a
```






