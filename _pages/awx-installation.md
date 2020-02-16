# 1. AWX Installation

Ref: 
- [Ansible AWX Project](https://github.com/ansible/awx)
- [Introduction to Ansible AWX Project](https://www.doag.org/formes/pubfiles/10846538/2018-DO-Alexander_Hofstetter-Einfuehrung_in_Ansible_AWX_Project_der_quot_freie_quot_Ansible_Tower-Praesentation.pdf)
- [AWX Command Line Interface](https://github.com/ansible/awx/tree/devel/awxkit/awxkit/cli)

<!-- TOC orderedlist:true -->

- [1. AWX Installation](#1-awx-installation)
- [2. Install AWX Using Ansible](#2-install-awx-using-ansible)
  - [2.1. Configure AWX Node for Ansible](#21-configure-awx-node-for-ansible)
  - [2.2. Install Pre-Req roles](#22-install-pre-req-roles)
  - [2.3. Install AWX using Roles](#23-install-awx-using-roles)
- [3. Install AWX Manually](#3-install-awx-manually)
  - [3.1. Install epel repo and then install jq](#31-install-epel-repo-and-then-install-jq)
  - [3.2. Install docker-ce related packages](#32-install-docker-ce-related-packages)
  - [3.3. Enable docker-ce repo and install docker engine.](#33-enable-docker-ce-repo-and-install-docker-engine)
  - [3.4. Install latest docker-compose](#34-install-latest-docker-compose)
  - [3.5. Install AWX dependencies](#35-install-awx-dependencies)
  - [3.6. Download Packages](#36-download-packages)
  - [3.7. Disable dockerhub reference in order to build local images.](#37-disable-dockerhub-reference-in-order-to-build-local-images)
  - [3.8. Create a folder in /opt/ to hold awx psql data](#38-create-a-folder-in-opt-to-hold-awx-psql-data)
  - [3.9. Provide psql data path to installer.](#39-provide-psql-data-path-to-installer)
  - [3.10. Enable SSL](#310-enable-ssl)
  - [3.11. Adding Logo](#311-adding-logo)
  - [3.12. Add admin user and password](#312-add-admin-user-and-password)
  - [3.13. Install AWX](#313-install-awx)
- [4. AWX Backup & Restore](#4-awx-backup--restore)
- [5. Migration](#5-migration)

<!-- /TOC -->

# 2. Install AWX Using Ansible

You need an ansible machine (just a machine with ansible configured). This is very easy and straight forward as there are roles availalbe for installaing and configuring Ansible AWX. If you want to know the actual steps and items involved in the AWX installation, try manual method.

## 2.1. Configure AWX Node for Ansible

- Create a node (VM in public or private cloud) and make sure you this is configured for Ansible to access. (You can manually do this or just use any roles if multiple nodes). I using a very simple role **[setup_ansible_user](https://galaxy.ansible.com/ginigangadharan/setup_ansible_user)** which will add a user and configure ssh keys. (You can skip this steps if your new VM already configured with ssh keys and sudo access)

Sample content of `setup-ansible.yaml`

```
- hosts: all
  become: true
  vars_files:
  vars:
    remote_user: devops
    remote_user_public_key: 'YOUR_PUBLIC_KEY' # use your public key to add to remote node
  roles:
    - { role: setup-ansible-user }
```

- Add your AWX node to Inventory

```
[awx]
awx-node-01 ansible_host=YOUR_VM_IP
```

Remember to use proper switches to ask password, become sudo, sudo password etc.

```
ansible-playbook setup-ansible.yaml -b -K -k -u root
```

## 2.2. Install Pre-Req roles

We can use the [`awx`](https://galaxy.ansible.com/geerlingguy/awx) role by [Jeff Geerling](https://galaxy.ansible.com/geerlingguy). But, make sure you have installed all pre-req roles from Ansible Galaxy; add roles in `requirements.yml` and install.

```
$ cat requirements.yml
# from galaxy
- src: geerlingguy.repo-epel
- src: geerlingguy.git
- src: geerlingguy.ansible
- src: geerlingguy.docker
- src: geerlingguy.pip
- src: geerlingguy.nodejs
- src: geerlingguy.awx
```

Then,

```
$ ansible-galaxy install -r requirements.yml

# verify roles directory
$ ls -l roles/
total 32
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.ansible
drwxr-xr-x 7 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.awx
drwxr-xr-x 8 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.docker
drwxr-xr-x 7 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.git
drwxr-xr-x 8 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.nodejs
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:09 geerlingguy.pip
drwxr-xr-x 6 net_gini net_gini 4096 Feb 12 22:08 geerlingguy.repo-epel
drwxr-xr-x 9 net_gini net_gini 4096 Feb 11 15:15 makarenalabs.wordpress
```

## 2.3. Install AWX using Roles

Now, just call your awx install playbook; sample playbook below.

*Note : You may customize AWX installation, refer [`awx`](https://galaxy.ansible.com/geerlingguy/awx) for more.

```
- hosts: awx-node-01
  become: true

  vars:
    nodejs_version: "6.x"
    pip_install_packages:
      - name: docker-py

  roles:
    - geerlingguy.repo-epel
    - geerlingguy.git
    - geerlingguy.ansible
    - geerlingguy.docker
    - geerlingguy.pip
    - geerlingguy.nodejs
    - geerlingguy.awx
```

Install Ansible AWX

```
$ ansible-playbook awx-install.yaml 
```

After AWX is installed, you can log in with the default username `admin` and password `password`.

# 3. Install AWX Manually

## 3.1. Install epel repo and then install jq
```
yum install -y epel-release -y && yum install jq
```

## 3.2. Install docker-ce related packages
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 3.3. Enable docker-ce repo and install docker engine.
```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum -y install docker-ce
systemctl enable docker && systemctl start docker
```

## 3.4. Install latest docker-compose
```
LATEST_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.tag_name')
curl -L "https://github.com/docker/compose/releases/download/$LATEST_VERSION/docker-compose-$(uname -s)-$(uname -m)" > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## 3.5. Install AWX dependencies

```
yum install -y python2-pip
pip install ansible
pip install docker-compose
```

## 3.6. Download Packages

Change dir to the home directory and get ansible-awx release tarball and extract it. 

```
cd ~
curl \
  -o ansible-awx-6.1.0.tar.gz https://codeload.github.com/ansible/awx/tar.gz/6.1.0 && \
  tar xvfz ansible-awx-6.1.0.tar.gz && \
  rm -f ansible-awx-6.1.0.tar.gz
```

## 3.7. Disable dockerhub reference in order to build local images.
```
cd awx-6.1.0
sed -i "s|^dockerhub_base=ansible|#dockerhub_base=ansible|g" installer/inventory
```

## 3.8. Create a folder in /opt/ to hold awx psql data

```
mkdir -p /opt/awx-psql-data
```

## 3.9. Provide psql data path to installer.

```
sed -i "s|^postgres_data_dir.*|postgres_data_dir=/opt/awx-psql-data|g" installer/inventory
```

*Note: If you wish to use an external database, in the inventory file, set the value of pg_hostname, and update pg_username, pg_password, pg_database, and pg_port with the connection information.*


## 3.10. Enable SSL

```
# Create awx-ssl folder in /etc.
mkdir -p /etc/awx-ssl/

# Make a self-signed ssl certificate
openssl req -subj '/CN=labs.local/O=Labs Local/C=TR' \
	-new -newkey rsa:2048 \
	-sha256 -days 1365 \
	-nodes -x509 \
	-keyout /etc/awx-ssl/awx.key \
	-out /etc/awx-ssl/awx.crt


# Merge awx.key and awx.crt files
cat /etc/awx-ssl/awx.key /etc/awx-ssl/awx.crt > /etc/awx-ssl/awx-bundled-key.crt

# Pass the full path of awx-bundled-key.crt file to ssl_certificate variable in inventory.
sed -i -E "s|^#([[:space:]]?)ssl_certificate=|ssl_certificate=/etc/awx-ssl/awx-bundled-key.crt|g" installer/inventory
```

## 3.11. Adding Logo

```
# Change dir to where awx main folder is placed:
cd ~

# Download and extract awx-logos repository. 
curl -L -o awx-logos.tar.gz https://github.com/ansible/awx-logos/archive/master.tar.gz
tar xvfz awx-logos.tar.gz

# Rename awx-logos-master folder as awx-logos  
mv awx-logos-master awx-logos

# Remove tarball
rm -f *awx*.tar.gz
```

*Note: AWX installer which resides at `installer/install.yml`, searches for `awx-logos` directory as `../../awx-logos`. So, your `awx-logos` folder should be located at where awx installer’s parent directory is placed.*

```
# Change dir to awx and replace awx_official parameter
cd ~/awx-6.1.0
sed -i -E "s|^#([[:space:]]?)awx_official=false|awx_official=true|g" installer/inventory
```

## 3.12. Add admin user and password

```
# Define the default admin username
sed -i "s|^admin_user=.*|admin_user=awx-admin|g" installer/inventory

# Set a password for the admin
sed -i "s|^admin_password=.*|admin_password=CHANGE_ME|g" installer/inventory
```

## 3.13. Install AWX

```
# Enter the installer directory.
cd ~/awx-6.1.0/installer

# Initiate install.yml
ansible-playbook -i inventory install.yml
```

Reference:
- [Installing AWX](https://github.com/ansible/awx/blob/devel/INSTALL.md)
- [Ansible AWX Installatio](https://medium.com/swlh/ansible-awx-installation-5861b115455a)


# 4. AWX Backup & Restore

http://yallalabs.com/automation-tool/how-to-backup-restore-awx-ansible-tower-objects-tower-cli-tool/
https://www.unixarena.com/2019/03/backup-restore-ansible-awx-tower-cli.html/
https://www.insentragroup.com/au/insights/geek-speak/modern-workplace/protecting-the-automation-engine-backup-for-ansible-awx-project/


# 5. Migration

- [Migrating Data Between AWX Installations](https://github.com/ansible/awx/blob/devel/DATA_MIGRATION.md)
- [Upgrading from previous versions](https://github.com/ansible/awx/blob/devel/INSTALL.md#upgrading-from-previous-versions)