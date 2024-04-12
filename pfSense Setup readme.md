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

- >>> *./ldap.sh*

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

### [5.3].Create Organization_unit ldif file (file extension name , ldif ) 

    cat organisation.ldif

- **we have create 5 organisation ( Dev, Support, POC, Document,    Observability)**

        dn: dc=keenable,dc=com
        objectClass: top
        objectClass: domain
        dc: keenable

        dn: ou=Dev,dc=keenable,dc=com
        objectClass: top
        objectClass: organizationalUnit
        ou: Dev

        dn: ou=Support,dc=keenable,dc=com
        objectClass: top
        objectClass: organizationalUnit
        ou: Support

        dn: ou=POC,dc=keenable,dc=com
        objectClass: top
        objectClass: organizationalUnit
        ou: POC

        dn: ou=Document,dc=keenable,dc=com
        objectClass: top
        objectClass: organizationalUnit
        ou: Document

        dn: ou=Observability,dc=keenable,dc=com
        objectClass: top
        objectClass: organizationalUnit
        ou: Observability

![Screenshot from 2024-04-01 16-15-12](https://github.com/harikesh1996/readme.md/assets/82168975/4db2d666-1451-4499-9199-39286f556efd)

- >>> Run this command to add **"organisation.ld"**

      ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -W  -f organisation.ldif

  ### [5.4]. Create 2 group inside Support
  
   cat  group.ldif


	    dn: cn=Admins,ou=Support,dc=keenable,dc=com
	    objectClass: top
	    objectClass: groupOfUniqueNames
	    cn: Admins
	    uniqueMember: uid=user1,ou=Support,dc=keenable,dc=com
	
	    dn: cn=SupportTeam,ou=Support,dc=keenable,dc=com
	    objectClass: top
	    objectClass: groupOfUniqueNames
	    cn: SupportTeam
	    uniqueMember: uid=user2,ou=Support,dc=keenable,dc=com
  ![Screenshot from 2024-04-01 16-24-33](https://github.com/harikesh1996/readme.md/assets/82168975/bb027e41-422d-4c08-97e0-ebbde924dc1b)


- >>> Run this command to add **"group.ldif"**

        
    ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -W  -f group.ldif

  ![Screenshot from 2024-04-01 16-27-09](https://github.com/harikesh1996/readme.md/assets/82168975/de8aac70-e827-4aaa-bf5f-d6ef89a8a913)


#  6. SQUID Configuration on Container

	 podman search squid	
  ![Screenshot from 2024-04-03 14-37-36](https://github.com/harikesh1996/readme.md/assets/82168975/accdb792-b6a8-45ac-9bbf-bd92680415c8)

	podman pull docker.io/ubuntu/squid
![Screenshot from 2024-04-03 14-40-16](https://github.com/harikesh1996/readme.md/assets/82168975/1322e129-e183-47a2-966f-e4eac380258f)

	 podman ps

	 podman run  -d –name squid -p 3128:3128 docker.io/ubuntu/squid:latest
  ![Screenshot from 2024-04-03 14-41-40](https://github.com/harikesh1996/readme.md/assets/82168975/07221fb0-4d34-4edd-9e12-82c9a6325d4b)

	padman exec -it squid /bin/bash
![Screenshot from 2024-04-03 14-42-40](https://github.com/harikesh1996/readme.md/assets/82168975/d9bd13f8-6ec5-4dfb-806a-1a00f6f062ed)


Then, Going to squid Configuration file:

	vim /etc/squid/squid.conf

 
	 acl block_url dstdomain "/etc/squid/block.acl"
	http_access deny block_url

	auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

	acl squid_users proxy_auth REQUIRED
	http_access allow squid_users

	# LDAP Authentication Parameters
	#auth_param basic program /usr/lib/squid/basic_ldap_auth \
	#  -b "dc=fspl,dc=com" \
	#  -h ldap://10.0.0.15:3389 \
	#  -D "cn=Directory Manager" \
	#  -w "redhat" \
	#  -s sub \
	#  -f "(&(objectclass=inetOrgPerson)(uid=%s))"

	# Define LDAP Group ACL
	#external_acl_type ldap_group %LOGIN /usr/lib/squid/ext_ldap_group_acl \
	#  -b "dc=fspl,dc=com" \
	#  -f "(&(objectclass=groupOfUniqueNames)(uniqueMember=%u))" \
	#  -F uid="%s" \
	#  -D "cn=Directory Manager" \
	#  -w "redhat" \
	#  -h ldap://10.0.0.15:3389
	#acl user1_acl proxy_auth "/etc/squid/fsplusers.txt"
	#acl block dstdomain "/etc/squid/block1.txt"
	#http_access deny user1_acl block
	#http_access allow user1_acl

then, squid service restart 

	podman restart squid
 ![image](https://github.com/harikesh1996/readme.md/assets/82168975/93e071fd-b5ca-4bd4-9867-fd2d3aa0a9c2)

 Then  going into the squid container.  
 
		 podman exec -it squid /bin//bash

- >> user & password create for squid ncsa auth

![Screenshot from 2024-04-03 14-51-37](https://github.com/harikesh1996/readme.md/assets/82168975/b79f2a76-239c-4021-b28d-5c9748c5fe57)

User & Password create for squid Authenticate

	htpasswd -c /etc/squid/passwd harikesh

 Then I am going to check the squid proxy server on the client machine for Access. 
![Screenshot from 2024-04-03 14-57-05](https://github.com/harikesh1996/readme.md/assets/82168975/68c4fbcd-e474-44d2-a8a4-87369da1da89)


==========================================================================
# 7. Using  LDAP to authenticate Squid proxy users

	podman run -dt --name pfsense-ldap -p 3391:3389 -v /home/client1/data:/data -e DS_SUFFIX_NAME=dc=keenable,dc=com -e DS_DM_PASSWORD=12345 quay.io/389ds/dirsrv
![Screenshot from 2024-04-03 15-00-35](https://github.com/harikesh1996/readme.md/assets/82168975/d0794c6c-6780-4ce5-8eb6-8a9ee9e83e5c)


	podman exec -it pfsense-ldap /bin/bash

	dsconf -D "cn=Directory Manager" ldap://localhost:3389 backend suffix list

  ![Screenshot from 2024-04-03 15-01-54](https://github.com/harikesh1996/readme.md/assets/82168975/8aaf2a4f-b132-4fd0-b7c9-47c22a12163c)

- >> For Database creation.

		dsconf -D "cn=Directory Manager" ldap://localhost:3389 backend create --suffix "dc=keenable,dc=in" --be-name "keenable.in"
  

![Screenshot from 2024-04-03 15-06-53](https://github.com/harikesh1996/readme.md/assets/82168975/0901f230-e8cf-4902-9af7-ec65ecad92bc)

client1@client1:~$ sudo vim org.ldif

dn: dc=keenable,dc=com

objectClass: top

objectClass: domain

dc: keenable

![Screenshot from 2024-04-03 15-09-43](https://github.com/harikesh1996/readme.md/assets/82168975/45ac652a-c4fc-4bbc-832a-42b1d412d99c)

- >>> Run this command to add **"org.ldif"**

		ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -W -f org.ldif

![Screenshot from 2024-04-03 15-11-55](https://github.com/harikesh1996/readme.md/assets/82168975/a22378cc-2127-4a61-9ed0-b7163c81c758)
![Screenshot from 2024-04-03 15-12-58](https://github.com/harikesh1996/readme.md/assets/82168975/c8190f09-bb75-4603-b6f6-efb6414376e1)

client1@client1:~$ cat all.ldif

	# LDIF for Organization with User IDs and Passwords

	# Keenable
	dn: o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: organization
	o: Keenable
	
	# Employees
	dn: ou=employees,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: organizationalUnit
	ou: employees
	
	# Rajiv Kumar in Employees
	dn: cn=rajiv,ou=employees,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: person
	cn: rajiv
	sn: kumar
	userPassword: 12345
	
	# Harikesh chourasiya in Employees
	dn: cn=Harikesh,ou=employees,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: person
	cn: harikesh
	sn: chourasiya
	userPassword: 12345
	
	# bheem prashad in Employees
	dn: cn=bheem,ou=employees,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: person
	cn: bheem
	sn: prashad
	userPassword: 12345
	
	# TL
	dn: ou=tl,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: organizationalUnit
	ou: tl
	
	# Amit Sir in TL
	dn: cn=amitsir,ou=tl,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: person
	cn: amitsir
	sn: sir
	userPassword: 12345
	
	# Mentors
	dn: ou=mentors,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: organizationalUnit
	ou: mentors
	
	# Pooja mam in mentors
	dn: cn=poojama  ,ou=mentors,o=Keenable,dc=keenable,dc=com
	objectClass: top
	objectClass: person
	cn: Poojamam
	sn: mam
	userPassword: 12345
 ![Screenshot from 2024-04-03 17-28-46](https://github.com/harikesh1996/readme.md/assets/82168975/c13ecb59-0357-4079-8a68-484c68f385ee)


- >>> Run this command to add **"all.ldif"**

 		 ldapadd -a -c -xH ldap://localhost:3389 -D "cn=Directory Manager" -W -f all.ldif

![Screenshot from 2024-04-03 17-25-39](https://github.com/harikesh1996/readme.md/assets/82168975/c461e5b5-db59-4e03-b52b-841c9890bc0d)

  
    
		 ldapsearch -xH ldap://localhost:3389 -D "cn=Directory Manager" -w "12345"

![Screenshot from 2024-04-03 16-07-26](https://github.com/harikesh1996/readme.md/assets/82168975/4ee9b3db-6cb9-4a23-bd12-39699447134c)

##### Squid.conf
		podman exec -it squid /bin/bash
  -----------------------------------------------
	
 	vim /etc/squid/squid.conf
	 # LDAP authentication for all users
	auth_param basic program /usr/lib/squid/basic_ldap_auth \
	  -b "dc=keenable,dc=com" \
	  -h ldap://10.0.0.15:3389 \
	  -D "cn=Directory Manager" -w "12345" \
	  -s sub -f "(&(objectclass=inetOrgPerson)(uid=%s))"
	auth_param basic children 50
	auth_param basic realm Web-Proxy
	auth_param basic credentialsttl 1 minute
	
	# Define ACL for LDAP authentication
	acl ldap-auth proxy_auth REQUIRED
	
	http_access allow ldap-auth
	
	# YouTube blocking
	external_acl_type interns_ldap_acl %LOGIN /usr/lib/squid/ext_ldap_interns_acl -b "dc=keenable,dc=com" -D "cn=Directory Manager" -w "12345"
	acl interns_users external interns_ldap_acl
	
	# ACL for YouTube
	acl youtube dstdomain .youtube.com
	
	# Block YouTube for the specific LDAP user 'raju123'
	http_access deny youtube interns_users
	
	http_access allow ldap-auth

-------------------------------------------------
sudo nano /usr/lib/squid/ext_ldap_al_acl

	 #!/bin/bash
	SEARCH_BASE="dc=keenable,dc=in"
	LDAP_SERVER="ldap://10.0.0.15:3389"
	BIND_DN="cn=Directory Manager"
	BIND_PW="12345"
	
	# Extract uid from the provided username
	USER_UID=$(echo $1 | cut -d "@" -f 1)
	
	ldapsearch -x -H "$LDAP_SERVER" -b "$SEARCH_BASE" -D "$BIND_DN" -w "$BIND_PW" -LLL "(uid=$USER_UID)" dn | grep -q "$1"
 ![Screenshot from 2024-04-03 17-44-17](https://github.com/harikesh1996/readme.md/assets/82168975/ba4fffd3-daa2-4886-84b3-6eec87cbd9f8)

For execute permission

 		chmod +x /usr/lib/squid/ext_ldap_al.acl
----------------------------------------------------------------


		 sudo tail -n 50 /var/log/squid/access.log

![Screenshot from 2024-04-03 17-47-34](https://github.com/harikesh1996/readme.md/assets/82168975/e90b35e3-ed99-4b5d-a5c4-2f58e6af3d8c)

 Apache Directory Studio
![Screenshot from 2024-04-03 17-48-14](https://github.com/harikesh1996/readme.md/assets/82168975/e29bc562-ed9b-4288-8e16-92b113f13d81)

User authentication by ldap user.
![Screenshot from 2024-04-03 17-49-01](https://github.com/harikesh1996/readme.md/assets/82168975/6a21c600-97b4-4091-b360-1ed980b9040d)
![Screenshot from 2024-04-03 17-49-58](https://github.com/harikesh1996/readme.md/assets/82168975/82682b14-b826-4e47-abc8-9cc3c096c011)

## ThankYou..........................................Harikesh Chourasiya 


		
		
		
	
	 
	 
	
	
	
	
	
















   
