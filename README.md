# 🚀 docker-testapp

A simple **Node.js + MongoDB** full stack demo app containerized using Docker, with Mongo Express for database UI management.

> 👨‍💻 _Made by [Tanvir Anjom Siddique](https://tanvirsweb.github.io)_  
> 📺 _Learned from YouTube: [Apna College Official](https://www.youtube.com/@ApnaCollegeOfficial)_

> ** [Click to view detailed instructions in website](https://tanvirsweb.github.io/docker-testapp/) **

---

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

---

## 🔧 Setup Environment via Docker

> 🧠 **First-time setup** (build & run containers + create network):
> Ensure Docker Desktop is running on your system before proceeding. Open terminal in your project root directory. Then run:

```bash
docker compose -f mongodb.yaml up
```

> ✅ **To stop environment (but keep containers for later use):**

```bash
docker compose -f mongodb.yaml stop
```

> 🔁 **To restart later without rebuilding containers:**

```bash
docker compose -f mongodb.yaml start
```

> ❌ **To remove containers and images entirely (full clean):**

```bash
docker compose -f mongodb.yaml down
```

---

## 🧱 Setup MongoDB

1. Open browser: [http://localhost:8081](http://localhost:8081)
2. Create database: `apnacollege-db`
3. Click on the created database and add a new collection: `users`
4. Insert the following document manually:

   ```
   {
     "_id": ObjectId(),
     "username": "Jane Doe",
     "email": "jane@gmail.com",
     "password": "password"
   }
   ```

> 💡 `mongo-express` runs at port `8081` and uses the same Docker network to connect with `mongo`.

---

## ⚙️ Run Node.js Server

Make sure Node.js is installed on your local system.

1. Open terminal in your project root directory

2. Run the following commands:

   ```bash
   # install all dependencies
   npm install

   # run nodejs server
   node server.js
   ```

3. Visit the endpoint to fetch users:
   [http://localhost:5050/getUsers](http://localhost:5050/getUsers)

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

---
