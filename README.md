# Kubernetes Basic Hands on Lab
이 문서는 Kubernetes Basic Hands on Lab 교육을 위해 필요한 문서를 포함하고 있습니다.   
   
# 테스트 환경 관련
기본적으로 이 자료는 k3s 에서 테스트 및 Hands on 이 가능하도록 만들어진 자료입니다.   
([`IBM IKS`](https://cloud.ibm.com/catalog?category=containers) 및 기타 K8S 에서도 사용이 가능합니다.)   
   
# 공통 자료
+ [`IBM Cloud Kubernetes Service` 서비스 무료로 사용해보기 - External](https://chhanz.github.io/kubernetes/2020/01/27/create-cluster-ibm-kubernetes-service/)   
+ [`Ansible.fast-k3s` 를 사용하여 Hands on 환경을 만들어보자 - External](https://chhanz.github.io/kubernetes/2020/09/05/fast-k3s-use-ansible/)   
+ [`CKA` 자격 시험 정보](/doc/cka-information.md)   

# Hands on 목차
+ [Build APP - Container imgae](/doc/buildah.md)   
+ Deploy APP
    + [Build APP](/doc/build-app-flask.md)   
    + [Create The Deployment](/doc/create-the-deployment.md)   
+ Check APP   
    + [Check The Node](/doc/check-the-node.md)   
    + [Check The APP](/doc/check-the-pod.md)   
+ Expose APP   
    + [Expose APP - `NodePort`](/doc/expose-app.md)   
+ Scale APP   
    + [Scale APP](/doc/scale-app.md)   
+ Update APP   
    + [Update APP - `Rolling Update / Rollback`](/doc/update-app.md)   
   
### 참고 자료
* [https://kubernetes.io/docs/tutorials/kubernetes-basics/](https://kubernetes.io/docs/tutorials/kubernetes-basics/)    
* [https://chhanz.github.io/tags/kubernetes/](https://chhanz.github.io/tags/kubernetes/)   
* [https://github.com/chhanz/flask-example-app](https://github.com/chhanz/flask-example-app)   
* [https://github.com/chhanz/ansible.fast-k3s](https://github.com/chhanz/ansible.fast-k3s)    
