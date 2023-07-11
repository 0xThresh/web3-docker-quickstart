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

## Building Containers from Dockerfile
When a Dockerfile is provided, you must first build the Docker image locally, then run a container with it. 
```
cd <local repo path with Dockerfile>
docker build . -t <image name> 
docker run <image name>
```

