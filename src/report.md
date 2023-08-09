# Simple Docker

## Part 1. Ready docker

- Take the official docker-image from **nginx** and download his with `docker pull`
```
sudo docker pull nginx
```

<img src="./ss/part1.1.png" alt="docker_pull" width="500"/>

- Check for a docker-image via `docker images`
```
sudo docker images
```

<img src="./ss/part1.2.png" alt="docker_images" width="500"/>

- Launch the docker-image via `docker run -d [image_id|repository]`
```
sudo docker run -d 021283c8eb95
```

<img src="./ss/part1.3.png" alt="docker_run_1" width="500"/>

- Check for a docker-image launched
```
sudo docker ps
```

<img src="./ss/part1.4.png" alt="docker_ps_1" width="600"/>

- Watch info about container
```
sudo docker inspect bb8b08471fce
```

<img src="./ss/part1.5.png" alt="docker_inspect" width="600"/>

- By the output command define and place it in the report:
	- container size
	
	<img src="./ss/part1.6.png" alt="ip" width="300"/>
	
	- lists of mapped ports
	
	<img src="./ss/part1.7.png" alt="ip" width="300"/>
	
	- container ip
	
	<img src="./ss/part1.8.png" alt="ip" width="300"/>

- Stop the docker-image
```
sudo docker stop bb8b08471fce
```

<img src="./ss/part1.9.png" alt="docker_stop" width="500"/>

- Check for a docker-image stopped
```
sudo docker ps
```

<img src="./ss/part1.10.png" alt="docker_ps_2" width="500"/>

- Launch the docker with 80 and 443 port in container, which mapped on there ports on a local machine, via *run*
```
sudo docker run -d -p 80:80 -p 443:443 021283c8eb95
```

<img src="./ss/part1.11.png" alt="docker_run2" width="600"/>

- Check that the start page **nginx** is available in browser at *localhost:80*
```
curl localhost:80
```

<img src="./ss/part1.12.png" alt="localhost" width="500"/>

- Restart docker via `docker restart [container_id|container_name]`

- Check in any way that the container was started on the local machine, using command *run*
```
STATUS Up 2 seconds
```
<img src="./ss/part1.13.png" alt="docker_restart_and_check" width="500"/>


## Part 2. Container operations 

- Read config file *nginx.conf* inside the docker container via *exec*
```
sudo docker exec 93906bf9857a cat /etc/nginx/nginx.conf
```

<img src="./ss/part2.1.png" alt="nginx.conf" width="500"/>

- Create file on the local machine *nginx.conf*
```
mkdir nginx
```
```
sudo docker exec 93906bf9857a cat /etc/nginx/nginx.conf > /home/student/nginx/nginx.conf
```
```
cat nginx/nginx.conf
```

<img src="./ss/part2.2.png" alt="nginx.conf_loc_create" width="300"/>

- Configure it along the path */status* return of the server status page **nginx**
<img src="./ss/part2.3.png" alt="nginx.conf_loc" width="500"/>

- Copy created file *nginx.conf* inside the docker-image via `docker cp`
```
sudo docker cp nginx.conf 93906bf9857a:/etc/nginx/
```

<img src="./ss/part2.4.png" alt="docker cp" width="600"/>

- Restart **nginx** inside the docker-image via *exec*
```
sudo docker exec 93906bf9857a nginx -s reload
```

<img src="./ss/part2.5.png" alt="docker_restart" width="500"/>

- Check that at *localhost:80/status* given page with server status **nginx**
```
curl localhost:80/status
```

<img src="./ss/part2.6.png" alt="localhost:80/status" width="300"/>

- Export container in the *container.tar* file via *export*
```
sudo docker export 93906bf9857a > container.tar
```
<img src="./ss/part2.7.png" alt="docker export run" width="600"/>

- Stop container
```
sudo docker stop 93906bf9857a
```
<img src="./ss/part2.8.png" alt="docker stop run" width="600"/>

- Delete image via `docker rmi [image_id|repository]`, without removing the containers before that
```
sudo docker rmi -f 021283c8eb95
```

<img src="./ss/part2.9.png" alt="docker rmi" width="600"/>

- Delete stopped container 
```
sudo docker rm 93906bf9857a
```

<img src="./ss/part2.10.png" alt="docker_rm" width="500"/>

- Import container back via *import*
```
sudo docker import -c 'CMD ["nginx", "-g", "daemon off;"]' container.tar
```

<img src="./ss/part2.11.png" alt="docker_import" width="600"/>

- Start imported container
```
sudo docker images
```

<img src="./ss/part2.12.png" alt="docker_start" width="600"/>

- Check that at *localhost:80/status* given page with server status **nginx**
```
curl localhost:80/status
```

<img src="./ss/part2.13.png" alt="localhost:80/status" width="300"/>


## Part 3. Mini web-server

- Write mini-server on **C** and **FastCgi**, which will return simple page with `Hello World!`
<img src="./ss/part3.1.png" alt="fastcgi code" width="500"/> 

