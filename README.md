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
