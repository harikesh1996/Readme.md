# 1. Task Requirement

Install and configure Ldap 389 DS in the containerised platform with persistence storage
Create organisation keenable.in
Create OU [Dev,Support,POC, Document, Observability]
Create group Admin,Support
Create custom attribute
#  2. Environment Detail
NAME="Ubuntu"
VERSION="20.04.6 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.6 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
#   3. List of Tools
1. Podman
2. Ldap
3. Apache Directory Studio
# 4. Definition of tools
## 1. Podman
Podman is an open-source containerization tool that manages containers and images, supporting features like pod management, rootless containers, and a daemoness architecture.
## 2. Ldap
SSThe Lightweight Directory Access Protocol is a communication protocol used to access directory servers and is used to store, update and retrieve data from a directory structure.

## 3. Apache Directory Studio
Apache Directory Studio is an open-source LDAP client with a graphical interface for managing and interacting with LDAP directories.
# 5.1 Command for the setup or configuration
Bash Script:-

    #!/bin/bash
     #create pod with name ldap389
    	podman pod create --name ldap389 --publish 3389:3389 --publish 3636:3636
	#create container
	podman run -dt\
	--pod ldap389 \
	--name 389ds-ldap \
	-v ~/389ds/data:/data \
	-e DS_SUFFIX=dc=keenable, dc=in \
	-e DS_DM_PASSWORD=<password> \
	docker.io/389ds/dirsrv

Run this script…

./ldap.ssh

Check pod list
   podman pod ps

   ![alt text](<Screenshot from 2024-04-01 13-28-32.png>)
   
