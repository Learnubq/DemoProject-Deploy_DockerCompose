## Demo Project - Deploy Docker Application on a Server With Docker Compose

This demo shows the process of deploying a Docker application on a server using Docker Compose.

## Steps to Deploy Docker Application on a Server With Docker Compose

1. **Within the Docker Compose file added a new container for the "my-app" application. The file already had the MongoDB and MongoExpress containers added**

2. **Within that added a new Image and its name - the Image I pushed to the AWS ECR registry**

3. **I executed a Docker login on the development server before I could interact with AWS ECR:**

```bash
cd ~
<AWS docker login command>
```

**Don't need a Docker login to pull Mongo Images from Docker Hub**

4. **Configured the ports in the Docker Compose file. The application runs on port 3000**

5. **The env variables were inside the Dockerfile so I didn't need to configure them in the Docker Compose file**

6. **The now updated Docker Compose file would then be used on the server to deploy all the applications/services**

7. **Because I was starting the application as part of the Docker Compose file, I needed to make one change to the server.js application code for the Node.js application to connect to the MongoDB service. Wherever I saw "mongoUrlLocal" in the mongoclient code I replaced it with "mongoUrlDockerCompose".

8. **I then needed to remove the Docker container, remove the Docker Image and rebuild the Docker Image with the 1.0 tag, and push the newly built Image to the Docker repository AWS ECR:**

```bash
docker ps
docker images
docker rmi <imagetag/imageID>
docker build -t my-app:1.0 .
docker images
docker run my-app:1.0
docker ps
docker tag my-app:1.0 470633809269.dkr.ecr.eu-west-1.amazonaws.com/my-app:1.0
docker images
docker push 470633809269.dkr.ecr.eu-west-1.amazonaws.com/my-app:1.0
```

<img width="410" height="55" alt="Dockercomp1" src="https://github.com/user-attachments/assets/7c95f15c-a3c6-49e5-a5ac-498150c7c488" />

<img width="913" height="53" alt="Dockercomp2" src="https://github.com/user-attachments/assets/eb5ce4aa-0632-4c85-b087-2027bf5217ed" />

<img width="334" height="33" alt="Dockercomp3" src="https://github.com/user-attachments/assets/cddd890f-5519-44c1-a8b0-f6739a1c0c3f" />

<img width="1386" height="61" alt="Dockercomp4" src="https://github.com/user-attachments/assets/a0942483-a11e-40c3-8936-06b33b99a75b" />

<img width="1256" height="83" alt="Dockercomp5" src="https://github.com/user-attachments/assets/7dd42137-d303-466e-9913-df3a67d096b6" />

<img width="1161" height="243" alt="Dockercomp6" src="https://github.com/user-attachments/assets/1fb2e5ec-9ef8-4f0e-8940-3243899e44b5" />

<img width="547" height="135" alt="Dockercomp7" src="https://github.com/user-attachments/assets/d6b91131-25b1-447f-9345-2f0657b39926" />


9. **After confirming I had added the Docker Compose file to the project directory I then started the my-app, MongoDB and MongoExpress containers using Docker Compose:**

```bash
docker compose --file docker_compose_mongo.yaml up -d
```

10. **I then checked the MongoDB database and MongoExpress UI were running. I confirmed the "user-account" database was lost as I recreated the MongoDB container. I would be able to persist the database data using volumes if I wanted the database to remain each time I recreated the MongoDB Container.**
