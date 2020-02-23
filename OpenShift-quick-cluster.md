---
layout: postnew
title: OpenShift Installation Methods - Examples
#author: gini
categories: [ devops ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: /OpenShift-quick-cluster
featured: false
hidden: false
---
<!-- TOC orderedlist:true -->

- [1. OpenShift Installation Methods - Examples](#1-openshift-installation-methods---examples)
- [2. Setup an OpenShift All-In-One](#2-setup-an-openshift-all-in-one)
  - [2.1. Install required packages](#21-install-required-packages)
  - [2.2. Disable Firewall](#22-disable-firewall)
  - [2.3. Installing OpenShift CLI](#23-installing-openshift-cli)
    - [2.3.1. Method 1 - Standard CentOS repositories](#231-method-1---standard-centos-repositories)
    - [2.3.2. Method 2 - Download and extract openshift origin](#232-method-2---download-and-extract-openshift-origin)
      - [2.3.2.1. Add the directory you untarred the release into to your path:](#2321-add-the-directory-you-untarred-the-release-into-to-your-path)
  - [2.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16](#24-configure-the-docker-daemon-with-an-insecure-registry-parameter-of-172300016)
    - [2.4.1. Restart docker service](#241-restart-docker-service)
  - [2.5. Initiate cluster](#25-initiate-cluster)
    - [2.5.1. Ref:](#251-ref)
- [3. Extra](#3-extra)
- [4. Setup minishift](#4-setup-minishift)
  - [4.1. Setup Virtual Environment](#41-setup-virtual-environment)
  - [4.2. Install minishift](#42-install-minishift)
  - [4.3. Start minishift cluster](#43-start-minishift-cluster)
- [5. OpenShift 4 - All in One Quick Cluster](#5-openshift-4---all-in-one-quick-cluster)

<!-- /TOC -->

# 2. Setup an OpenShift All-In-One 

## 2.1. Install required packages
```
yum install ansible docker wget -y
systemctl enable docker
systemctl start docker
```

## 2.2. Disable Firewall
Or you have to open required ports.
```
systemctl disable firewalld
systemctl stop firewalld
```

## 2.3. Installing OpenShift CLI

### 2.3.1. Method 1 - Standard CentOS repositories
```
yum -y install centos-release-openshift-origin39
yum -y install origin-clients
```

### 2.3.2. Method 2 - Download and extract openshift origin

Create a directory for data (anywhere)
```
mkdir /data
cd /data
```

Check the latest version if you need.
```
wget "https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz"
tar -xzvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz 
cd openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit
```

#### 2.3.2.1. Add the directory you untarred the release into to your path:
```
export PATH=$PATH:/data/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/

# or

export PATH=$PATH:`pwd`
```

## 2.4. Configure the Docker daemon with an insecure registry parameter of 172.30.0.0/16
```
cat > /etc/docker/daemon.json <<DELIM
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
DELIM
```

### 2.4.1. Restart docker service
```
service docker restart
```

## 2.5. Initiate cluster
```
oc cluster up --base-dir="/data/clusterup" --public-hostname=<IP>
```
- `--base-dir=BASE_DIR` : Directory on Docker host for cluster up configuration

(oc cluster up --public-hostname=35.239.51.76 --routing-suffix=35.239.51.76.xip.io)
openshift.local.clusterup/openshift-controller-manager/openshift-master.kubeconfig


### 2.5.1. Ref:
- https://github.com/openshift/origin/blob/release-3.11/docs/cluster_up_down.md
- https://medium.com/@fabiojose/working-with-oc-cluster-up-a052339ea219


# 3. Extra
```
# oc create user redhat
user.user.openshift.io/redhat created
# oc adm policy add-cluster-role-to-user cluster-admin redhat
cluster role "cluster-admin" added: "redhat"
```

# 4. Setup minishift

## 4.1. Setup Virtual Environment

- setup KVM or virtualbox (or other virtulization)
Ref: [Set up your virtualization environment](https://docs.okd.io/latest/minishift/getting-started/setting-up-virtualization-environment.html)

## 4.2. Install minishift
- [Download](https://github.com/minishift/minishift/releases) and manually install 

or 

Installation on mac
```
brew cask install minishift
```

* in case issue to install `export HOMEBREW_NO_ENV_FILTERING=1`*

## 4.3. Start minishift cluster

```
minishift start --vm-driver virtualbox

# setup VirtualBox Permanently
minishift config set vm-driver virtualbox
```

Once started, access the console using url or open in browser
```
minishift console
```

setup oc access
```
$ minishift oc-env
export PATH="/home/john/.minishift/cache/oc/v1.5.0:$PATH"
# Run this command to configure your shell:
# eval $(minishift oc-env)
```

# 5. OpenShift 4 - All in One Quick Cluster

https://github.com/openshift/okd/releases
