# Create the Deployment
이 문서는 Kubernetes 의 Deployment 를 생성하는 방법에 대해 포함되어 있습니다.   
   
# Deployment 란?
![](https://d33wubrfki0l68.cloudfront.net/152c845f25df8e69dd24dd7b0836a289747e258a/4a1d2/docs/tutorials/kubernetes-basics/public/images/module_02_first_app.svg)   
`Deployment`는 Kubernetes 가 애플리케이션의 인스턴스를 어떻게 생성하고 업데이트해야 하는지를 지시합니다.   
`Deployment`가 만들어지면, Kubernetes Master 가 해당 `Deployment` 에 포함된 애플리케이션 인스턴스가 클러스터의 개별 노드에서 실행되도록 스케줄합니다.   
   
# Create Deployment 
아래 명령어와 같이 수행합니다.   
```bash
$ kubectl create deployment --image=han0495/flask-example-app:v1 flask-example-app
```
   
# Check Deployment 
Deployment 가 생성되고 Pod 가 정상적으로 실행중인지 확인합니다.   
```bash
$ kubectl get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/flask-example-app-959c5f88d-k95wk   1/1     Running   0          4s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   5m37s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/flask-example-app   1/1     1            1           5s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/flask-example-app-959c5f88d   1         1         1       5s
```
   
# Detail Check Deployment 
실행중인 Deployment 를 자세히 확인 해보겠습니다.   
```bash
$ kubectl describe deployment flask-example-app
Name:                   flask-example-app
Namespace:              default
CreationTimestamp:      Wed, 09 Sep 2020 15:37:59 +0900
Labels:                 app=flask-example-app
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=flask-example-app
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=flask-example-app
  Containers:
   flask-example-app:
    Image:        han0495/flask-example-app:v1
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   flask-example-app-959c5f88d (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  7m21s  deployment-controller  Scaled up replica set flask-example-app-959c5f88d to 1
```
   
# 참고 자료
* [https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)    