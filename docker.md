1. difference between vm and docker?
ans. VMs emulate entire hardware environments and run their own guest OS, making them heavier and slower to start. Docker containers share the host OS kernel and isolate processes at the OS level, making them lightweight and fast. Containers are ideal for microservices and DevOps pipelines due to their speed, portability, and efficient resource usage, while VMs are better suited for traditional applications or when strong OS-level isolation is needed.”

2. cmd and entrypoint?
Purpose:	Sets default command & args	           Sets fixed command to run
CMD command can be overriden by any command on terminal ex: 
*ENTRYPOINT can't overriden
*you cant overridden entry point, if you try to do it will do and append to entry point-->  docker run entry:v2
* if user not giving any options at entrypoint, cmd can supply default arguments to entrypoint.
* user can always overridden cmd through terminal.

3. run and cmd?
RUN--> to install or configure the packages or imagesinside base os ,it will run at image creation
CMD--> it is run the container,it will work only at container creation

4. copy and run?
ADD->add the local files/directories  to the image similarly COPY but it has some extra features i.e directly download files of  urls from the internet to your image  and directly untar the files to the image 

5. how to keep base os size small?
“To minimize Docker image size, I use Alpine or scratch base images, multi-stage builds, clean up package caches, and avoid unnecessary dependencies, combine layers. This ensures faster builds, quicker deployments, and better security.”

6. how to persist the data in docker?
a. unamed volumes 
b. named volumes
unames volume:
-v hostpath:container path
we have to create directory and mount that container path

named volume:
docker volume create nginx
-v nginx:container path.

To persist data in Docker, I use named volumes which are managed by Docker and survive container deletion. I can mount them using -v volume_name:/path/in/container. For local dev, I might use bind mounts, but they’re less portable. In production, named volumes are safer and easier to back up or migrate."

7. networking in docker?
a. default
b. bridge

bridge	       Default network for standalone containers; allows communication via IP
host	         Shares the host network stack; no isolation
               The container shares the host’s network stack directly.
               No separate network namespace is created.
               The container uses the host's IP address, not its own
none	         No networking for containers; fully isolated container
“The none network isolates the container entirely from other networks. It’s useful for scenarios where no network access is required, such as strict security environments or debugging resource-limited apps.”

overlay	         Used with Docker Swarm/Kubernetes for cross-host communication
macvlan	         Assigns containers a MAC address from the physical network


docker network ls                  # List networks
docker network inspect bridge      # Inspect default bridge network

Run a container with default bridge:
docker run -d --name web nginx

Each container gets its own IP inside the bridge network. To let them talk by name:

Create a Custom Bridge Network
Custom bridge networks support container name resolution (via DNS).

docker network create mynetwork

docker run -d --name app1 --network mynetwork nginx
docker run -d --name app2 --network mynetwork busybox sleep 9999

# Now inside app2:
docker exec -it app2 ping app1

✅ app2 can ping app1 using the container name.

8. best practices for docker image creation?

1.  Use Official Images
2. Write a coustmize Dockerfile
3. keep containers small.
  how to known the size of os: docker pull node:14(it takes more size)----for cart
  so alpine is very less in size image.so u can reduce the size.
 4. .dockerignore.-->unneccasary files and directories from the build context.
 5. use multi-stage builds.
 
 java: in this we will use one docker file as a builder and we will be copy the output in second dockerfile so obiviouslr first image will be removed.we will use this in the java environment.
 5. update dependencies--developer job
 6. limit privileges.---(as part of scuritybest approach)
 if u give root acess to the user they can get acess to the underlying sever,they can access other containers,they can do wt ever things they want.keeping your own network also as part of security.
 
 IF i dont  want to use root user i want to use user so creating user and group -->cart check
 7. isolate containers: keeping your own network also a part of security.
 8. monitor and log: to persist the log generally use elk
 9. automate the container management---docker-compose up -d but it is not best approach go to kuberenetes.
 10. backup data: use volumes.
 11. use environment variables.
 12. security scanning.--twist lock
 13. implement ci/cd.
 14. follow docker compose best paractice.
 15. document your containers.


 9.  What is multi-stage build and why use it?
A multi-stage build allows you to use one container to build your app and another to run it — reducing image size and attack surface.

10.  What’s the difference between Docker Swarm and Kubernetes?
Feature	                Docker Swarm	               Kubernetes
Ease of setup	        Simple (1-line init)	           More complex (YAML, components)
Scalability          	Medium	                           High (web-scale)
Community support	   Limited (deprecated focus)	       Strong (cloud-native standard)
Features	           Built-in load balancing, secrets	    Advanced autoscaling, health checks
Networking	           Overlay built-in	                    CNI plugins, flexible
Use cases	           Simple clusters, dev/testing      	Production-grade deployments


11. docker tags?

1. --no-cache: build docker with no cache
2. if docker file not in current directory use: -f complete docker folder name and file name(docker file different directory)
3. latest commit of the docker repository u need to tag commit id use:
docker build -t docker2:$(git-rev-parse --short HEAD).(git commit as a docker tag)
4. date as docker tag: docker build -t broadgame:release-$(date +%y-%m-%d)
5. setup for ram and cpu for container: docker run --memory=100m --cpus=1.5m broad:latest 
6. run commands as non root user in container: docker run -u 1001:1001 broad:latest
7. launch container read only: docker run --read-only -v /tmp --tmpfs /tmp -d -p 443:3030 bradgame:latest 
8. setup automatic container restart: docker run -d --restart=on-failure nginx -c "sleep 2 && exit 1"
9. health check of the containers: HEALTHCHECK CMD curl --fail (url) || exit1 (pass instruction in dockerfile)
10. tailing of logs of container: docker logs -f --tail 50
11. check specific process runnimng in the container: 
docker exec -it container-id  bin/bash -c "ps -aux | grep java"
12. running containers in restricted mode: docker run --pids-limit 100 --memory=100m --read-only alphine
13. container logs on host machine:docker run -v $(pwd)/logs:/var/log/nginx nginx
14. deleting dangling images: docker run system prune -a --volumes -f 
if volumes are named then it is not worked.
untaged images: docker rmi $(docker images -f "dangling=true -q)
15. delete all containers: docker rm -f $(docker ps -aq)
16. list images created after specific image: docker image ls -f "since=xyz:v1"
17. rename containers: docker rename actualname renamename
18. restart policy setup in container: docker update --restart=always conatinername
19. pause and unpause the container: docker pause containername. same like unpause
20. docker diff command: docker diff containername
21. export docker images: docker save nginx:v1 > python.tar
22. import docker images into tar files: docker load < python.jar
23. copy files from container to host container: docker cp cname:absolutepath of file host conatiner directory/testfilename
docker cp abc:/usr/src/app/adhi.txt ./adhi.txt
24. real time containers resourse usage like ram and cpu: docker stats --no-stream

