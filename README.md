### Dockerfile.dev

    FROM node
    WORKDIR /app
    COPY package.json .
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["npm", "start"]

### Dockerfile.prod

    FROM node:alpine as build

    WORKDIR /app
    COPY package.json .

    RUN npm install
    COPY . .
    ENV REACT_APP_NAME=Tonya
    EXPOSE 3000
    RUN npm run build

    FROM nginx
    COPY --from=build /app/build /usr/share/nginx/html

docker build -t react-image .

docker run -d --name react-app react-image
docker run -v -d -p 3000:3000 --name react-app react-image
docker run -v $(pwd)/src:/app/src -d --name react-app react-image
docker run -v $(pwd)/src:/app/src:ro -d --name react-app react-image (readonly)
docker run --env-file ./.env -v $(pwd)/src:/app/src -d --name react-app react-image

docker build --target build -f  Dockerfile.prod -t mulyi-stage-example

docker image rm ID(name) -f

docker ps

docker image ls

docker exes -it react-app bash
docker exes -it react-app /bin/sh
touch
exit

docker-compose up (down)

docker-compose -f docker-compose.yml -f docker-compose-prod.yml up -d --build
docker-compose -f docker-compose.yml -f docker-compose-prod.yml down


### docker-compose.yml

    version: "3"
    services:
    react-app:
        # stdin_open: true
        # tty: true
        build: .
        ports:
        - "3000:3000"
        volumes:
        - ./src:/app/src
        environment:
        - REACT_APP_NAME=Tonya
        # env_file:
        # - ./.env

### .dockerignore

    node_modules
    Dockerfile
    build
    .dockerignore
    .git
    .gitignore
    .env
