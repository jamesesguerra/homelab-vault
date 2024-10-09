1. Update package repositories and install Samba
```sh
sudo apt update
sudo apt install samba
```

2. Check status to ensure that it's running
```sh
systemctl status smbd
```

3. Go to Samba config directory in `/etc`  and rename Samba config to back up
```sh
cd /etc/samba
sudo mv smb.conf smb.conf.bak
```

4. Stop the samba service
```sh
systemctl stop smbd
```

5. Create new Samba configuration files
```sh
# smb.conf
[global]
server string = File Server
workgroup = WORKGROUP
security = user
map to guest = Bad User
name resolve order = bcast host
include = /etc/samba/shares.conf
```

```conf
# shares.conf
[Text Files]
path = /share/text_files
force user = smbuser
force group = smbgroup
create mask = 0664
force create mode = 0664
directory mask = 0775
force directory mode = 0775
public = yes
writable = yes
```

6. Create the directories you specified in the shares
```sh
sudo mkdir -p /share/text_files
```

7. Add the Samba group specified in the configuration files
```sh
sudo groupadd --system smbgroup
cat /etc/group # check if it exists
```

8. Create new Samba system user (can't log in)
```sh
sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser

cat /etc/passwd # check if new user exists
```

9. Change the ownership of the share directory so that the user and group own them
```sh
sudo chown -R smbuser:smbgroup /share
```

10. Change the read/write access of the shared directory
```sh
sudo chmod -R g+w /share 
```

11. Allow connections to the port of the Samba file server if using ufw
```sh
sudo ufw allow Samba
```

