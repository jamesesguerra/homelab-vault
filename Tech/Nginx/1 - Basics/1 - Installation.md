1. Update package information for configured sources and install packages that will assist in configuring the official NGINX package repository
```sh
apt update
apt install -y curl gnupg2 ca-certificates lsb-release debian-archive-keyring
```

2. Download and save the Nginx signing key
```sh
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
```

3. Use `lsb_release` to set variables defining the OS and release names, then create an apt source file
```sh
OS=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
RELEASE=$(lsb_release -cs)
echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] http://nginx.org/packages/${OS} ${RELEASE} nginx" | tee /etc/apt/sources.list.d/nginx.list
```

4. Update package information again, and then install Nginx
```sh
apt update
apt install -y nginx
systemctl enable nginx # or
service nginx start
```

5. Verify the installation. You should see a version number, 1 master process, and several worker processes running.
```sh
nginx -v
ps -ef | grep nginx
```

6. Verify that Nginx is returning requests correctly.
```sh
curl localhost
```


