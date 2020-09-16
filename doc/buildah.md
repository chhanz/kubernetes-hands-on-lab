# Build APP
이 문서는 Application 을 Build 하는 방법에 대해 설명하고 있습니다.   
   
# Buildah 란?
`Buildah` 는 `Podman` 의 Container image Build 및 push 등을 지원하는 Tool 로 `podman build` 을 사용할 경우,   
내부적으로 `Buildah` 를 이용하여 Container image 를 Build 하게 됩니다.   
또한 Buildah 를 이용하면 layer 별로 순차적으로 Build 를 수행 할 수 있습니다.   
   
# Buildah 설치
- CentOS 7
    ```bash
    $ yum install -y buildah
    ```
- CentOS 8
    ```bash
    $ dnf install -y buildah
    ```
   
# Podman 설치
- CentOS 7
    ```bash
    $ yum install -y podman
    ```
- CentOS 8
    ```bash
    $ dnf install -y podman
    ```
   
# Build APP
Build 과정을 이해하기 위해 간단한 APP 을 Container image 로 build 해보겠습니다.   
```bash
$ buildah from centos:7
$ buildah list            < CONTAINER ID 확인
CONTAINER ID  BUILDER  IMAGE ID     IMAGE NAME                       CONTAINER NAME
bff73bfb1adc     *     7e6257c9f8d8 docker.io/library/centos:7       centos-working-container
$ buildah run bff73bfb1adc -- yum -y install epel-release
$ buildah run bff73bfb1adc -- yum install sl
$ buildah commit bff73bfb1adc sl-test-app
```
   
# RUN APP
Build 된 APP 을 실행해보겠습니다.   
```bash
$ podman run sl-test-app /usr/bin/sl
```
   
화면에 무엇이 보이나요?   
   
# 마치며
위와 같이 간단하게 Container image 를 만들어보고 어떻게 image 가 만들어 지는지 확인 했습니다.   
   
# 참고 자료
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)   
