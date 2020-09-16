# Update APP
이 문서는 Rolling Update / Rollback APP 에 대한 방법을 포함하고 있습니다.   
   
# Update Source
아래와 같이 Source 를 UPDATE 가 되었습니다.   
__신규 Build 및 Image Push 는 [`build app flask`](/doc/build-app-flask.md) 를 확인하여 진행합니다.__   
   
```diff
diff --git a/main.py b/main.py
index 2b9f022..9b347b4 100755
--- a/main.py
+++ b/main.py
@@ -8,7 +8,7 @@ podname = os.uname()[1]

 @app.route("/")
 def index():
-    return " Container LAB | POD Working : " + podname + " | v=1\n"
+    return " Container LAB | POD Working : " + podname + " | v=2\n"

 if __name__ == "__main__":
     app.run(host="0.0.0.0", port="80")
```
   
시간 관계상 Push 된 이미지를 사용할 것입니다.   
* Container Image Tag : `han0495/flask-example-app:v2`   
   
# Rolling Update 
아래 명령을 통해 기존에 `v1` 에서 `v2` 로 image 를 변경하여 Rolling Update 해보겠습니다.   
* 기존 정보
    ```bash
    $ kubectl describe deployments flask-example-app
    ...
    Pod Template:
      Labels:  app=flask-example-app
      Containers:
       flask-example-app:
        Image:        han0495/flask-example-app:v1    <<
        Port:         <none>
        Host Port:    <none>
        Environment:  <none>
        Mounts:       <none>
      Volumes:        <none>
    ```
