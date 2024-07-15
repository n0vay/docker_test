FROM node:16-alpine
# import base image with OS

WORKDIR /app
# define workdir folder inside the image

COPY package.json .
# copy package.json file over for layer caching
# add .dockerignore add the dependency folder to that so it does not get copied

RUN npm install
# run the command to install dependencies in the image

COPY . .
# copy all the local files to the image

EXPOSE 3000
# required for docker desktop port mapping

CMD ["npm", "start"]
# command to start script for react 