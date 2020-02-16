---
title: Ansible for VMware
#bigimg: /img/ansible-banner-tower.PNG
---
# Ansible for VMware
Ref: https://docs.ansible.com/ansible/latest/scenario_guides/guide_vmware.html

## Requirements
Ansible VMware modules are written on top of pyVmomi which is the Python SDK for the VMware vSphere API that allows user to manage ESX, ESXi, and vCenter infrastructure. 

```
$ pip install pyvmomi
```

## vmware_guest module
The vmware_guest module manages various operations related to virtual machines in the given ESXi or vCenter server.

## VMware Prerequisites
### Installing vCenter SSL certificates for Ansible
From any web browser, go to the base URL of the vCenter Server without port number like https://vcenter-domain.example.com
Click the “Download trusted root CA certificates” link at the bottom of the grey box on the right and download the file.
Change the extension of the file to .zip. The file is a ZIP file of all root certificates and all CRLs.
Extract the contents of the zip file. The extracted directory contains a .certs directory that contains two types of files. Files with a number as the extension (.0, .1, and so on) are root certificates.
Install the certificate files are trusted certificates by the process that is appropriate for your operating system.

### Installing ESXi SSL certificates for Ansible
Enable SSH Service on ESXi either by using Ansible VMware module vmware_host_service_manager or manually using vSphere Web interface.
SSH to ESXi server using administrative credentials, and navigate to directory /etc/vmware/ssl
Secure copy (SCP) rui.crt located in /etc/vmware/ssl directory to Ansible control node.
Install the certificate file by the process that is appropriate for your operating system.

