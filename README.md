# 2048 Game Kubernetes

This project deploys the 2048 game in a Kubernetes environment using Kind locally. It includes creating a Docker image of the game and running it on Kubernetes with a Deployment and NodePort service.

## 1- Game and Dockerization

The source code of the 2048 game is a web-based application and consists of HTML, CSS and JavaScript files. The source codes of this website are taken from the https://github.com/gabrielecirulli/2048 repository.

While containerizing the game, the first step was creating a Dockerfile. The Nginx image was chosen as the base to serve the game as a web application, and port 80 was exposed to allow external access. After the Dockerfile was created, the image was built and pushed to Docker Hub as erenduyuk/2048-image.

```bash
docker build . -t erenduyuk/2048-image

docker push erenduyuk/2048-image
```

## 2- Create Deployment and Service

The 2048_deployment.yaml file contains definitions for a Deployment and a Service to run the 2048 game on Kubernetes. The Deployment is configured to run two replicas of the game, with each pod created using the erenduyuk/2048-image:latest Docker image.

The selector and labels within the Deployment ensure that the service and pods are correctly matched. Each pod exposes port 80, which serves the web-based interface of the game.

The Service section is designed to make the game accessible to the outside world. The NodePort-type service allows external access to the game from outside the Kubernetes cluster. The service selects pods with the deployment2048 label and directs incoming traffic to port 80 on those pods. NodePort assigns a port (e.g., between 32000–32767) for external connections, enabling access to the game through a web browser.

## 3- Preparing Kind Environment and Deploying the Game

In the Kind environment, a cluster named 2048cluster was created, and the 2048_deployment.yaml file was applied to set up the service and deployment.

![Settings Window](https://github.com/erenduyuk/2048game_kubernetes/blob/2134298e56c378505477171ff25ec1ed15fb6f4e/screenshots/get%20cluster.png)

![Settings Window](https://github.com/erenduyuk/2048game_kubernetes/blob/2134298e56c378505477171ff25ec1ed15fb6f4e/screenshots/apply%20and%20get.png)

Afterward, since the service port was 31668, the Kind container was accessed using docker exec -it <container id> bash, and the curl command was used to test whether the website was up and running.

![Settings Window](https://github.com/erenduyuk/2048game_kubernetes/blob/2134298e56c378505477171ff25ec1ed15fb6f4e/screenshots/curl.png)

To access the website from the local computer, port forwarding was applied. This allowed access to the game from the local machine via the URL http://127.0.0.1:8080.

![Settings Window](https://github.com/erenduyuk/2048game_kubernetes/blob/2134298e56c378505477171ff25ec1ed15fb6f4e/screenshots/port%20forwading.png)

![Settings Window](https://github.com/erenduyuk/2048game_kubernetes/blob/2134298e56c378505477171ff25ec1ed15fb6f4e/screenshots/website.png)
