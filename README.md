# web3-docker-quickstart
Docker containers can help simplify many of the workflows used in web3. They can be especially useful when you need to run an Ethereum client (such as geth) or network node (such as a Filecoin Lotus node or a Chainlink node), without having to do all of the pre-configuration yourself. 

## Installing Docker
The best option to install Docker for personal use is by downloading [Docker Desktop](https://docs.docker.com/desktop/). Docker Desktop is supported on Windows (including support for WSL2), Mac, and Linux. It also has a UI that makes many aspects of building and using containers easier. 


## Finding Docker Images
### Docker Hub
Many projects maintain Docker images on [Docker Hub](https://hub.docker.com/). Images hosted on Docker Hub are free to download and install, and are the most common place to find official and community-maintained Docker images. 


### Github Repos
Many projects will ship a Docker image with their Github repository. The image is created by building a `Dockerfile` available from a Github repository by following the instructions in the project's README. An example can be seen in the [Geth Github repository](https://github.com/ethereum/go-ethereum#docker-quick-start). 


## Building Containers from Docker Hub
When pulling from Docker Hub, you can pull an image and run a container with it in a single command -
`docker run -d --name ethereum-node -p 8545:8545 -p 30303:30303 ethereum/client-go`


The `docker` command acts as our starting point for any Docker operations we'll perform. The `run` command tells Docker to create a container using the `ethereum/client-go` image. The `-p` flag is used to specify what ports on our local machine the container should listen on, allowing us to interact with those ports on the container. The `--name` flag gives a name to the container so that we can identify it with other Docker commands later. The `-d` flag runs the container in "detached" mode, which means it will run without connecting our active terminal to the container. 


In order to connect to our geth node in our terminal, we can run the command below:
`docker exec -i ethereum-node /bin/sh` 


## Building Containers from Dockerfile
When a Dockerfile is provided, you must first build the Docker image locally, then run a container with it. Let's use the Geth repo as our example.
```
git clone git@github.com:ethereum/go-ethereum.git
cd go-ethereum
docker build . -t geth
docker run -d --name test-geth geth 
```

## Building Your Own Images
When writing your own software, you can turn it into a Docker image that can be used to easily distribute it to others. For our example, we have a very simple Express server in `src/server.ts` that we'll use to demonstrate the process.

First, we'll want to build our image: 
`docker build -t docker-express .`

We should see output similar to this: 
```
[+] Building 9.3s (9/9) FINISHED                                                                                 
 => [internal] load .dockerignore                                                                           0.0s
 => => transferring context: 2B                                                                             0.0s
 => [internal] load build definition from Dockerfile                                                        0.0s
 => => transferring dockerfile: 223B                                                                        0.0s
 => [internal] load metadata for docker.io/library/node:16.20                                               1.6s
 => CACHED [1/4] FROM docker.io/library/node:16.20@sha256:466d0a05ecb1e5b9890960592311fa10c2bc6012fc27dbfd  0.0s
 => [internal] load build context                                                                           0.4s
 => => transferring context: 960.98kB                                                                       0.4s
 => [2/4] COPY . /opt/express                                                                               0.8s
 => [3/4] WORKDIR /opt/express                                                                              0.0s
 => [4/4] RUN yarn install                                                                                  5.6s
 => exporting to image                                                                                      1.0s
 => => exporting layers                                                                                     1.0s
 => => writing image sha256:9320eb3a322abe61c58ba19d191944fd2bb05a59da6798dcd6b3fbaad03d2ed8                0.0s 
 => => naming to docker.io/library/docker-express 
```

Then, we can create a container using our new image. We'll want to make sure to use the `-d` flag to run it in the background, and the `-p` flag to expose port 8080 on localhost. The final command will look like this: 
`docker run -d -p 8080:8080 --name test-express docker-express`

Now that our container is running, we should be able to run a command to test that it's up:

`curl localhost:8080`

As you can see, our Express server answers the request, just like it would if we had simply run `yarn start` to start our server! 
