---
layout: postnew
title: Certified Kubernetes Administrator (CKA) 
#author: gini
categories: [ cloud ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: [cloud, automation, containers, kubernetes]
show-avatar: false
permalink: /cka-certified-kubernetes-administrator
featured: false
hidden: false
---

## 1.1. Disclaimer

This note is mainly based on an [online course](https://kodekloud.com/p/certified-kubernetes-administrator-with-practice-tests?affcode=142820_ttulld8g) for CKA exam - by [Mumshad Mannambeth](https://www.udemy.com/user/mumshad-mannambeth). I found this course very useful and highly recommended. This note is purely for exam preparation purpose (kind of quick reference or cheatsheet) and I do not have a plan to keep it as a permanent document here (maybe later I will add articles in my blog **[techbeatly.com](https://www.techbeatly.com))**.

## 1.2. References
- [Certified Kubernetes Administrator](https://www.cncf.io/certification/cka/)
- [Exam Curriculum (Topics)](https://github.com/cncf/curriculum)
- [Candidate Handbook](https://www.cncf.io/certification/candidate-handbook)
- [Exam Tips](http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD)

## 1.3. Table of Contents
<!-- TOC depthfrom:1 orderedlist:true -->

- [1. Certified Kubernetes Administrator (CKA)](#1-certified-kubernetes-administrator-cka)
  - [1.1. Disclaimer](#11-disclaimer)
  - [1.2. References](#12-references)
  - [1.3. Table of Contents](#13-table-of-contents)
- [2. Core Concepts](#2-core-concepts)
  - [2.1. What is ETCD](#21-what-is-etcd)
    - [2.1.1. etcd in k8s](#211-etcd-in-k8s)
  - [2.2. kube-api server](#22-kube-api-server)
  - [2.3. kube controller manager](#23-kube-controller-manager)
    - [2.3.1. Node Controller](#231-node-controller)
    - [2.3.2. Replication Controller](#232-replication-controller)
      - [2.3.2.1. Load balancing and Scaling](#2321-load-balancing-and-scaling)
  - [2.4. Kube Scheduler](#24-kube-scheduler)
  - [2.5. Kubelet](#25-kubelet)
  - [2.6. Kube Proxy](#26-kube-proxy)
  - [2.7. pods](#27-pods)
  - [2.8. Practice Test](#28-practice-test)
  - [2.9. Deployments](#29-deployments)
    - [2.9.1. Rolling Updates](#291-rolling-updates)
  - [2.10. Namespaces](#210-namespaces)
  - [2.11. Services](#211-services)
    - [2.11.1. Types of services](#2111-types-of-services)
      - [2.11.1.1. Node-pod service : Node port to pod port](#21111-node-pod-service--node-port-to-pod-port)
      - [2.11.1.2. Service Cluster IP](#21112-service-cluster-ip)
      - [2.11.1.3. Load Balancer :](#21113-load-balancer-)
- [3. Scheduling](#3-scheduling)
  - [3.1. Manual Scheduling](#31-manual-scheduling)
  - [3.2. Labels and Selectors](#32-labels-and-selectors)
  - [3.3. Taints and Tolerations](#33-taints-and-tolerations)
  - [3.4. Node Selectors](#34-node-selectors)
    - [3.4.1. Using Node Selectors](#341-using-node-selectors)
      - [3.4.1.1. Label a node](#3411-label-a-node)
      - [3.4.1.2. Add `nodeSelector` in pod definition](#3412-add-nodeselector-in-pod-definition)
    - [3.4.2. Node Affinity](#342-node-affinity)
    - [3.4.3. Node Affinity vs Taints and Tolerations](#343-node-affinity-vs-taints-and-tolerations)
  - [3.5. Resource Requirements and Limits](#35-resource-requirements-and-limits)
  - [3.6. Daemon Sets](#36-daemon-sets)
  - [3.7. Static Pods](#37-static-pods)
    - [3.7.1. Use Cases](#371-use-cases)
  - [3.8. Multiple Schedulers](#38-multiple-schedulers)
- [4. Logging and Monitoring](#4-logging-and-monitoring)
  - [4.1. Monitoring a cluster](#41-monitoring-a-cluster)
    - [4.1.1. Heapster and Metrics Server](#411-heapster-and-metrics-server)
    - [4.1.2. Application Logging](#412-application-logging)
- [5. Application Lifecycle Management](#5-application-lifecycle-management)
  - [5.1. Rolling Updates and Rollbacks](#51-rolling-updates-and-rollbacks)
  - [5.2. Deployment Strategies](#52-deployment-strategies)
    - [5.2.1. Recreate Strategy](#521-recreate-strategy)
    - [5.2.2. Rolling Update Strategy](#522-rolling-update-strategy)
    - [5.2.3. How to make changes ?](#523-how-to-make-changes-)
    - [5.2.4. Rollback](#524-rollback)
  - [5.3. Configure Applications](#53-configure-applications)
    - [5.3.1. Commands and Arguements](#531-commands-and-arguements)
    - [5.3.2. Configure Environment Variables](#532-configure-environment-variables)
      - [5.3.2.1. configMaps](#5321-configmaps)
      - [5.3.2.2. Secrets](#5322-secrets)
  - [5.4. Multi Container Pods](#54-multi-container-pods)
  - [5.5. Self Healing Applications](#55-self-healing-applications)
- [6. Cluster Maintenance](#6-cluster-maintenance)
  - [6.1. OS Upgrades](#61-os-upgrades)
  - [6.2. Kubernetes Releases](#62-kubernetes-releases)
  - [6.3. References](#63-references)
  - [6.4. Cluster Upgrade](#64-cluster-upgrade)
  - [6.5. Backup and Restore Methods](#65-backup-and-restore-methods)
    - [6.5.1. Backup entire etcd cluster](#651-backup-entire-etcd-cluster)
      - [6.5.1.1. Take snapshot using etcd](#6511-take-snapshot-using-etcd)
      - [6.5.1.2. Restore etcd snapshot](#6512-restore-etcd-snapshot)
    - [6.5.2. References](#652-references)
- [7. Security](#7-security)
  - [7.1. Security Primitives](#71-security-primitives)
    - [7.1.1. Authentication in kubernetes cluster](#711-authentication-in-kubernetes-cluster)
    - [7.1.2. Auth Mechanisams](#712-auth-mechanisams)
      - [7.1.2.1. Static password file](#7121-static-password-file)
      - [7.1.2.2. static token file](#7122-static-token-file)
      - [7.1.2.3. TLS Certificates](#7123-tls-certificates)
      - [7.1.2.4. identity services (LDAP, kerberos)](#7124-identity-services-ldap-kerberos)
    - [7.1.3. TLS in Kubernetes](#713-tls-in-kubernetes)
      - [7.1.3.1. Server Components](#7131-server-components)
      - [7.1.3.2. Client Components](#7132-client-components)
    - [7.1.4. TLS Certificate Creation](#714-tls-certificate-creation)
      - [7.1.4.1. Create CA (cert authority) Certificates](#7141-create-ca-cert-authority-certificates)
      - [7.1.4.2. Client Certificates](#7142-client-certificates)
      - [7.1.4.3. Node Certificates](#7143-node-certificates)
    - [7.1.5. View Certificate Details](#715-view-certificate-details)
    - [7.1.6. Certificates API](#716-certificates-api)
      - [7.1.6.1. Create Bootstrap Token on Master Node](#7161-create-bootstrap-token-on-master-node)
      - [7.1.6.2. Create Cluster Role Binding](#7162-create-cluster-role-binding)
      - [7.1.6.3. Authorize workers(kubelets) to approve CSR](#7163-authorize-workerskubelets-to-approve-csr)
      - [7.1.6.4. Auto rotate/renew certificates](#7164-auto-rotaterenew-certificates)
      - [7.1.6.5. Create bootstrap context on worker node](#7165-create-bootstrap-context-on-worker-node)
      - [7.1.6.6. Create Kubelet Service on worker nodes](#7166-create-kubelet-service-on-worker-nodes)
  - [7.2. KubeConfig](#72-kubeconfig)
  - [7.3. API Groups](#73-api-groups)
  - [7.4. kubectl proxy](#74-kubectl-proxy)
  - [7.5. Role Based Access Controls in kuberenetes](#75-role-based-access-controls-in-kuberenetes)
    - [7.5.1. Sample Role Definition](#751-sample-role-definition)
    - [7.5.2. Sample RoleBinding Definition](#752-sample-rolebinding-definition)
  - [7.6. Cluster Roles](#76-cluster-roles)
    - [7.6.1. ClusterRole Defintion](#761-clusterrole-defintion)
    - [7.6.2. ClusterRoleBinding Defintion](#762-clusterrolebinding-defintion)
  - [7.7. Image Security](#77-image-security)
  - [7.8. Security Contexts](#78-security-contexts)
    - [7.8.1. Adding Capabilities](#781-adding-capabilities)
  - [7.9. Network Policies](#79-network-policies)
- [8. Storage in kubernetes](#8-storage-in-kubernetes)
  - [8.1. Volumes](#81-volumes)
  - [8.2. Persistent Volumes (PV)](#82-persistent-volumes-pv)
  - [8.3. Persistent Volumes Claimes (PVC)](#83-persistent-volumes-claimes-pvc)
  - [8.4. Use PV & PVC in Pods](#84-use-pv--pvc-in-pods)
- [9. Networking in Kubernetes](#9-networking-in-kubernetes)
  - [9.1. Docker Networking Types](#91-docker-networking-types)
    - [9.1.1. none - no connectivity](#911-none---no-connectivity)
    - [9.1.2. Host network](#912-host-network)
    - [9.1.3. Bridge](#913-bridge)
    - [9.1.4. Container Networking Interface (CNI)](#914-container-networking-interface-cni)
  - [9.2. Cluster Networking](#92-cluster-networking)
  - [9.3. Pod Networking](#93-pod-networking)
  - [9.4. CNI in Kubernetes](#94-cni-in-kubernetes)
  - [9.5. Weaveworks CNI Plugin](#95-weaveworks-cni-plugin)
    - [9.5.1. Deploy weave](#951-deploy-weave)
  - [9.6. Weavenet IP Address Management (IPAM)](#96-weavenet-ip-address-management-ipam)
  - [9.7. Service Networking](#97-service-networking)
  - [9.8. DNS in kubernetes](#98-dns-in-kubernetes)
  - [9.9. CoreDNS in kubernetes](#99-coredns-in-kubernetes)
  - [9.10. Ingress](#910-ingress)
    - [9.10.1. Ingress Controller](#9101-ingress-controller)
- [10. Installing Kubernetes](#10-installing-kubernetes)
  - [10.1. minikube](#101-minikube)
  - [10.2. kubeadm](#102-kubeadm)
  - [10.3. Production Clusters](#103-production-clusters)
    - [10.3.1. Turnkey Solutions](#1031-turnkey-solutions)
    - [10.3.2. Hosted Solutions](#1032-hosted-solutions)
  - [10.4. HA in Kubernetes](#104-ha-in-kubernetes)
  - [10.5. Get etcdctl utility if it's not already present.](#105-get-etcdctl-utility-if-its-not-already-present)
  - [10.6. Installing kubernetes](#106-installing-kubernetes)
  - [10.7. Configuring kubectl for remote access](#107-configuring-kubectl-for-remote-access)
  - [10.8. Deploy POD networking solution](#108-deploy-pod-networking-solution)
  - [10.9. kubernetes cluster test suite - kubetest](#109-kubernetes-cluster-test-suite---kubetest)
  - [10.10. Deploy a Kubernetes Cluster with KubeAdm](#1010-deploy-a-kubernetes-cluster-with-kubeadm)
- [11. Troubleshooting kubernetes](#11-troubleshooting-kubernetes)
- [12. kubectl advanced usage](#12-kubectl-advanced-usage)
    - [12.1. JSON PATH in kubectl](#121-json-path-in-kubectl)
- [13. Appendix](#13-appendix)
    - [13.1. Editing a pod](#131-editing-a-pod)

<!-- /TOC -->


# 2. Core Concepts

## 2.1. What is ETCD
Distributed reliable key-value store that is simple, secure and fast.

*Learn how to install etcd : https://computingforgeeks.com/how-to-install-etcd-on-rhel-centos-8/*
- etcd will start a service and listen on port 2379
- etcdctl 
    ./etcdctl set key1 value1

### 2.1.1. etcd in k8s
- if you install k8s manually, etcd has to install manually
- if k8s installation is by using kubeadm, then kubeadm will install and configure etcd

- etcd will be running under kube-system namespace
```
kubectl get pods -n kube-system 
  # you will see a pod running with name `etcd-master`
kubecrl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only
```
- in a HA system, there will be multiple master nodes and in the cluster and each master node will have an etcd pod running for HA.

## 2.2. kube-api server
- kubectl command is hitting on kube-api server 
- kube-api server will authenticate and validate the request and send back the result.
- It also assist to retrive information, update etcd etc

Config file ```/etc/kubernetes/manifests/kube-apiserver.yaml```

## 2.3. kube controller manager
- continuous monitoring of cluster and components
- remediation

There are other controllers under kube controller manager like deployment controller, job controller, name-space controller etc.

Check controller manager status by ```kubectl get pods -n kube-system```.

### 2.3.1. Node Controller
**Node monitoring**
- will monitor status of every nodes in the cluster every 5 sec; if not reachable then it will wait for 40 sec (grace period) before marking as unreachable
- if unreachable for 5 min, it will evict the pods from that node and deploy to other available nodes

### 2.3.2. Replication Controller
- Monitor the status of replicasets and make sure desired number of replicas are running on nodes.

- Replication controller and Replica Sets -> for same purpose but not same
    - Replication controller is the old technology
    - Replica set is the new recommended method

**Sample rc-def.yml**
```
apiVerion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp
      label:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-controller
        image: nginx
  replicas: 3
```

**Sample replicationset-def.yml**

*See the difference and options from replicationcontrollder like ```selector``` etc.*

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp
      label:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-controller
        image: nginx
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```

**Sample pod-defenition.yml**
```
apiVerion: v1
kind: pod
```

#### 2.3.2.1. Load balancing and Scaling
You can scale application by below methods.
- changing the ```replicas``` option in replicaset defenition. And then run ```kubectl replace -f replicaset-defenition.yml```
- run ```kubectl scale --replicas=6 -f replicaset-defenition.yml```
- run ```kubectl scale --replicas=6 replicaset myapp-replicaset``` (Note: this one will not update replica details in replicaset defenition file.)

 
## 2.4. Kube Scheduler
- kube scheduler doesnt place pods on nodes, instead it will decide where to place the pods.
- placing pods to nodes is the job of kubelet

**How to see kube-scheduler options**
- ```/etc/kubernetes/manifests/kube-scheduler.yaml```
- ```systemctl status kube-scheduler``` - pod defenition file
- ```ps -aux|grep kube-scheduler```

## 2.5. Kubelet
- like a captain of ship
- enquire status and report back to master node 
- **kubeadm install will not install kubelet by default**; you must always need to manually install it.

## 2.6. Kube Proxy
- pod network
- pods are accessible by service IP instead of pod IP
- kube-proxy is running on all nodes
- everytime a new service is created, kube-proxy will coordinate to create the same rule (iptables) on other podes so that pods behind the service will be accessible on all nodes.
- kubeadm will deploy kube-proxy under kube-system namespace
- kube-proxy is deployed a daemonset and will be deployed on all nodes in the cluster - ```kubectl get daemonset -n kube-system```

## 2.7. pods
- k8s doesn't deploy containers directly to nodes, instead it will encapsulated in an object called pod. 
- smallest object in kubernetes


## 2.8. Practice Test
```
# create an nginx pod
kubectl run nginx --image=nginx --generator=run-pod/v1
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.

# check pods in current project
kubectl get pods

# delete a pod
kubectl delete pod webapp

# Create a new pod with the name 'redis' and with the image 'redis123'
kubectl run redis --image=redis123 --generator=run-pod/v1
```
 
## 2.9. Deployments

### 2.9.1. Rolling Updates
Update one by one

**Defenition**
```
controllers/nginx-deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
    type: back-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

- Replicaset and deployments are almost same but deployment will create an object called "deployment"

## 2.10. Namespaces
Below namespace will be created automatically by kuberenetes
- default
- kube-system
- kube-public

```
kubectl create namespace myproj
```

**Calling a service/pod**
- use dfeault name if in same namespace
  ```
  mysql.connect("db-service")
  ```
- use name.namespace if another namespace
  ```
  mysql.connect("db-service.prod.svc.cluster.local")
  ```
- A DNS entry will be automatically added when a service is created. 

**Exam Tips**
- `kubectl run` will help to generate yaml template. 
https://kubernetes.io/docs/reference/kubectl/conventions/

Create an NGINX Pod
```
kubectl run --generator=run-pod/v1 nginx --image=nginx
```

Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml
```

Create a deployment
```
kubectl run --generator=deployment/v1beta1 nginx --image=nginx
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
```
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run -o yaml
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)
```
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml
```

Save it to a file - (If you need to modify or add some other details before actually creating it)
```
kubectl run --generator=deployment/v1beta1 nginx --image=nginx --dry-run --replicas=4 -o yaml > nginx-deployment.yaml
```

## 2.11. Services
- Enable communication between pods and outside. (eg: frontend, backend etc)

### 2.11.1. Types of services

#### 2.11.1.1. Node-pod service : Node port to pod port

  Valid Range : 30000-32767
  
```
apiVersion: v1
kind: Service
metadata: 
  name: my-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

#### 2.11.1.2. Service Cluster IP

```
apiVersion: v1
kind: Service
metadata: 
  name: back-end
spec:
  type: ClusterIP
  ports:
    - targetPort: 80
      port: 80
  selector:
    app: myapp
    type: back-end
```

#### 2.11.1.3. Load Balancer :

# 3. Scheduling

## 3.1. Manual Scheduling
- scheduler will check for `nodeName` input in the defention to decide the node to host the pod.
- if no scheduler in place, pod will not get deployed as we will see `pending` status.
- only during creation you can assign a `nodeName`; once created kubernetes wont allow to change it.
- You can still change node but using a binding object (create a Binding object and call a POST request)

## 3.2. Labels and Selectors
- under metadata --> labels
- used to tag objects in kubernetes
- use `--selector` to show specific labelled items.

## 3.3. Taints and Tolerations
- decision on what pods can be schedule on nodes
  eg: add taint to node so only tolerant pods can be placed on that node
- taints on nodes
- tolerance on pods
- master nodes are already tainted with NoShedule and will not deploy and pod by default

Apply taint on node
```
kubectl taint nodes node1 app=blue:NoSchedule
```

untaint a node by using `"-"` at the end.
```
kubectl taint nodes node1 app=blue:NoSchedule- 
```

Sample pod definition
```
.
.
spec: 
  containers:
  - name: nginx-container
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

## 3.4. Node Selectors

Two methods to reserve a node for specific pods. (or to control pod placement on a specific node)

### 3.4.1. Using Node Selectors

#### 3.4.1.1. Label a node 

```
kubectl label nodes node1 size=Large
```

#### 3.4.1.2. Add `nodeSelector` in pod definition

```
.
.
  containers:
  - name: nginx-container
    image: nginx
    
  nodeSelector:
    size: Large
```

### 3.4.2. Node Affinity

```
.
.
spec: 
  containers:
  - name: nginx-container
    image: nginx
  
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: Size
            operator: In
            values:
            - Large
```

### 3.4.3. Node Affinity vs Taints and Tolerations
- Use both methods to ensure pods are placed on dedicated nodes (if required)

## 3.5. Resource Requirements and Limits
- by default kubernetes assumes a pod requires .5 CPU and 256Mi of memory
- by default kubernetes assumes the limit as 1 vCPU and 512Mi for pods/containers
- 1 count of CPU = 1 vCPU (1 AWS/GCP/Azure/Hyperthread)

```
.
.
spec: 
  containers:
  - name: nginx-container
    image: nginx
  
  resources:
    requests:
      memory: "1Gi"
      cpu: 1
    limits:
      memory: "2Gi"
      cpu: 2
```
- A pod cannot use more CPU resources than the limits
- If a pod try to use more memory than limit, pod will be terminated.

## 3.6. Daemon Sets
- daemonsets are like replicasets but it runs one copy of pod in each node in your cluster. Whenever a new node is added to cluster, automatically a pod will be deployed on that node too.
- Use cases : Monitoring agent, logging agent etc
- Kube-proxy can be deployed on all nodes as daemonset

DaemonSet definition is almost same like ReplicaSet.

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  template:
    metadata:
      labels:
        app: montoring-agent
    spec:
      containers:
      - name: montoring-agent
        image: montoring-agent
  selector:
    matchLabels:
      app: montoring-agent
```
- Ensure `nodeName` property is set on all nodes (until v1.12)
- from v1.12 onwards, daemonset uses default scheduler and node affinity

Sample fluentd pod
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: elasticsearch
spec:
  template:
    metadata:
      labels:
        app: elasticsearch
    spec:
      containers:
      - name: elasticsearch
        image: k8s.gcr.io/fluentd-elasticsearch:1.20
  selector:
    matchLabels:
      app: elasticsearch
```      

## 3.7. Static Pods
- kubelet can manage a pod independently 
- but no API to provide pod details
- so we need to put pod defenition inside a file in a dedicated place (/etc/kubernetes/manifests)
- kublet will scan this location and create pods automatically
- if the application crashes or you made changes on denition, kubelet will try to re-create pod.
- if you remove a file from this directory, kubelet will delete the pod as well
- you can create only pods like this but no replicasets or daemonsets
- the directory can be anywhere and can mention while running the service as `--pod-manifest-path=/etc/kubernetes/manifests` (or using `--config=kubeconfig.yaml` and path inside)

### 3.7.1. Use Cases
- kube-apiserver, kube-scheduler, kuber-controller-manager, etcd etc

`kubectl get pods --all-namespaces` and look for `-master` or `-node01` at the end of pod name.
Run the command `ps -aux | grep kubelet` and identify the config file `--config=/var/lib/kubelet/config.yaml`. Then check in the config file for staticPdPath.


## 3.8. Multiple Schedulers
You can configure custom schedulers by downloading the binary.
- configure pod to use which scheduler to pickup scheduling
- `kubectl get events` to see which scheduler created the pod

```
apiVersion: v1
kind: Pod
metadata:
  name: annotation-default-scheduler
  labels:
    name: multischeduler-example
spec:
  schedulerName: default-scheduler
  containers:
  - name: pod-with-default-annotation-container
    image: k8s.gcr.io/pause:2.0  
```

# 4. Logging and Monitoring

## 4.1. Monitoring a cluster

- node level metrics - number of nodes, CPU, Memory
- pod level metrics - number of pods, performance, CPU, Memory consuption

Kubernetes doesn't have a fully featured system yet. 
We have tools like Metrics Server, Prometheus, Elastic Stack, Datadog, Dynatrace etc.

### 4.1.1. Heapster and Metrics Server
- Heapster already deprecated
- Metrics Server collect data from node and pods
- in memory solution and doesnt store anywhere.
- cAdvisor (container advisor) which is part of `kubelet` will be responsible for collecting metrics from pods.

**Deployment in cluster**
```
git clone https://github.com/kubernetes-incubator/metrics-server.git
cd metric-server
kubectl create -f deploy/1.8+/
```

To see node or pod usage
```
kubectl top node
kubectl top pod
```

**Enable in minikube**

```
minikube addons enable metrics-server
```

### 4.1.2. Application Logging

```
# mention container name if you have multiple container inside a pod
kubectl logs -f event-simulator-pod event-simulator 
```

# 5. Application Lifecycle Management

## 5.1. Rolling Updates and Rollbacks

```
kubectl rollout status deployment/myapp-dc
kubectl rollout history deployment/myapp-dc
```

## 5.2. Deployment Strategies

### 5.2.1. Recreate Strategy
- delete all pods and recreate new pods
- all pod will be scaled down to 0
- there will be downtime as all pods will be deleted and recreated
- not the default one

### 5.2.2. Rolling Update Strategy
- delete a pod and create new one
- will be doing it one by one rather than all together
- less or no downtime as doing pods one by one
- default deployment strategy
 

### 5.2.3. How to make changes ?
- edit the deployment - `kubectl edit deployment DEPLOYMENT`
or
- edit the deploynent definition file and `kubectl apply -f DEFINITION.YML`
 
then a new rollout will be triggered and a new revision of deployment will be created.

Also you can use `kubectl set image deployment/myapp-dc`

### 5.2.4. Rollback

```
kubectl rollout undo deployment/myapp-dc
```

check status of replicasets before and after rollback and see difference. (`kubectl get replicasets`)

## 5.3. Configure Applications

### 5.3.1. Commands and Arguements

```
                      DOCKER                  k8s
                      =================================================
Command to execute    ENTRYPOINT              command 
Default Argument      CMD                     args

eg:                   ENTRYPONT ["sleep"]     command:["sleep2.0"]
                      CMD ["5"]               args:["10"]
```

### 5.3.2. Configure Environment Variables

#### 5.3.2.1. configMaps

```
# plain key-value pair
  env:
    - name: COLOR
      value: blue

# configMap
  envFrom:
    - configMapKeyRef:

# secrets
  envFrom:
    - secretKeyRef:
```

```
kubectl create configmap \
  CONFIGMAP_NAME --from-literal=KEY=VALUE

kubectl create configmap \
  CONFIGMAP_NAME --from-file=app.properties
```  

Sample configMap definition
```
apiVersion: v1
kind: ConfigMap
metadata: 
  name: app-config
data:
  allowed: '"true"'
  enemies: aliens
  lives: "3"
```
then,
```
  envFrom:
    - configMapRef:
        name: app-config
```

#### 5.3.2.2. Secrets
- used to store sensitive data
- must mention in hashed format; use any encoding tools like `base64` to encode secret.

```
echo -n "root" | base64 
cmm9vdA==
```

create secret using command

```
kubectl create secret generic \
  SECRET_NAME --from-literal=KEY=VALUE
```

or via a definition file

```
apiVersion: v1
kind: Secret
metadata: 
  name: app-secret
data:
  DB_HOST: mysql
  DB_USER: root
  DB_PASS: password
```

**Get secret details**

```
kubectl describe secrets     # will hide value
kubectl get secrets app-secret -o yaml
                            # will display hashed value
```

To decode:

```
echo -n "cmm9vdA==" | base64 --decode
root
```

**Inject secrets into pods**

```
# All ENV from secrets
- image:
  .
  .
  .
  envFrom:
  - secretRef: 
      name: app-secret

# Single ENV
  env:
  - name: DB_PASSWORD
    valueFrom:
      secretKeyRef:
        name: app-secret
        key: DB_PASS

# Volumes - each attribute in the secret will be created 
# as a file in that volume path
  volumes:
  - name: app-secret-volume
    secret:
      secretName: app-secret

```

## 5.4. Multi Container Pods

```
spec:
  containers:
  - name: web-app
    image: web-app-image
    ports:
      - containerPort: 8080
  - name: log-agent
    image: log-agent-image
```

Read more about initContainers [here](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/).


## 5.5. Self Healing Applications

Kubernetes supports self-healing applications through ReplicaSets and Replication Controllers.

# 6. Cluster Maintenance

## 6.1. OS Upgrades
- kubernetes will wait for 5 minute if a node is offline; otherwise will consider as dead
- if the pods are part of replicaset, it will be recreated on other nodes
- the time to come back a pod is called pod eviction time- default 5m0s

```
kubectl drain node-1
kubectl cordon node-2
kubectl uncordon node-1 
```

## 6.2. Kubernetes Releases

```
v1.11.3
    | | 
    | |___  Patch - Bug Fixes
    |___ _  Minor - Features, Functionalities
```

## 6.3. References
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api_changes.md

## 6.4. Cluster Upgrade 

- kubernetes support recent version and -2 versions only

```
kubeadm upgrade plan
kubeadm upgrade apply
```

upgrading kubelet on master

```
apt-get upgrade -y kubeadm=1.12.0-00
  # or
apt-get install kubeadm=1.12.0-00

kubeadm upgrade apply v1.12.0
apt-get upgrade -y kubelet=1.12.0-00
systemctl restart kubelet
```

do on nodes (remember to login to node)

```
apt-get upgrade -y kubeadm=1.12.0-00
apt-get upgrade -y kubelet=1.12.0-00
kubeadm upgrade node config --kubelet-version v1.12.0
systemctl restart kubelet


apt install kubeadm=1.12.0-00
apt install kubelet=1.12.0-00
kubeadm upgrade node config --kubelet-version $(kubelet --version | cut -d ' ' -f 2)
```

Note: While upgrading kubelet, if you hit dependency issue while running the apt-get upgrade kubelet command, use the apt install kubelet=1.12.0-00 command instead

## 6.5. Backup and Restore Methods

- Save in SCM (git)
- save all objects 

```
kubectl get all --all-namespaces -o yaml > all-config.yaml
```

- Use tools like [velero](https://velero.io/) (by Heptio, which is part of vmware now)

### 6.5.1. Backup entire etcd cluster
- data will be stored in `--data-dir=/var/lib/etcd`; take backup of the same

#### 6.5.1.1. Take snapshot using etcd

- Take a snapshot

```
ETCDCTL_API=3 etcdctl \
  --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /tmp/snapshot-pre-boot.db
```  

- See the snapshot details. 

```
ETCDCTL_API=3 etcdctl snapshot status snapshot.db
```

#### 6.5.1.2. Restore etcd snapshot

- stop kube-api-server

```
service kube-apiserver stop
```

- restore snapshot

```
ETCDCTL_API=3 etcdctl \
  --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/ kubernetes/pki/etcd/ca.crt \
  --name=master \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/erver.key \
  --data-dir /var/lib/etcd-from-backup \
  --initial-cluster=master=https://127.0.0.1:2380 \
  --initial-cluster-token etcd-cluster-1 \
  --initial-advertise-peer-urls=https://127.0.0.1:2380 \
  snapshot restore /tmp/snapshot-pre-boot.db
2019-11-08 09:05:46.195550 I | mvcc: restore compact to 3263
2019-11-08 09:05:46.203028 I | etcdserver/membership: added member e92d66acd89ecf29 [https:/127.0.0.1:2380] to cluster 7581d6eb2d25405b
```

- Configure etcd to start with new data directory and cluster token by editing `/etckubernetes/manifests/etcd.yaml`, then etcd pod will be re-deployed.
  - Update --data-dir to use new target location

    `--data-dir=/var/lib/etcd-from-backup`

  - Update new initial-cluster-token to specify new cluster

    `--initial-cluster-token=etcd-cluster-1`

  - Update volumes and volume mounts to point to new path
  
    `/var/lib/etcd-from-backup`
  
- Restart etcd
- start kube-apiserver

### 6.5.2. References
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- https://github.com/etcd-io/etcd/blob/master/Documentation/op-guide/recovery.md
- https://www.youtube.com/watch?v=qRPNuT080Hk


# 7. Security

## 7.1. Security Primitives
- Authntication and Authorization
- All using TLS Encryption
- Network policies

### 7.1.1. Authentication in kubernetes cluster
- Different users - admins, developers, bots (other services)
- kubernetes cannot user accounts but can create service accounts
- All user access managed by kube-apiserver, which will authenticate the request before processing it.

### 7.1.2. Auth Mechanisams

- Static password/token files
- Certificates
- identity services (LDAP, kerberos)

#### 7.1.2.1. Static password file

```
$ cat user-details.csv
# User File Contents
password123,user1,u0001
password123,user2,u0002
password123,user3,u0003
password123,user4,u0004
password123,user5,u0005
```
- mention `--basic-auth-file=user-details.csv` in `ExecStart` for `kube-apiserver` 
- mention in `/etc/kubernetes/manifests/kube-apiserver.yaml` manifest)
- and modify the kube-apiserver startup options to include the basic-auth file

`curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`

#### 7.1.2.2. static token file
Same like static file but toekn inside the static file - `--token-auth-file=user-details.csv`

`curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: user1 KsdHGsyhSjysh36sShdhS6fhsgdjS7dsh"`

#### 7.1.2.3. TLS Certificates
- PKI - Public Key Infrastructure

- Root Certificates -  Certificate Authorities (Symantec, DigiCert etc)
- Server Certificates - on servers
- Client Certificates - on clients

- Public Keys or Certificate - `*.crt`, `*.pem` (server.crt, client.pem etc)
- Private Key - `*.key`, `*key.pem` (server.key, client-key.pem etc)

#### 7.1.2.4. identity services (LDAP, kerberos)

### 7.1.3. TLS in Kubernetes

#### 7.1.3.1. Server Components
**kube-apiserver**
- `apiserver.crt` and `apiserver.key`

**etc server**
- `etcdserver.crt` and `etcdserver.key`

**kubelet server**
- `kubelet.crt` and `kubelet.key`

#### 7.1.3.2. Client Components
- admin/users - `admin.crt` and `admin.key`

- scheduler is a client which contact `kube-apiserver` to do things - `scheduler.crt` and `scheduler.key`

- `kube-controller-manager` is another client which contacts `kube-apiserver` - `controller-manager.crt` and `controller-manager.key`

- `kube-proxy` need certificate to authenticate with `kube-apiserver` - `kube-proxy.crt` and `kube-proxy.key`

### 7.1.4. TLS Certificate Creation
- EASYRSA, OPENSSL, CFSSL

#### 7.1.4.1. Create CA (cert authority) Certificates

```
# create a key
openssl genrsa -out ca.key 2048

# CSR
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr 

# Sign it
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
```

#### 7.1.4.2. Client Certificates

#### 7.1.4.3. Node Certificates

### 7.1.5. View Certificate Details

Check certificates configured
```
cat /etc/kubernetes/manifests/kube-apiserver.yaml
```
Check cetificate details
```
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text --noout
```
If there is an issue,
- check pod definition and certificate details
- `docker ps -a` and `docker logs DOCKER_ID` and check logs
- if expired, generate a new certificate

```
openssl x509 -req \
  -in /etc/kubernetes/pki/apiserver-etcd-client.csr \
  -CA /etc/kubernetes/pki/etcd/ca.crt \
  -CAkey /etc/kubernetes/pki/etcd/ca.key \
  -CAcreateserial \
  -out /etc/kubernetes/pki/apiserver-etcd-client.crt
```

### 7.1.6. Certificates API
- CA certificates are saved sacurly in a place caled CA Server.
- Usually this will be the master node.
- Automated this via Certificate API
- worker node proper permission to do this automated certificate handling. For this, we need to create `bootstraptoken` which is associated to `system:bootstrappers`. Then configure the kubelet to use this to authenticate to certificate api-server. 
- Then need to assign a role (default role - `system:node-bootstrapper` to `system:bootstrappers`) so that kubelet can do auth.
```
kubectl create clusterrolebinding crb-bootstrappers --clusterrole=system:node-bootstrapper --group=system:bootstrappers
```

- To automate the signing request approval, assign role `certificatesigningrequest:nodeclient` to `system:bootstrappers`
```
kubectl create clusterrolebinding crb-node-autoapprove-csr --clusterrole=system:certificates.k8s.io:certificatesigningrequests:nodeclient --group=system:bootstrappers
```

- now you can use `--bootstrap-kubeconfig="/var/lib/kubeconfig/bootstrap-kubeconfig"`(which contains the token) in `kubelet` argument (instead of `--kubeconfig="/var/lib/kubelet/kubeconfig"`)

on worker node

```
$ cat /var/lib/kubelet/bootstrap-kubeconfig
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://172.17.0.11:6443
  name: bootstrap
contexts:
- context:
    cluster: bootstrap
    user: kubelet-bootstrap
  name: bootstrap
current-context: bootstrap
kind: Config
preferences: {}
users:
- name: kubelet-bootstrap
  user:
    token: 05832d.x262bbbe835dx21k
```

Sample `kubelet` service definition file (on worker node)

```
$ cat /etc/systemd/system/kubelet.service
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/bin/kubelet \
  --bootstrap-kubeconfig=/var/lib/kubelet/bootstrap-kubeconfig \
  --kubeconfig=/var/lib/kubelet/kubeconfig \
  --register-node=true \
  --v=2
Restart=on-failure
StandardOutput=file:/var/kubeletlog1.log
StandardError=file:/var/kubeletlog2.log
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Then reload and start kubelet service
```
systemctl daemon-reload && service kubelet start
```

#### 7.1.6.1. Create Bootstrap Token on Master Node

```
$ cat /var/answers/bootstrap-token-05832d.yaml
apiVersion: v1
kind: Secret
metadata:
  # Name MUST be of form "bootstrap-token-<token id>"
  name: bootstrap-token-09426c
  namespace: kube-system

# Type MUST be 'bootstrap.kubernetes.io/token'
type: bootstrap.kubernetes.io/token
stringData:
  description: "The default bootstrap token generated by 'kubeadm init'."

  # Token ID and secret. Required.
  token-id: 05832d
  token-secret: x262bbbe835dx21k

  # Expiration. Optional.
  expiration: 2020-03-10T03:22:11Z

  # Allowed usages.
  usage-bootstrap-authentication: "true"
  usage-bootstrap-signing: "true"

  # Extra groups to authenticate the token as. Must start with "system:bootstrappers:"
  auth-extra-groups: system:bootstrappers:node03
```

#### 7.1.6.2. Create Cluster Role Binding
```
kubectl create clusterrolebinding crb-to-create-csr --clusterrole=system:node-bootstrapper --group=system:bootstrappers
```

#### 7.1.6.3. Authorize workers(kubelets) to approve CSR
```
kubectl create clusterrolebinding crb-to-approve-csr --clusterrole=system:certificates.k8s.io:certificatesigningrequests:nodeclient --group=system:bootstrappers
```

#### 7.1.6.4. Auto rotate/renew certificates
```
kubectl create clusterrolebinding crb-autorenew-csr-for-nodes --clusterrole=system:certificates.k8s.io:certificatesigningrequests:selfnodeclient --group=system:nodes
```

#### 7.1.6.5. Create bootstrap context on worker node
`kubectl cluster-info` will display api-server info.

on workder node :
```
kubectl config --kubeconfig=/tmp/bootstrap-kubeconfig set-cluster bootstrap --server='https://172.17.0.11:6443' --certificate-authority=/etc/kubernetes/pki/ca.crt
kubectl config --kubeconfig=/tmp/bootstrap-kubeconfig set-credentials kubelet-bootstrap --token=09426c.g262dkeidk3dx21x
kubectl config --kubeconfig=/tmp/bootstrap-kubeconfig set-context bootstrap --user=kubelet-bootstrap --cluster=bootstrap
kubectl config --kubeconfig=/tmp/bootstrap-kubeconfig use-context bootstrap
```

#### 7.1.6.6. Create Kubelet Service on worker nodes
```
cat > /etc/systemd/system/kubelet.service <<-EOF
[Unit]
Description=Kubernetes Kubelet
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/bin/kubelet \
  --bootstrap-kubeconfig=/tmp/bootstrap-kubeconfig \
  --kubeconfig=/var/lib/kubelet/kubeconfig \
  --register-node=true \
  --cgroup-driver=cgroupfs \
  --v=2
Restart=on-failure
StandardOutput=file:/var/kubeletlog1.log
StandardError=file:/var/kubeletlog2.log
RestartSec=5

[Install]
WantedBy=multi-user.target

EOF
```

- Restart daemon
```
node03$ systemctl daemon-reload
node03$ service kubelet start
```

- to enable `kubelet` to automatically roate or renew certificate, pass `--rotate-certificates=true` argument.
```
kubectl create clusterrolebinding crb-node-autorotate-csr --clusterrole=system:certificates.k8s.io:certificatesigningrequests:selfnodeclient --group=system:nodes
```

- client certificates will be automatically approved by config but server certificates need to be approved manually (due to security reasons)
```
kubectl get csr
kubectl certificate approve <CSR_NAME>
```

- All the Certificate creation operations are carried out by `controller-manager`

  1. User generates a key and submit to Admin

  2. Administrator takes the key and generate CSR using as a kubernetes object defenition (`kind: CertificateSigningRequest`)
  
```
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: johnp
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQXJSbXBGcjBwWWlaNVV5c2FkUk5PaTMwMVF0T2hvK09vZG1mMTZnWEhPWTVYCmtjMjROTG1sSHNDTEFCcHZMMmZ6bllDMXRmWHFpSTQwc3FVU2hYNzlkanBac0FoUXVrdlpucjh0UmFwRVNqSzMKUXNtQmpCcmx6OXhMNEhXenJQSjlGMFdFNHd0amNTUHFEZkdkMkl0MUdyZWhmN1RhTVozbGU5Zm1ZNVR5YXRHSgpVc3VQMXJpUWpIUGJWYjV4WEQ3UkhQbVRNVmNUU1gyeFdlT3RLbDUwSmcwNlBjK3NVUzF2dEpvMDRybjA2TjM1ClREbjhUNmZ0RlFCV0k0bTU4TVpHcXM5VWcwcHpkd0taRnRlZGNVUE1JS0NrZW5HV3NWYkdxQzlvdWdOekY0OFcKbGF0ZExsSXZsajlVNEpWWkFDOGNEaGcxR3pBeEkzOFdrekcyTEJRL213SURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBQ0QyWGFwdC8wQ1hldjQ4QlFNTm9XY2FOZ2dRMHNmbWU5UlpnUzlweWtDMHpiSmNjeW5KCjZtUFJWMnp6Q1Mvd2V6Sjh1VzV0bm83Y1MxYUdzbVV0VDFWbGowb2ExbVNJUlBFZGt3aDBDMFByaytsV2Q4NW4KMFV5YW9GK0JSNzNSWExsRXdhT2ZlSG9SRWkzc2FYaGR1TmdtT1lyRmRsUk8rdWc4bnA5cjR5U3U3cXJINUFoRAo5YVR2a2dpakJqdzRnazNwWUxFbG5rTnlYZVFWWUFjdTZJNkRIUy9saDFmQzdDcGhzclFIN0RvaENIY3NTS2RSCnZsMEhLdDFzUk85UTd5dnNLbVFQdnRxcnJnTGdZd3NJWVgxSzBiWGVpS2wwd2dzRnpmZjJieG1qQmNwTDlUSmMKS3dZcHRVTWp3MDQ0TW9NUnZxL1J4T0FvZlNvT1pvd3dEdU09Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  usages:
  - digital signature
  - key encipherment
  - server auth
```    

  3. `request` part will be encoded using `base64` and add to definition

  4. Once object created, all administrators can see the CSR (`kubectl get csr`)

  5. Approve the request - `kubectl certificate approve user1` 
  (or reject via `kubectl certificate deny user2`)
  
  6. Once approved, administrator can see the certificate and extract for the user - `kubectl get csr user1 -o yaml` (remember to decode the content using `|base64 --decode`)


## 7.2. KubeConfig
- instead of passing each certificates via commandline, we can mention all cert details in a config file and pass this to kubectl command.

before :
```
kubectl get pods \
  --server kubesandbox:6443 \
  --client-key admin.key \
  --client-certificate admin.crt \
  --certificate-authority ca.cert
```

After :

Put these info inside a file `config` and call `--kubeconfig` in command
```
kubectl get pods \
  --kubeconfig config
```
- kuberctl will search for a file `~/.kube/config`; so we dont need to specify the file path.

```
current-context: gcp-user@gcp-kube-cluster



kubectl config view           # to view the config in ~/.kube/config
kubectl config view --kubeconfig=path-to-config-file
                              # to view the config
kubectl config use-context dev@singapore-cluster                              
                              # to change the current-context 
kubectl config -h             # to list avaialbe options                              
```

- You can also add `namespace: YOUR_NAMESPACE` under context
- Certificate information can be mentioned as,

  a) `certificate-authority: PATH-TO-CA-CERT`

  b) `certificate-authority-data: base64_ENCODED_DATA`

Sample kubeconfig file
```
apiVersion: v1
kind: Config

clusters:
- name: production
  cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://172.17.0.25:6443

- name: development
  cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://172.17.0.25:6443

- name: kubernetes-on-aws
  cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://172.17.0.25:6443

- name: test-cluster-1
  cluster:
    certificate-authority: /etc/kubernetes/pki/ca.crt
    server: https://172.17.0.25:6443

contexts:
- name: test-user@development
  context:
    cluster: development
    user: test-user

- name: aws-user@kubernetes-on-aws
  context:
    cluster: kubernetes-on-aws
    user: aws-user

- name: test-user@production
  context:
    cluster: production
    user: test-user

- name: research
  context:
    cluster: test-cluster-1
    user: dev-user

users:
- name: test-user
  user:
    client-certificate: /etc/kubernetes/pki/users/test-user/test-user.crt
    client-key: /etc/kubernetes/pki/users/test-user/test-user.key
- name: dev-user
  user:
    client-certificate: /etc/kubernetes/pki/users/dev-user/developer-user.crt
    client-key: /etc/kubernetes/pki/users/dev-user/dev-user.key
- name: aws-user
  user:
    client-certificate: /etc/kubernetes/pki/users/aws-user/aws-user.crt
    client-key: /etc/kubernetes/pki/users/aws-user/aws-user.key

current-context: test-user@development
preferences: {}
```

## 7.3. API Groups

```
curl https://mycluster:6443/version
                                # check API version
```                                

API's are grouped as see below sample structure.
- under `apis`, 2nd level are API Groups
- then goes to resources
- and actions (list, get, create etc)
```
/api
  /v1
    /namespaces
    /pods
    /rc
    /event
    /endpoints
    /nodes
    /PV
    /PVC
    /bindings
    .
    .
/apis
  /apps
    /v1
      /deployments
        list
        get
        create
        delete
      /replicasets
      /statefulsets
      .
      .
  /extensions
  /networking.k8s.io
    /v1
      /networkpolicies
      .
      .
  /storage.k8s.io
  /authentication.k8s.io
  /certificates.k8s.io
    /v1
      /certificatesigningrequests
      .
      .
  .
  .
```

## 7.4. kubectl proxy
- When you access kube-api without authentication, limited information will be seen. 
- you can give credential in curl command itself to access the info.
- or you can use kubectl proxy to use local credential to access the kubernetes api

```
$ kubectl proxy
Starting to serve on 127.0.0.1:80001

$ curl http://localhost:8001 -k
```

## 7.5. Role Based Access Controls in kuberenetes
- define `role` to access kubernetes resources
```
rules:
- apiGroups: ["apps", "extensions"]
  resources: ["pods","deployments"]
  verbs: ["list", "create"]
```  
- define `rolebinding` to link user object to tole object

 
```
kubectl get roles                     # list roles
kubectl get resources
kubectl get rolebindings

kubectl auth can-i create deployments
                                      # check access
kubectl auth can-i create deployments \
  --as user1
                                      # check access for a user
kubectl auth can-i create deployments \
  --as user1 \
  --namespace dev                     
                                      # check access for a user in a namespace
kubectl describe rolebinding weave-net -n kube-system  
                                      # details of roles and accounts 
```

### 7.5.1. Sample Role Definition

```
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "create"]
```

### 7.5.2. Sample RoleBinding Definition

```
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: dev-user-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```  

## 7.6. Cluster Roles
- to manage cluster level resources (eg: nodes, storage etc)
```
kubectl get clusterrol
kubectl get clusterrolebinding
```

### 7.6.1. ClusterRole Defintion

See 2 samples below
```
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: node-admin
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "watch", "list", "create", "delete"]

---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: storage-admin
rules:
- apiGroups: [""]
  resources: ["persistentvolumes"]
  verbs: ["get", "watch", "list", "create", "delete"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "watch", "list", "create", "delete"]
```

### 7.6.2. ClusterRoleBinding Defintion

```
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: johnt-binding
subjects:
- kind: User
  name: johnt
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: node-admin
  apiGroup: rbac.authorization.k8s.io

---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: johnt-storage-admin
subjects:
- kind: User
  name: johnt
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: storage-admin
```  

## 7.7. Image Security

- default registry - docker.io
- google - gcr.io

- To pull image from registry (eg: private registry) docker/kubernetes need access
- This is achived via `docker-registry secret`
```
$ kubectl create secret docker-registry regcred \
    --docker-server=myregistry
    --docker-username=registry-user
    --docker-password=registry-password
    --docker-email=registry-user@example.com
```
- then specify the image pull secret under the `imagePullSecrets` of pod/deployment definition (same level of `container`)
```
    imagePullSecrets:
    - name: regcred
```        

## 7.8. Security Contexts
- to specify pod level`securityContext`, add in pod spec

```
spec:
  securityContext:
    runAsUser: 1001
```    
- To specify container level `securityContext` add inside container spec

  to run the `sleep` command as user ID `1010`,
```
spec:
  containers:
  - command:
    - sleep
    - "4800"
    securityContext:
      runAsUser: 1010
```

### 7.8.1. Adding Capabilities
- You can add capabilities only container level and not pod level.

```
spec:
  containers:
  - command:
    - sleep
    - "4800"
    securityContext:
      runAsUser: 1010
      capabilities:
        add: ["SYS_TIME"]  # or ["MAC_ADMIN"] etc
```

## 7.9. Network Policies
- an object which you can attach to another object like pods to implment network rules.
- use labels & selectors to apply network policies.

```
kubectl get networkpolicies
```

on pod: 
```
labels:
  role: db
```

on network policy:
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod
    ports:
    - protocol: TCP
      port: 3306
```

Another policy definition to restrict Egress traffic
- allow all ingress
- allow only mysql:3306 and payroll:8080 as egress

```
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080
```      

# 8. Storage in kubernetes

## 8.1. Volumes
- volume attach to pod
- Kubernetes support different storage solutions (NFS, GlusterFS, Flocker,Ceph, SCALEIO, AWS EBS, Google Persistent Disk, Azure Disk etc)

Sample pod definition
```
.
.
spec:
  containers:
    volumeMounts:
    - mountPath: /data
      name: data-volume
  volumes:
  - name: data-volume
    hostPath:      
      path: /data
      type: Directory
```      

## 8.2. Persistent Volumes (PV)
- instead of adding volume information in pods, use PV and PVC
- PV accessmode can be configured as `ReadOnlyMany`, `ReadWriteOnce` or `ReadWriteMany`.

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv101
spec:
  accessMode:
    - ReadWriteMany
  capacity:
    storage: 1Gi
  hostPath:
    path: /my-data-dir
```


Replace `hostPath` with proper storage path.
```
  awsElasticBlockStore:
    volumeID: VOLUME_ID
    fsType: ext4
```

## 8.3. Persistent Volumes Claimes (PVC)

- kubernetes will match and bind PVC with PV.
- when we delete PVC, underlying PV will remain in `Retain` state by default. We can choose `persistentVolumeReclaimPolicy` as `Delete` to free up storage or choose `Recycle` to cleanup data and make PV available for next PVC.

Create a PVC `data-volume-claim101`
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-volume-claim101
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
```

## 8.4. Use PV & PVC in Pods

Let's use `data-volume-claim101` in our pod.
```
spec:
  containers:
    volumeMounts:
    - mountPath: /log
      name: log-volume

  volumes:
  - name: log-volume
    persistentVolumeClaim:
      claimName: data-volume-claim101
```

```
kubectl get pv
kubectl get pvc
```

# 9. Networking in Kubernetes

## 9.1. Docker Networking Types

### 9.1.1. none - no connectivity
```
docker run --network none nginx
```

### 9.1.2. Host network
- will use hostn network and act as in same network.
```
docker run --network host nginx
```

### 9.1.3. Bridge
- connect to an internally created bridge
- default network `172.17.0.0`
- `bridge` network will be create by default when we install docker
- default interface on bridge will be havaing IP `172.17.0.11`
- whenever a container is created, docker create a network namespace
- docker also create a virtual cable (link) which is connected to bridge and network namespace
- both side of virtual cable will get an IP address- bridge side and network namespace

```
ip -n NETWORK_NAMESPACE addr     # show ip address inside the network namespace
```
- so container can talk each other as they are in same network.
- container and docker host can talk each other as they are in same network
- to access container from other network we need to ask docker to map ports. Docker achive this by implementing rules (iptable) to forward traffic.

eg:
```
$ iptables \
  -t nat \
  -A DOCKER \
  -j DNAT \
  --dport 8080 \
  --to-destination 172.17.0.3:80

$ iptables -nvL -t nat        # show nat rules
```

### 9.1.4. Container Networking Interface (CNI)
- a standard to support network operations
- support different container technologies such as rkt, MESOS, kubernets implemented CNI, but docker does not implemented CNI
- docker has its own network standard called **CONTAINER NETWORK MODEL (CNM)**
- `bridge` is a Network Plugin for CNI 
- Other plgin example : VLAN, IPVLAN, MACVLAN, WINDOWS, DHCP, host-local etc


## 9.2. Cluster Networking

- master should accept connection on `6443` or API Server
- `kubelet` on master and worker nodes should listen on `10250`
- `kube-scheduler` should listen on `10251`
- `kube-controller-manager` should be able to listen on `10252`
- worker-nodes should listen `30000-32767` for external access of pods
- `etcd` will listen on port `2379`
- `etcd` client will communicate over 2380 if you have more than one master with etcd
 
## 9.3. Pod Networking

- CNI will help to manage networking between pods
- `kubelet` on each node responsible for creating containers
- `kubelet` will looking for command line argunments as `--cni-conf-dir=/etc/cni/net.d`
- `kubelet` will find scripts in path `--cni-bin-dir=/opt/cni/bin` (`./net-script.sh add CONTAINER NAMESPACE`)

## 9.4. CNI in Kubernetes

- CNI component is configured in `kubelet` service in each node,
```
ExecStart=/usr/local/bin/kubelet \\
  .
  .
  --network-plugin=cni \\
  --cni-bin-dir=/opt/cni/bin \\
  --cni-conf-dir=/etc/cni/net.d \\
  .
  .
```
- You can see available cni plugins in `/opt/cni/bin` (`ls /opt/cni/bin`)
- You can identify the configured CNI plugin by checking `ls /etc/cni/net.d/`

## 9.5. Weaveworks CNI Plugin

- when we deploy weave CNI in a cluster, it will deploy an agent on all nodes in the cluster
- each agent will keep topology and information about the network
- weave create its own bridge on the node and assign IP 

### 9.5.1. Deploy weave

```
$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
serviceaccount/weave-net created
clusterrole.rbac.authorization.k8s.io/weave-net created
clusterrolebinding.rbac.authorization.k8s.io/weave-net created
role.rbac.authorization.k8s.io/weave-net created
rolebinding.rbac.authorization.k8s.io/weave-net created
daemonset.apps/weave-net created
```

## 9.6. Weavenet IP Address Management (IPAM)

- IP are keeping inside a file in each node
- to manage pool of IPs, weavenet use `DHCP` & `host-local` plugins
- We can use these plugins inside CNI, under IPAM sesction
- Weaveworks - default subnet 10.32.0.0/12 for entire network
- Then split into subnet for each node.

## 9.7. Service Networking

- service is hosted across the cluster and will be available on all ndoes
- service will be accessible within cluster only and this is called cluster IP
- to make the service available outside of the cluster, we use nodeport

- An IP will be assigned when service is created
- Then, on each node forwarding rules will be created to route the traffic to from service to correct pods.
- kube-proxy will use `userspace`, `iptables` or `ipvs` to create it
- the service IP range is set `kube-api-server --service-cluster-ip-range=10.96.0.0/12` (default is `10.0.0.0/24`)
- rules can see by `iptables -L -t net |grep SERVICE_NAME` 
- also we can see details in log `/var/log/kube-proxy.log`

## 9.8. DNS in kubernetes
- kubenetes setup a built in DNS server when we build a cluster
- whenever a service is created, kubernetes will create a record in DNS to map the service name with service IP 
- `SERVICE_NAME.NAMESPACE.svc.cluster.local`
- kubernets create name record for pods as well, by using ip address of pod but replacing `.` with `-` - eg: `10-244-2-5.NAMESPACE.pod.cluster.local`

## 9.9. CoreDNS in kubernetes

- kube-dns - from 1.12 onwards, recommended solution is to use `CoreDNS`
- CoreDNS will deploy as pod in kubernetes and for reduntancy, it will deploy as replicaset
- confuguration in `/etc/coredns/Corefile` (check by `kubectl exec <core-dns-pod-name> -n kube-system ps`)
- config file is passed as a `configmap` object (`kubectl get configmap -n kube-system`)
- Finde details - `kubectl describe configmap coredns -n kube-system`
- nameserver information will be recorded in `/etc/resolv.conf`
- nameserver IP will be the service IP of CoreDNS
 
## 9.10. Ingress
- enable to access the application via url

### 9.10.1. Ingress Controller
- nginx, contour, HAPROXY, traefik, istio

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /pay
        backend:
          serviceName: pay-service
          servicePort: 8282
```
          

Samepl :
```
kubectl create namespace ingress-space
kubectl create configmap nginx-configuration --namespace ingress-space
                                    # create a configmap
kubectl create serviceaccount ingress-serviceaccount --namespace ingress-space
                                    # create a service account
kubectl get roles,rolebindings --namespace ingress-space
                                    # see rolebindings

```

Create ingress controller
```
---
apiVersion: extensions/v1beta1                                                          
kind: Deployment
metadata:
  name: ingress-controller
  namespace: ingress-space
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:                                                                                                   
    metadata:
      labels:
        name: nginx-ingress
    spec:
      serviceAccountName: ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration
            - --default-backend-service=app-space/default-http-backend
          env:
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```              

Create a service
```
---
apiVersion: v1
kind: Service
metadata:
  name: ingress
  namespace: ingress-space
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    nodePort: 30080
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress
```

Create ingress resources
```
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
  namespace: app-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
spec:
  rules:
  - http:
      paths:
      - path: /wear
        backend:
          serviceName: wear-service
          servicePort: 8080
      - path: /watch
        backend:
          serviceName: video-service
          servicePort: 8080
```

# 10. Installing Kubernetes

- kubeadm for on-prem
- GKE on GCP
- Kops for AWS
- AKS for Azure (Azure Kubernetes Service)

## 10.1. minikube
  - Deploys VM
  - Single node/All in one

## 10.2. kubeadm
  - Requires VM to be ready
  - Single or multi-node cluster
  
## 10.3. Production Clusters 

### 10.3.1. Turnkey Solutions
- You need to provision VM and configure them
- use scritps and tools to install kubernetes cluster
- need to maintain VM
  eg: Kops on AWS
  
### 10.3.2. Hosted Solutions
- Kubernetes as a service
- provider provision VMs and maintain it
- provider install kubernetes
  eg: GKE

**Choosing a Network Solution**

- https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model
- https://www.objectif-libre.com/en/blog/2018/07/05/k8s-network-solutions-comparison/


## 10.4. HA in Kubernetes
- api server :6443, behind an LB when multiple api-server running; can run as active-active
- scheduler should be active-standby; leader selection will happen based on end-point reporting
```
kube-controller-manager --leader-elect true
```
- etcd can run on master nodes but recommended way is to schedule it on dedicated etcd node.

## 10.5. Get etcdctl utility if it's not already present.
Reference: https://github.com/etcd-io/etcd/releases

```
ETCD_VER=v3.3.13

# choose either URL
GOOGLE_URL=https://storage.googleapis.com/etcd
GITHUB_URL=https://github.com/etcd-io/etcd/releases/download
DOWNLOAD_URL=${GOOGLE_URL}

rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
rm -rf /tmp/etcd-download-test && mkdir -p /tmp/etcd-download-test

curl -L ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
tar xzvf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp/etcd-download-test --strip-components=1
rm -f /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz

/tmp/etcd-download-test/etcd --version
ETCDCTL_API=3 /tmp/etcd-download-test/etcdctl version

mv /tmp/etcd-download-test/etcdctl /usr/bin
```

## 10.6. Installing kubernetes
https://github.com/mmumshad/kubernetes-the-hard-way


## 10.7. Configuring kubectl for remote access

## 10.8. Deploy POD networking solution
- cluster rolebinding must be in place for accessing kubelet on worker node by api-server

## 10.9. kubernetes cluster test suite - kubetest
```
go get -u k8s.io/test-infra/kubetest
kubetest --extract=v1.11.3
cd kubernetes
export KUBE_MASTER_IP="MASTER_IP:6443"
export KUBE_MASTER=master-1
kubetest --test --provider=skeleton 
                          # means local cluster
```

Test will take about 12 hrs

## 10.10. Deploy a Kubernetes Cluster with KubeAdm

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

```
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg |  apt-key add -
cat <<EOF | tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

For lab case use correct version

```
apt-get install kubeadm=1.10.0-00
```

# 11. Troubleshooting kubernetes

Read [Official Troubleshooing Doc](https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/).

- check node status

```
kubectl get nodes
kubectl describe node NODE_NAME

# check node CPU, Memory etc
top
df -h

# check certificates
openssl x509 -in CERTIFICATE_PATH_NAME -text
```

- check pods

```
kubectl get pods
```

- Check services on master nodes

```
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
```

- Check services on 

```
service kubelet status
service kube-proxy status
```

- check pods is control-plane are deployed as pods

```
kubectl get pods -n kube-system
```

- check service logs

```
kubectl logs kube-apiserver-master -n kube-system

# or for service
sudo journalctl -u kube-apiserver
```

# 12. kubectl advanced usage

### 12.1. JSON PATH in kubectl

Learn JSON Path for filtering and quering kubernetes info

```
kubectl get nodes -o=jsonpath='{.item[*].metadata.name}'
```

**Sample usages**

```
kubectl get nodes -o=custom-columns=COLUMN_NAME:JSON_PATH
kubectl get nodes -o=custom-columns=NODE:.metadata.name, CPU:.status.capacity.cpu

kubectl get nodes --sort-by=.metadata.name

kubectl get nodes -o json
                              # get node details in json format
kubectl get nodes -o=jsonpath='{.items[*].metadata.name}'
                              # get node names only
kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.osImage}'
                              # get node osImage
kubectl config view --kubeconfig=/root/my-kube-config -o=jsonpath='{.users[*].name}'
                              # read kubeconfig from a file and filter
kubectl get pv --sort-by=.spec.capacity.storage
                              # sort by a field
kubectl get pv --sort-by=.spec.capacity.storage \
  -o=custom-columns=NAME:.metadata.name,CAPACITY:.spec.capacity.storage
                              # sort and use custom columns
kubectl config view --kubeconfig=my-kube-config \
  -o jsonpath="{.contexts[?(@.context.user=='aws-user')].name}"
                              # use custom search inside jsonpath

```
=======================================================================

# 13. Appendix

### 13.1. Editing a pod
You cannot edit specifications of an existing pod, except,
  - spec.containers[*].image
  - spec.initContainers[*].image
  - spec.activeDeadlineSeconds
  - spec.tolerations

If you really want to edit, use `kubectl edit pod POD_NAME`. But when you try to save, kubernetes will not allow the same and save your pod definition in a temporary location. So you can delete the existing pod and create the pod using the definition file again.

You can also export the pod definition to a YAML file by `kubectl get pod myapp -o yaml > my-new-pod.yaml` and later create a new pod using this file.


