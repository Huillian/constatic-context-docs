## Docker

### How to host the Discord bot project using Docker

Docker is an open-source platform that facilitates the creation, testing, and deployment of applications in containers, which are isolated and lightweight environments that contain everything needed for an application to run, ensuring consistency and portability across different development and production environments.

It is necessary to have Docker installed and basic knowledge of Docker to follow this guide!

Create a `Dockerfile` in the project root with the following content:

`Dockerfile`
```dockerfile
FROM docker.io/library/node:21.5

WORKDIR /bot

COPY ./package*.json .
RUN npm install

COPY . .

RUN npm run build

CMD [ "npm", "start" ]
```

Create a `.dockerignore` file in the project root with the following content:

`.dockerignore`
```
build
node_modules
```

Then run the command below:

```bash
docker build --pull --rm -f Dockerfile -t discord-bot .
```
The `-t` flag defines a name for the image; in this case, it will be `discord-bot`.

After that, just run a container with this image:

```bash
docker run -d --name my-bot discord-bot
```
The `--name` flag defines a name for the container; in this case, it will be `my-bot`.

That's enough; your application will be online in moments.


