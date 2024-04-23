# Insta Clone a MERN application

This guide demonstrates how to containerize a MERN (MongoDB, Express.js, React, Node.js) application using Docker and Docker Compose.



## Frontend Dockerfile

The Frontend Dockerfile contains the following instructions:

- `FROM node:latest`
  - Specifies the base image for the frontend container as the latest version of the official Node.js Docker image.

- `WORKDIR /app`
  - Sets the working directory within the container to `/app`.

- `COPY . .`
  - Copies the entire contents of the current directory (which should contain the frontend code) into the `/app` directory inside the container.

- `RUN npm install`
  - Installs all the dependencies specified in the `package.json` file for the frontend application.

- `EXPOSE 5000`
  - Exposes port 5000 inside the container, allowing external access to the frontend application.

- `CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]`
 

To create the docker image, simply open up your terminal and change your present working directory to the root directory of your frontend. Make sure the docker file is in the same directory and run the command
    
            docker build -t image_name .


## Backend Dockerfile

The Backend Dockerfile contains the following instructions:

- `FROM node:latest`
  - Defines the most recent official Node.js Docker image as the base image for the backend container.

- `WORKDIR /app`
  - Sets the working directory within the container to `/app`.

- `COPY ../Server .`
  - Copies the contents of the `../Server` directory (which should contain the backend code) into the current working directory (`/app`) inside the container.

- `RUN npm install`
  - Installs all the dependencies specified in the `package.json` file for the backend application.

- `EXPOSE 5000`
  - Exposes port 3000 inside the container, allowing external access to the backend application.

- `CMD ["npm", "run", "devStart"]`
  - Specifies the command to be executed when the container starts. In this case, it runs the `devStart` script defined in the `package.json` file, which likely starts the backend application in development mode.


To build the Docker image, navigate to the root directory of your backend code and run:
    
            docker build -t image_name .

## .dockerignore

The .dockerignore file specifies files and directories that should be ignored by Docker when building the image:

- `README.md`: The project's README file.
- `build` and `dist`: Directories that may contain compiled or built artifacts.
- `node_modules`: The directory containing installed Node.js dependencies.
- `LICENSE`: The project's license file.
- `package-lock.json`: The file used by npm to lock down dependency versions.
- `.git`: The Git repository directory.
- `.DS_Store`: A file used by macOS to store folder metadata.
- `.env`: A file that may contain environment variables.

## docker-compose.yml

The `docker-compose.yml` file is used to define and configure multi-container Docker applications:

- `frontend`
  - `image: tanviagrawal1704/21bcp218-front`
    - Specifies the Docker image to be used for the frontend container. Change it to whatever name you have given
  - `ports`
    - Maps the host port (`3000`) to the container port (`3000`), allowing access to the frontend application from outside the container.

- `backend`
  - `image: tanviagrawal1704/21bcp218-back`
    - Specifies the Docker image to be used for the backend container. Change it to whatever name you have given
  - `ports`
    - Maps the host port (`5000`) to the container port (`5000`), allowing access to the backend application from outside the container.
  - `depends_on`
    - Specifies that the backend container depends on the `mongodb` service, ensuring that the MongoDB container is started before the backend container.

- `mongodb`
  - `image: mongo`
    - Specifies the official MongoDB Docker image.
  - `container_name: mongodb`
    - Sets a custom name for the MongoDB container.
  - `ports`
    - Maps the host port (`27017`) to the container port (`27017`), allowing access to the MongoDB instance from outside the container.
  - `environment`
    - Sets environment variables for the MongoDB container.
    - `MONGO_INITDB_DATABASE=test`: Specifies the name of the initial database to be created.
    - `MONGO_INITDB_ROOT_USERNAME=mongo_user`: Sets the root username for the MongoDB instance.
    - `MONGO_INITDB_ROOT_PASSWORD=mongo_pass`: Sets the root password for the MongoDB instance.

By using Docker Compose, you can easily start and manage all the containers required for your application with a single command.
    
            docker compose up

Check the running containers using ```docker container ls``` and you will see that docker has created 3 containers with the names mentioned in the docker compose file. To stop these containers simply run 
            
            docker compose down
and you will see that docker stops and also deletes all 3 containers.
    
