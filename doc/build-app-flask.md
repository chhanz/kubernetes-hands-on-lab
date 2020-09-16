# Build APP for Flask WEBAPP
이 문서는 Kubernetes Hands on LAB 에서 활용할 Flask WEBAPP 를 Build 하는 방법에 대해 포함 되어 있습니다.   
   
# Install git
```bash
$ yum -y install git
```
   
# Clone source 
```bash
$ git clone https://github.com/chhanz/flask-example-app.git
```
   
# Check Dockerfile
해당 Source 는 아래와 같은 file 을 포함하고 있습니다.   
```console
flask-example-app/
├── Dockerfile         > Build 를 위한 Dockerfile
├── main.py            > Flask WEBAPP
├── README.md
└── requirements.txt   > Requirements Flask APP 
```
   
Dockerfile 는 아래와 같습니다.   
```docker
FROM python:3.8.5-alpine3.12
MAINTAINER cheolhee.han@ibm.com

WORKDIR /usr/src/app

COPY . .
RUN pip install --no-cache-dir -r requirements.txt

CMD [ "python", "./main.py" ]
```
   
# Build APP
아래 명령어를 통해 Build 를 수행합니다.    
- `buildah` 의 경우,
    ```bash
    $ buildah bud -t flask-test-app:v1 .
    ```
   
- `podman` 의 경우,
    ```bash
    $ podman build -t flask-test-app:v1 .
    ```
     
# TEST APP 
아래 명령어를 통해 Container 를 시작합니다.   
```bash
$ podman run -d -p 39999:80 --name test flask-test-app:v1
$ curl 127.0.0.1:39999
```
    
# Push image
DockerHub 에 image 를 Push 하기위해 아래와 같이 Login 을 합니다.   
```bash
$ buildah login docker.io
Username: < dockerhub id >
Password: < dockerhub pw >
Login Succeeded!
```
   
Push 할 image 의 TAG 를 수정합니다.   
```bash
$ buildah tag flask-test-app:v1 docker.io/<dockerhub id>/flask-test-app:v1
```
   
아래 명령어를 통해 PUSH 를 합니다.   
```bash
$ buildah push docker.io/<dockerhub id>/flask-test-app:v1
```
Push 가 완료되면 [`Dockerhub`](https://hub.docker.com/) 로 접속해서 확인합니다.   
   
# 마무리
다음 LAB 위해 Podman Container 를 종료합니다.   
```bash
$ podman rm -f $(podman ps -aq)
```
   
# 참고 문서
* [https://github.com/chhanz/flask-example-app](https://github.com/chhanz/flask-example-app)   