- Start written mini-server via *spawn-fcgi* on 8080 port
```
sudo docker run -d -p 81:81 --name task3 021283c8eb95
```

<img src="./ss/part3.2.png" alt="starting server" width="500"/> \
```
sudo docker cp main.c task3:home/
```

<img src="./ss/part3.3.png" alt="cp main.c" width="500"/> \
```
sudo docker exec -it task3 bash
```

<img src="./ss/part3.4.png" alt="exec task3" width="500"/> \
```
gcc /home/main.c -lfcgi -o server
```

<img src="./ss/part3.5.png" alt="server is working" width="500"/> 

- Write your *nginx.conf*, which will proxy all request with 81 port on *127.0.0.1:8080*
<img src="./ss/part3.6.png" alt="nginx.conf" width="500"/> 

- Check that in browser on *localhost:81* given written urself page
<img src="./ss/part38.png" alt="localhost:81" width="500"/> 

- Put *nginx.conf* file on the way *./nginx/nginx.conf* (that will need later)
```
sudo docker cp nginx.conf task3:/etc/nginx/
```

<img src="./ss/part3.7.png" alt="cp nginx.conf" width="500"/> 


## Part 4. Your docker

- Write your docker-image, which:
	1) Collect sources mini-server on FastCgi from Part 3
	2) Start him on 8080 port
	3) Copy inside image written *./nginx/nginx.conf*
	4) Start **nginx**.
  - Dockerfile:
  
  	<img src="./ss/part4.1.png" alt="dockerfile" width="500"/>
  	
  - script.sh:
  
  	<img src="./ss/part4.2.png" alt="script.sh" width="300"/>
  	
- Collect written docker-image via `docker build` at the same time specifyin the name and tag
```
 sudo docker build -f part_4/Dockerfile -t part_4:1.0 
```
 
- Check via `docker images`, that everything collect correct 
<img src="./ss/part4.3.png" alt="docker_images" width="500"/>

- Start assembled docker-image with 81 mapping and 80 port on local machine and mapping folder *./nginx* inside container on address, when lay configure **nginx** files
```
sudo docker run -d -p 80:81 -v /home/student/nginx/nginx.conf:/etc/nginx/nginx.conf --name container part_4:1.0
```
<img src="./ss/part4.4.png" alt="docker_run" width="600"/>

- Check that on localhost:80 available the page written mini-server
<img src="./ss/part4.5.png" alt="localhost:80" width="300"/>

- Finish writting in *./nginx/nginx.conf* page proxying */status*, on which to give **nginx** server status 
<img src="./ss/part4.6.png" alt="/status" width="300"/>

- Restart docker-image
```
 sudo docker restart container
```
```
 sudo docker exec container cat /etc/nginx/nginx.conf
```

<img src="./ss/part4.7.png" alt="nginx.conf" width="500"/>

- Check that what now on the *localhost:80/status* given page with **nginx** status 
<img src="./ss/part4.8.png" alt="localhost:80/status" width="300"/>

## Part 5. **Dockle**

- Download Dockle
- Created install_dockle.sh, which contains inside next code:
```
#!/bin/bash

VERSION=$(
curl --silent "https://api.github.com/repos/goodwithtech/dockle/releases/latest" | \
grep '"tag_name":' | \
sed -E 's/.*"v([^"]+)".*/\1/' \
) && curl -L -o dockle.deb https://github.com/goodwithtech/dockle/releases/download/v${VERSION}/dockle_${VERSION}_Linux-64bit.deb
```

- and then ill use this command:
```
sudo dpkg -i dockle.deb && rm dockle.deb
```

- Scan image from lastest task via `dockle [image_id|repository]`
<img src="./ss/part5.1.png" alt="dockle1" width="600"/>

- Fix image so that on check via **dockle** there are no errors

<img src="./ss/part5.2.png" alt="dockle2" width="600"/> \
<img src="./ss/part5.3.png" alt="dockerfile" width="300"/>


## Part 6. Базовый **Docker Compose**

- Download docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

- Write *docker-compose.yml* file, with the help of:
    1) Raise docker container for Part 5 _(he is should work in local network, that is dont need using instruction **EXPOSE** and mapping ports on the local machine)_
    
	1.1) Mapping 8080 port second container on the 80 port local machine

	<img src="./ss/part6.1.png" alt="port8080" width="300"/>
    
    2) Raise docker container with **nginx**, which will proxying all request from 8080 port on the 81 port first container

	<img src="./ss/part6.2.png" alt="port8080" width="300"/>

- Collect and start project with commands `docker-compose build` and `docker-compose up`

<img src="./ss/part6.5.png" alt="docker-compose" width="500"/> \
<img src="./ss/part6.6.png" alt="docker-compose" width="500"/>

- Check that in browser on the *localhost:80* given written urself page, like later 
<img src="./ss/part6.7.png" alt="localhost:80" width="300"/>

