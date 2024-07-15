Docker Image: 
•	It is a container blueprint containing a runtime environment (like node), application code, dependencies, configurations (env variables), and commands.
•	It is read-only and needs to be a new one after any change.

Container:
•	A process that runs our application as outlined in Image.
•	They are isolated from other processes inside our computer.

We can share an image of an application that can be used to create similar applications on different computers.

Images are made up of several layers:
•	Parent Image: Includes the OS and sometimes the runtime environment.
•	Source Code
•	Dependencies

Where to get Images? Several images are listed at hub.docker.com and can be pulled using the following command in the terminal, it also has tags for different distributions: 
docker pull node
After pulling an image, we can run it in the container section of docker and interact with it using the command line interface.
