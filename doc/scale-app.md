# Scale APP
이 문서는 Pod 을 Scale-out 하는 방법에 대해 포함되어 있습니다.   
   
# Scale 개요
* Before
![](https://d33wubrfki0l68.cloudfront.net/043eb67914e9474e30a303553d5a4c6c7301f378/0d8f6/docs/tutorials/kubernetes-basics/public/images/module_05_scaling1.svg)   
* After   
![](https://d33wubrfki0l68.cloudfront.net/30f75140a581110443397192d70a4cdb37df7bfc/b5f56/docs/tutorials/kubernetes-basics/public/images/module_05_scaling2.svg)   

# Commnad 를 이용하여 Scale-out
기존에 1개의 Pod 으로 실행중이던 APP 을 5개의 Pod 으로 Scale-out 하도록 하겠습니다.   
```bash
$ kubectl scale deployment --replicas=5 flask-example-app
```
   
* Scale 확인
    ```bash
    $ kubectl get deployment
    NAME                READY   UP-TO-DATE   AVAILABLE   AGE
    flask-example-app   5/5     5            5           126m
    ```
* Pod 확인
    ```bash
    $ kubectl get pod
    NAME                                READY   STATUS    RESTARTS   AGE
    flask-example-app-959c5f88d-k95wk   1/1     Running   0          127m
    flask-example-app-959c5f88d-rb4rv   1/1     Running   0          2m21s
    flask-example-app-959c5f88d-9kl5k   1/1     Running   0          2m21s
    flask-example-app-959c5f88d-b62j5   1/1     Running   0          2m21s
    flask-example-app-959c5f88d-fbjtl   1/1     Running   0          2m21s
    ```
* 서비스 확인(Round-Robin)
    ```bash
    $ while true; do curl 192.168.200.110:32296; done
    $ while true; do curl <TEST-SERVER-IP>:<NODEPORT>; done   << TEST 환경에 맞게 수정합니다.  
    ```
    아래와 같이 Scale-out 되어 서비스 중인 것을 볼 수 있습니다.   
    ```console
    $ while true; do curl 192.168.200.110:32296; done
     Container LAB | POD Working : flask-example-app-959c5f88d-9kl5k | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-9kl5k | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-rb4rv | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-9kl5k | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-9kl5k | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-b62j5 | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-rb4rv | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-k95wk | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-rb4rv | v=1
     Container LAB | POD Working : flask-example-app-959c5f88d-9kl5k | v=1
    ...
    ```

# `edit` 옵션을 이용하여 수정하는 방법 
```bash
$ kubectl edit deployment flask-example-app
```