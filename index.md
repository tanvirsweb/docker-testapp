# 🚀 docker-testapp

A simple **Node.js + MongoDB** full stack demo app containerized using Docker, with Mongo Express for database UI management.

> 👨‍💻 _Made by [Tanvir Anjom Siddique](https://tanvirsweb.github.io)_  
> 📺 _Learned from YouTube: [Apna College Official](https://www.youtube.com/@ApnaCollegeOfficial)_

> [View as website](https://tanvirsweb.github.io/docker-testapp/)

## 🐳 Docker Command Reference

| Sl. | Command                                 | Description                                       | Reason                                                          |
| --- | --------------------------------------- | ------------------------------------------------- | --------------------------------------------------------------- |
| 1   | `docker compose -f mongodb.yaml up`     | Builds (if needed) and starts containers          | First-time setup to pull images and run MongoDB & Mongo Express |
| 2   | `docker compose -f mongodb.yaml down`   | Stops and removes containers, networks, volumes   | Clean teardown to remove all Docker resources                   |
| 3   | `docker compose -f mongodb.yaml start`  | Starts existing, _already created_ containers     | Reuse previously created containers without rebuilding          |
| 4   | `docker compose -f mongodb.yaml stop`   | Gracefully stops containers without removing them | Use this to stop the environment temporarily                    |
| 5   | `docker ps`                             | Lists active containers                           | Ensures that services are running                               |
| 6   | `docker images`                         | Lists available images                            | Check if images were successfully pulled                        |
| 7   | `docker network ls`                     | Shows available Docker networks                   | Ensure Mongo and Express are on the same network                |
| 8   | `docker logs <container_name>`          | View runtime logs of a container                  | Debug startup or runtime errors                                 |
| 9   | `docker exec -it <container_name> bash` | Access shell inside a running container           | Manually inspect or interact with the container                 |
| 10  | `docker rm <container_id>`              | Remove a specific container                       | For cleanup if not using Compose                                |
| 11  | `docker rmi <image_id>`                 | Remove a specific image                           | Cleanup unused Docker images                                    |

## 🔧 Setup Environment via Docker

### 🧠 First-time setup (build & run containers + create network)

Ensure Docker Desktop is running on your system. Then open terminal in your project root and run:

```bash
docker compose -f mongodb.yaml up
```

Or run it in the background:

```bash
docker compose -f mongodb.yaml up -d
```

### ✅ To stop environment (but keep containers for later use):

```bash
docker compose -f mongodb.yaml stop
```

### 🔁 Restart later without rebuilding containers:

```bash
docker compose -f mongodb.yaml start
```

### ❌ Remove containers and images entirely (full clean):

```bash
docker compose -f mongodb.yaml down
```

## 💾 Will MongoDB Data Be Lost After Stopping Docker?

- **NO**, if you run `docker compose -f mongodb.yaml stop`, data will be retained.
- **YES**, if you run `docker compose -f mongodb.yaml down` _without named volumes_, data will be lost.

✅ To ensure persistent storage, declare volumes explicitly:

```yaml
version: "3.8"

services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: qwerty
    volumes:
      - D:/gitProjectsTanvirAnjomSiddique/All_Github_Projects/docker-testapp/data:/data/db

  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: qwerty
      ME_CONFIG_MONGODB_URL: mongodb://admin:qwerty@mongo:27017/

volumes:
  mongodb_data:
```

To remove persistent data manually:

```bash
docker volume rm mongodb_data
```

## 🧱 Setup MongoDB via Mongo Express

- Visit: [http://localhost:8081](http://localhost:8081)
- Login with:
  ```
  username: admin
  password: pass
  ```
- Create database `apnacollege-db` → Add collection `users` → Insert:

  ```json
  {
    "_id": ObjectId(),
    "username": "Jane Doe",
    "email": "jane@gmail.com",
    "password": "password"
  }
  ```

## ⚙️ Run Node.js Server

Make sure Node.js is installed.

```bash
npm install
node server.js
```

- Visit: [http://localhost:5050/index.html](http://localhost:5050/index.html)
- API: [http://localhost:5050/getUsers](http://localhost:5050/getUsers)

Expected output:

```json
[
  {
    "_id": "686f916e9c6b5f37b65bd45d",
    "username": "Jane Doe",
    "email": "jane@gmail.com",
    "password": "password"
  }
]
```

## 🐋 Dockerize Node.js App

Dockerfile structure:

```
FROM node
WORKDIR /docker-testapp
COPY . .
RUN npm install
CMD ["node", "server.js"]
```

Build Docker image:

```bash
docker build -t testapp:1.0 .
```

## ⬆️ Publish Docker Image to DockerHub

```bash
docker build -t tanviranjomsiddique/testapp .
docker login
docker push tanviranjomsiddique/testapp
```

## 💾 Docker Volumes Explained

Volumes are persistent data stores.

```bash
# Bind mount from host to container
docker run -it -v D:/gitProjectsTanvirAnjomSiddique/All_Github_Projects/docker-testapp/data:/test/data ubuntu

# Inside container
cd /test/data
echo "hello!" >> data.txt
cat data.txt
exit
```

## 🔁 Docker Volumes Commands

```bash
docker volume ls              # list all volumes
docker volume create VOL     # create named volume
docker volume rm VOL         # remove specific volume
docker volume prune          # delete all unused volumes
```

Types of Mounts:

- **Named Volumes**: `-v myVolume:/data`
- **Anonymous Volumes**: `-v /data`
- **Bind Mounts**: `-v /host/path:/container/path`

## 🌐 Docker Networks

```bash
docker network ls
docker network create myNetwork
```

Types of networks:

- **bridge**: Default local network for containers
- **host**: Shares host network (Linux only)
- **none**: No networking (fully isolated)
