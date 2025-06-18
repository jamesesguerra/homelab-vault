1. **Obtain an SSL / TLS certificate**
You can generate self-signed certificates for local / dev environments, but this isn't recommended in production. You want an official certificate authority (CA) signed SSL.

In this command, `nginx-selfsigned.key` will be the private key kept on the server to decrypt messages from the client. And the `nginx-selfsigned.crt` will be the public key that clients will use to encrypt the data.
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt
```

2. **Update the server directives**
```nginx
server {
	listen 443 ssl;
	server_name localhost;

	ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
	ssl_certificate_key /etc/ssl/certs/nginx-selfsigned.key;

	location / {
		proxy_pass http://backend;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
	}
}
```

3. **Create a server block to redirect HTTP requests to the HTTPS server**
```nginx
server {
	listen 80;
	server_name localhost;

	location / {
		return 301 https://$host$request_uri;
	}
}
```