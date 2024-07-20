## What is dns and how it works.
The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through domain names, like nytimes.com or espn.com. Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to IP addresses so browsers can load Internet resources

![UntitledDiagram](https://github.com/user-attachments/assets/98c9c770-7554-449e-8451-7b8f78e5f415)


- >>> Check IP Addresses for dhcp

  ![image](https://github.com/user-attachments/assets/09d83d6a-c6cf-4e8f-9aed-bde1f28d38d1)

### Step #1: Find the netplan Configuration Files
These files are typically located in the /etc/netplan/ directory. You can list them using the following command:

     ls /etc/netplan/
     
![image](https://github.com/user-attachments/assets/6f41c285-8459-4a95-a0f2-ce7570e89e93)

### Step #2: Edit the Configuration File

Open the 01-netcfg.yaml file with a text editor like Nano or Vim.

For the demonstration, we’ll open it with Vim:

     sudo vim/etc/netplan/00-installar-config.yaml
     
This is Nwetwork Configuration file 

              network:
                version: 2
                renderer: networkd
                ethernets:
                  enp1s0:
                    addresses:
                      - 192.168.122.56/24
                    nameservers:
                      addresses: [4.2.2.2, 8.8.8.8]
                    routes:
                      - to: default
                        via: 192.168.122.1
~                            

Find the relevant network interface section and add the new DNS server informatio

### Step #3: Activate Changes
- >>> then Reboot now, after that check ip addresses for static

![image](https://github.com/user-attachments/assets/6fbed3c9-0fdd-43fb-ae17-886c9df833cc)

    cat /etc/hosts
    
            127.0.0.1	localhost
            192.168.122.56  dns.harikesh.com dns
            ::1     ip6-localhost ip6-loopback
            fe00::0 ip6-localnet
            ff00::0 ip6-mcastprefix
            ff02::1 ip6-allnodes
            ff02::2 ip6-allrouters

- >>> Check hostname & domainname



  ![image](https://github.com/user-attachments/assets/5eb8363d-d6a0-46dd-bc91-e80918f68c8f)

  Then Update System

           sudo apt update -y
  
Installation of Bind9 

        sudo apt install bind9 bind9utils bind9-doc
    sudo apt install bind9 bind9utils bind9-doc



