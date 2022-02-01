# NFS-Configurations-Ubuntu

## Basic NFS Configurations 
☁️**AWS**

👉1 VPC (default VPC is enough)
👉2 Intances (1 Server 1 Clients)
👉2 Elastic IPs (1 for each one of the Instances)

🖥️**Termius**

▶️ **Server**

◽First step do make its to update and upgrade your machine ➡️ `apt update && aptupgrade ` 

◽Install the NFS `apt install nfs-kernel-server`

❗ You will need a file for where you will export ➡️ `mkdir /var/homes` (/var/homes its a file that it is made by me ,the name doesnt metter and where it is,only matters that you specify on the next step.)
🔽
```
# code block
- nano /etc/exports (this page is going to be edited so we can indicate which page to export,in this case is /var/homes)
--------------------------------------------------------------------------------------------------------------------
# /etc/exports: the access control list for filesystems which may be exported                                                                  
#               to NFS clients.  See exports(5).                                                                                               
#                                                                                                                                              
# Example for NFSv2 and NFSv3:                                                                                                                 
# /srv/homes       hostname1(rw,sync,no_subtree_check) hostname2(ro,sync,no_subtree_check)                                                     
#                                                                                                    
# Example for NFSv4:
# /srv/nfs4        gss/krb5i(rw,sync,fsid=0,crossmnt,no_subtree_check)
# /srv/nfs4/homes  gss/krb5i(rw,sync,no_subtree_check)
#
/var/homes *(rw,sync,no_subtree_check,no_root_squash) -> add this in the end of this file
---------------------------------------------------------------------------------------------------------------------
- exportfs -a  -> this command is used to export the files indicated in /etc/exports (the command done before)

``` 
◽Restart the NFS server ➡️ `systemctl restart --now nfs-kernel-server` ❗

◽Its needed a user
```
# code block
adduser   _______ --home /var/homes/_______  -> (ex: adduser azores --home /var/homes/azores)
echo xfce4-session > /var/homes/_______/.xsession -> (ex: echo xfce4-session > /var/homes/azores/.xsession)
chown ______:______ /var/homes/________/.xsession -> (chown azores:azores /var/homes/azores/.xsession)
```

▶️**Clients**

◽First Step is to update the machine ➡️ `apt update && aptupgrade `

◽Install NFS ➡️ `apt install nfs-common`

◽You need to create the directory with the same name of the server ❗ ➡️ `mkdir /var/homes`

◽You need to mount ➡️ `mount -t nfs serverIP:/var/homes /var/homes` ➡️ this command is used to connect to server through the path done in /etc/exports ❗

◽To save the mount ➡️ `cat /etc/mtab` copy the line ⤵️ to `/etc/fstab` 

![imagem](https://user-images.githubusercontent.com/96176081/152041454-ef946f71-3df3-43a0-98d7-c3f2b7d2e95e.png) and add ,nofail ❗

✔️to check if its working, on Client do `cd /var/homes` and if the file with the user name is there its the confirmation that is working.




