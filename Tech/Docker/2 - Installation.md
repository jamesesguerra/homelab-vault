[Install Link](https://docs.docker.com/engine/install/ubuntu/)

1. Set up Docker's `apt` repository
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$UBUNTU_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install the Docker packages
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

3. Verify that the Docker installation is successful
```sh
 sudo docker run hello-world
```

4. Add the current user to the `docker` group to avoid having to use `sudo` when executing Docker commands
```sh
sudo usermod -aG docker $USER
```

5. Restart the PC if still not working.

6. Check Docker version (client and server), as well as Docker info
```sh
docker version
docker info
```