# Check The Pod
이 문서는 Pod 를 확인하는 방법에 대해 포함되어 있습니다.   
   
# Kubernetes Pod
![](https://d33wubrfki0l68.cloudfront.net/fe03f68d8ede9815184852ca2a4fd30325e5d15a/98064/docs/tutorials/kubernetes-basics/public/images/module_03_pods.svg)   
앞선 [`Create the Deployment`](/doc/create-the-deployment.md) 를 통해 `Deployment` 가 생성이 되고 나면   
Kubernetes 는 여러분의 애플리케이션 인스턴스에 Pod 를 생성했습니다.   
***Pod 는 하나 또는 그 이상의 애플리케이션 컨테이너 (도커 또는 rkt와 같은)들의 그룹을 나타내는 쿠버네티스의 추상적 개념으로 일부는 컨테이너에 대한 자원을 공유합니다.***   
   
# Check The Pod
* Pod 정보 확인
    ```bash
    $ kubectl get pod
    NAME                                READY   STATUS    RESTARTS   AGE
    flask-example-app-959c5f88d-k95wk   1/1     Running   0          24m
    ```
* Pod 성능 사용량 확인
    ```bash
    $ kubectl top pod
    NAME                                CPU(cores)   MEMORY(bytes)
    flask-example-app-959c5f88d-k95wk   1m           2Mi
    ```
* Pod 내 Container 의 log 확인
    ```bash
    $ kubectl logs flask-example-app-959c5f88d-k95wk
    ```
    실시간 확인을 위해서는 아래와 같이 `-f` 옵션을 추가합니다.   
    ```bash
    $ kubectl logs -f flask-example-app-959c5f88d-k95wk
    ```
* Pod 내 Container 에 명령어 수행
    ```bash
    $ kubectl exec -ti flask-example-app-959c5f88d-k95wk -- pwd
    /usr/src/app
    ```
* Pod 상세 정보 확인 - 1
    ```bash
    $ kubectl get pod -o wide
    NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
    flask-example-app-959c5f88d-k95wk   1/1     Running   0          24m   10.42.0.36   fastvm-centos-7-7-110   <none>           <none>
    ```
* Pod 상세 정보 확인 - 2 
    ```bash
    $ kubectl describe pod flask-example-app-959c5f88d-k95wk
    Name:         flask-example-app-959c5f88d-k95wk
    Namespace:    default
    Priority:     0
    Node:         fastvm-centos-7-7-110/192.168.200.110
    Start Time:   Wed, 09 Sep 2020 15:38:00 +0900
    Labels:       app=flask-example-app
                  pod-template-hash=959c5f88d
    Annotations:  <none>
    Status:       Running
    IP:           10.42.0.36
    IPs:
      IP:           10.42.0.36
    Controlled By:  ReplicaSet/flask-example-app-959c5f88d
    Containers:
      flask-example-app:
        Container ID:   containerd://f99d19b1ac52ced19e300a294cd93ad62a97f6259cf6e426c63c545e0918a9d8
        Image:          han0495/flask-example-app:v1
        Image ID:       docker.io/han0495/flask-example-app@sha256:f3d45e996bc86a13e1a8a363d9736e18c1501804690733b86aa02df3f59bda10
        Port:           <none>
        Host Port:      <none>
        State:          Running
          Started:      Wed, 09 Sep 2020 15:38:01 +0900
        Ready:          True
        Restart Count:  0
        Environment:    <none>
        Mounts:
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-89vwb (ro)
    Conditions:
      Type              Status
      Initialized       True
      Ready             True
      ContainersReady   True
      PodScheduled      True
    Volumes:
      default-token-89vwb:
        Type:        Secret (a volume populated by a Secret)
        SecretName:  default-token-89vwb
        Optional:    false
    QoS Class:       BestEffort
    Node-Selectors:  <none>
    Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                     node.kubernetes.io/unreachable:NoExecute for 300s
    Events:
      Type    Reason     Age        From                            Message
      ----    ------     ----       ----                            -------
      Normal  Scheduled  <unknown>  default-scheduler               Successfully assigned default/flask-example-app-959c5f88d-k95wk to fastvm-centos-7-7-110
      Normal  Pulled     29m        kubelet, fastvm-centos-7-7-110  Container image "han0495/flask-example-app:v1" already present on machine
      Normal  Created    29m        kubelet, fastvm-centos-7-7-110  Created container flask-example-app
      Normal  Started    29m        kubelet, fastvm-centos-7-7-110  Started container flask-example-app
    ```
* 현재 실행중인 Pod 의 정보를 `yaml` 형식으로 출력
    ```bash
    $ kubectl get pod flask-example-app-959c5f88d-k95wk -o yaml 
    ```
    아래와 같이 저장이 가능합니다.   
    ```bash
    $ kubectl get pod flask-example-app-959c5f88d-k95wk -o yaml > flask-example-app.yml
    ```
   
# Create Pod
아래 명령어를 통해 Pod 을 생성 할 수 있습니다.   
```bash
$ kubectl run pod-test-app --image=nginx
pod/pod-test-app created

$ kubectl get pod
NAME                                READY   STATUS    RESTARTS   AGE
flask-example-app-959c5f88d-k95wk   1/1     Running   0          38m
pod-test-app                        1/1     Running   0          66s    <<
```
   
아래와 같이 `yaml` 을 이용해서 Pod 을 생성 할 수 있습니다.   
```bash
$ cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: pod-test-app-2
  name: pod-test-app-2
  namespace: default
spec:
  containers:
  - image: nginx
    name: pod-test-app-2
EOF
```
or
```bash
$ kubectl create -f pod-test-app-2.yml
```
   
# Delete Pod
아래 명령을 통해 Pod 삭제 할 수 있습니다.   
```bash
$ kubectl delete pod pod-test-app
$ kubectl delete pod pod-test-app-2
```
   
# 연습 문제
아래 container 를 포함한 하나의 Pod 을 생성하시오.   
- nginx
- redis
- memcached
   
