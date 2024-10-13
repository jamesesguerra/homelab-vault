1. Update package repositories
```sh
sudo apt update
```

2. Install OpenSSH server
```sh
sudo apt install openssh-server
```

3. Start the SSH server (if not already started)
```sh
sudo systemctl start ssh
```

4. Verify that the computer is listening on the standard SSH ports
```sh
sudo ss -ltup
```

5. Allow connections to the SSH TCP ports if using UFW
```sh
sudo ufw allow OpenSSH
# or
sudo ufw allow ssh
```

6. Connect to the SSH server
```sh
ssh james@192.168.55.117
```

7. Test copying a folder to the server
```sh
scp -r ssh-test james@192.168.55.117:~/ssh-test
```

8. Create a private / public key pair on the client
```sh
ssh-keygen
```

9. Secure copy the public key to the server
```sh
scp homelab.pub james@192.168.55.117:~/.ssh/homelab.pub
```

10. Add the public key to the `authorized_keys` file of the server

11. You can also do steps 9 & 10 with one command
```sh
ssh-copy-id -t <path-to-key> james@192.168.55.117
```

