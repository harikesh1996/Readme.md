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
#### 1. Podman
Podman is an open-source containerization tool that manages containers and images, supporting features like pod management, rootless containers, and a daemoness architecture.
#### 2. Ldap
SSThe Lightweight Directory Access Protocol is a communication protocol used to access directory servers and is used to store, update and retrieve data from a directory structure.

#### 3. Apache Directory Studio
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

![Screenshot from 2024-04-01 13-22-42](https://github.com/harikesh1996/readme.md/assets/82168975/215baf32-2292-4752-af7c-09d658a562f9)


Run this script…

./ldap.ssh

Check pod list

    podman pod ps

![Screenshot from 2024-04-01 13-28-32](https://github.com/harikesh1996/readme.md/assets/82168975/9ec50b50-73c9-4e71-b92d-b31ff8c97715)


Check container list 

	podman ps

 ![Screenshot from 2024-04-01 15-43-29](https://github.com/harikesh1996/readme.md/assets/82168975/d87a4899-3b3e-443e-89d0-34b95853930e)

# [5.2]. Setup ApacheDirectory studio for ldap db UI
1. Install ApacheDirectory studio tar file and extract in directory
2. open apache directory

![Screenshot from 2024-04-01 15-46-33](https://github.com/harikesh1996/readme.md/assets/82168975/f50acc3d-6dfd-4897-8ed0-2b6ef72c54b4)

Run this command for untar this file..

  	tar -xvf (ApacheDirectoryStudio-2.0.0.v20210717-M17-linux.gtk.x86_64.tar.gz)

   ![Screenshot from 2024-04-01 15-48-56](https://github.com/harikesh1996/readme.md/assets/82168975/2168d8cf-37b2-4640-ba90-04dc03ed140a)
   ![Screenshot from 2024-04-01 15-50-36](https://github.com/harikesh1996/readme.md/assets/82168975/10a6ad65-d710-43f0-842e-e1757d8dd37a)
   ![Screenshot from 2024-04-01 15-51-22](https://github.com/harikesh1996/readme.md/assets/82168975/3813bed6-fbea-435f-b471-7d4ddd8d4a8e)

3. Go to new connection and enter some details
ldap connection name
 
Hostname -> localhost
 
ldap port > Press next (3389)

Enter Bind DN name (cn= Directory Manager)
 
Enter Bind DN password -> finish












   
