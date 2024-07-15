FROM node:17-alpine 
# import base image with OS 

RUN npm install -g nodemon
# this command isused to create volumes to reflect real time changes in the image, used for testing and development purpose.

WORKDIR /app
# define workdir inside the image, this is the image where all the files are coppied. 

COPY package.json .
# copy package lock from local files to install dependencies, in the next step.
# it will help with caching dependicies when we rebuild image, this process is called layer caching. 

RUN npm install
# run a command to install dependencies.

COPY . .
# copy the local files to the image
# first . is the source path which is current folder and the second is the target folder which is defined by WORKDIR in the above code.
# a .dockerignore is defined in the folder to remove dependency folders, here its node modules

EXPOSE 4000
# required for docker desktop port mapping

CMD ["npm", "run", "dev"]
# command for starting server after setup is complete