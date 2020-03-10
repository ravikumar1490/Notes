# Docker

https://docs.docker.com/docker-for-windows/ 
Micro services and cloud native

## Commands

- **Listing docker images**

  `docker image ls` 

- **Listing docker containers**

  `docker container ls --all`

- **Run sample app**

  `docker run hello-world`

  On a specfic port of a web server

  `docker run --detach --publish 80:80 --name webserver nginx`

  On a container web server

  `docker container start webserver`

  `docker container run -d --name web2 -p 8080:8080 ravikumar1490/ctr-demo:1`

- **Stop webserver**

  `docker stop webserver`

- **Build docker image**

  `docker image build -t mytag .`
  `docker image build -t ravikumar1490/ctr-demo:1 .`

- **Push to Registry**

  `docker  image push ravikumar1490/ctr-demo:1` 