* Change image
    ```bash
    $ kubectl set image deployments flask-example-app flask-example-app=han0495/flask-example-app:v2
    ````
* Rolling Update 상태 확인
    + 상태 확인
    ```bash
    $ kubectl rollout status deployments flask-example-app
    Waiting for deployment "flask-example-app" rollout to finish: 1 old replicas are pending termination...
    Waiting for deployment "flask-example-app" rollout to finish: 1 old replicas are pending termination...
    Waiting for deployment "flask-example-app" rollout to finish: 1 old replicas are pending termination...
    deployment "flask-example-app" successfully rolled out
    ```
    + 실제 서비스 확인  
    ```bash
    $ while true; do curl 192.168.200.110:32296; done
     Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-677cbdd665-mhpqs | v=2
     Container LAB | POD Working : flask-example-app-677cbdd665-wwmf8 | v=2
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-677cbdd665-mhpqs | v=2
     Container LAB | POD Working : flask-example-app-677cbdd665-wwmf8 | v=2
     Container LAB | POD Working : flask-example-app-677cbdd665-mhpqs | v=2
     Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
     ...
    ```
* 변경 확인
    ```bash
    $ kubectl describe deployments flask-example-app
    Name:                   flask-example-app
    Namespace:              default
    CreationTimestamp:      Wed, 09 Sep 2020 15:37:59 +0900
    Labels:                 app=flask-example-app
    Annotations:            deployment.kubernetes.io/revision: 2
    Selector:               app=flask-example-app
    Replicas:               5 desired | 5 updated | 5 total | 5 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
      Labels:  app=flask-example-app
      Containers:
       flask-example-app:
        Image:        han0495/flask-example-app:v2       <<< 변경됨
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
    NewReplicaSet:   flask-example-app-677cbdd665 (5/5 replicas created)
    Events:
      Type    Reason             Age    From                   Message
      ----    ------             ----   ----                   -------
      Normal  ScalingReplicaSet  19m    deployment-controller  Scaled up replica set flask-example-app-959c5f88d to 5
      Normal  ScalingReplicaSet  3m31s  deployment-controller  Scaled up replica set flask-example-app-677cbdd665 to 2
      Normal  ScalingReplicaSet  3m31s  deployment-controller  Scaled down replica set flask-example-app-959c5f88d to 4
      Normal  ScalingReplicaSet  3m30s  deployment-controller  Scaled up replica set flask-example-app-677cbdd665 to 3
      Normal  ScalingReplicaSet  2m59s  deployment-controller  Scaled down replica set flask-example-app-959c5f88d to 3
      Normal  ScalingReplicaSet  2m58s  deployment-controller  Scaled up replica set flask-example-app-677cbdd665 to 4
      Normal  ScalingReplicaSet  2m56s  deployment-controller  Scaled down replica set flask-example-app-959c5f88d to 2
      Normal  ScalingReplicaSet  2m55s  deployment-controller  Scaled up replica set flask-example-app-677cbdd665 to 5
      Normal  ScalingReplicaSet  2m50s  deployment-controller  Scaled down replica set flask-example-app-959c5f88d to 1
      Normal  ScalingReplicaSet  2m3s   deployment-controller  Scaled down replica set flask-example-app-959c5f88d to 0
    ```
   
# Rollback APP
Rollback 은 배포된 APP 에 문제가 있을 때, 다시 이전 이미지로 배포 해야 되는 경우 사용하는 방법입니다.   
(꼭 문제가 있어야 사용이 가능한 것은 아님, 주 목적은 이전 버전으로 Rollback 하기 위함입니다.)   
   
먼저 아래 명령어를 통해 Rolling Update 를 다시 진행합니다.   
```bash
$ kubectl set image deployments flask-example-app flask-example-app=han0495/flask-example-app:v99
```
   
어떤 일이 발생될까요?   
   
```bash
$ kubectl get pod
NAME                                 READY   STATUS             RESTARTS   AGE
flask-example-app-677cbdd665-mhpqs   1/1     Running            0          8m33s
flask-example-app-677cbdd665-wwmf8   1/1     Running            0          8m33s
flask-example-app-677cbdd665-x95tn   1/1     Running            0          8m31s
flask-example-app-677cbdd665-754fq   1/1     Running            0          7m59s
flask-example-app-79f5875dcf-26pvs   0/1     ImagePullBackOff   0          38s
flask-example-app-79f5875dcf-thxmx   0/1     ImagePullBackOff   0          38s
flask-example-app-79f5875dcf-jmwpv   0/1     ErrImagePull       0          38s
```
Image 에 `v99` 라는 TAG 는 없으므로 ImagePull 에서 Error 가 발생되면서 서비스 배포가 정상적으로 진행이 안됩니다.   

* Rollback 진행
    + 현재 상태 확인
    ```bash
    $ kubectl rollout history deployment flask-example-app
    deployment.apps/flask-example-app
    REVISION  CHANGE-CAUSE
    1         <none>
    2         <none>
    3         <none>   < 현재 버전
    ``` 
    + Rollback 진행
    ```bash
    $ kubectl rollout undo deployment flask-example-app
    deployment.apps/flask-example-app rolled back
    ```
    + Rollback 확인
    ```bash
    $ kubectl describe deployments.apps flask-example-app
    Name:                   flask-example-app
    Namespace:              default
    CreationTimestamp:      Wed, 09 Sep 2020 15:37:59 +0900
    Labels:                 app=flask-example-app
    Annotations:            deployment.kubernetes.io/revision: 4
    Selector:               app=flask-example-app
    Replicas:               5 desired | 5 updated | 5 total | 5 available | 0 unavailable
    StrategyType:           RollingUpdate
    MinReadySeconds:        0
    RollingUpdateStrategy:  25% max unavailable, 25% max surge
    Pod Template:
      Labels:  app=flask-example-app
      Containers:
       flask-example-app:
        Image:        han0495/flask-example-app:v2    <<
        Port:         <none>
        Host Port:    <none>
    ```
   
* `v1` 로 돌아가기
    + Rollback 수행
    ```bash
    $ kubectl rollout undo deployment flask-example-app --to-revision=1
    ```
    + 서비스 확인
    ```bash
    $ while true; do curl 192.168.200.110:32296; done
     Container LAB | POD Working : flask-example-app-959c5f88d-rjbqc | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-sgrfk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-sgrfk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-rjbqc | v=1
     Container LAB | POD Working : flask-example-app-677cbdd665-wwmf8 | v=2  <<
     Container LAB | POD Working : flask-example-app-959c5f88d-sgrfk | v=1
     Container LAB | POD Working : flask-example-app-677cbdd665-wwmf8 | v=2  <<
     Container LAB | POD Working : flask-example-app-959c5f88d-rjbqc | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-l7zjp | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-sgrfk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-l7zjp | v=1
    ``` 
    + Rollback 완료
    ```bash
    $ kubectl describe deployments.apps flask-example-app
    Name:                   flask-example-app
    Namespace:              default
    CreationTimestamp:      Wed, 09 Sep 2020 15:37:59 +0900
    Labels:                 app=flask-example-app
    Annotations:            deployment.kubernetes.io/revision: 5
    Selector:               app=flask-example-app
    Replicas:               5 desired | 5 updated | 5 total | 5 available | 0 unavailable
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
    ```
