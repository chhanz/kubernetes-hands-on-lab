# Expose APP
이 문서는 Service 생성에 대해 포함된 문서입니다.   
   
# Service 란?
![](https://d33wubrfki0l68.cloudfront.net/cc38b0f3c0fd94e66495e3a4198f2096cdecd3d5/ace10/docs/tutorials/kubernetes-basics/public/images/module_04_services.svg)    
Kubernetes Pod 들은 언젠가는 죽게됩니다. 실제 Pod 들은 생명주기를 갖습니다.   
워커 노드가 죽으면, 노드 상에서 동작하는 Pod 들 또한 종료됩니다.   
Kubernetes 에서 `service` 는 Pod 들에 접근 할 수 있는 정책을 정의하는 추상적 개념입니다.   
+ `ClusterIP` (기본값)
    - 클러스터 내에서 내부 IP 에 대해 서비스를 노출합니다. 이 방식은 클러스터 내에서만 서비스가 접근될 수 있도록 합니다.
+ `NodePort`
    - NAT가 이용되는 클러스터 내에서 각각 선택된 노드들의 동일한 포트에 서비스를 노출 시켜줍니다.   
    `<NodeIP>`:`<NodePort>`를 이용하여 클러스터 외부로부터 서비스가 접근할 수 있도해줍니다. ClusterIP 의 상위 집합입니다.
+ `LoadBalancer`
    - (지원 가능한 경우) 기존 클라우드에서 외부용 로드밸런서를 생성하고 고정된 공인 IP를 할당합니다.   
    NodePort 의 상위 집합입니다.
+ `ExternalName`
    - 이름으로 CNAME 레코드를 반환함으로써 임의의 이름(Spec 에서 externalName 으로 명시)을 이용하여 서비스를 노출시켜줍니다.   
    프록시는 사용되지 않습니다. 이 방식은 kube-dns 버전 1.7 이상에서 지원 가능합니다.
    - 외부 서비스를 쿠버네티스 내부에서 호출 하고자 할때 사용할 수 있습니다.
# Expose APP
아래 명령어를 통해 APP 을 외부에 노출 할 수 있습니다.   
***현재 Hands on 환경은 `LoadBalancer` or `Ingress` 가 사용이 불가능하므로 Nodeport 를 이용하여 TEST 해보도록 하겠습니다.***    
   
* `expose` 옵션으로 생성
    ```bash
    $ kubectl expose deployment flask-example-app --port 80
    ```
* `create`옵션으로 생성    
    ```bash
    $ kubectl create service clusterip flask-example-app --tcp=80
    ```
위와 명령으로 생성하는 경우, 기본적으로 `ClusterIP` 로 생성이 됩니다.   
   
아래 명령을 통해 `ClusterIP` 를 `NodePort` 로 변경하도록 하겠습니다.   
```bash
$ kubectl edit service flask-example-app
```
   
해당 `yaml` 구문에서 `spec` 부분을 보면 아래와 같습니다.   
```yaml
...
spec:
  clusterIP: 10.43.65.110
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: flask-example-app
  sessionAffinity: None
  type: ClusterIP              <<<
...
```
위와 같이 `ClusterIP` 부분은 `NodePort` 로 변경하고 저장을 합니다.   
(저장 및 종료, `VIM` or `VI` 과 동일함.)   
   
`NodePort` 로 변경이 되었는지 확인합니다.
```bash
kubectl get service
NAME                TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes          ClusterIP   10.43.0.1      <none>        443/TCP        99m
flask-example-app   NodePort    10.43.65.110   <none>        80:32296/TCP   3m26s
```
위와 같이 `32296` 로 Port 할당된 것을 확인 할 수 있습니다.   
(해당 Port 할당은 따로 yaml 에서 지정을 안하면 랜덤 Mapping 입니다.)   
   
# TEST SERVICE
```bash
$ curl 192.168.200.110:32296
 Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
```
   
