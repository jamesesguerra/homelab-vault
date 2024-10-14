A proxy server is a server that makes requests on behalf of a client. It's a server that receives requests, passes them to the proxied servers, retrieves responses from them, and sends them to clients.

Define the configuration for the proxied server by adding one or more server blocks. When you indicate that a server listen on a certain port, Nginx uses one of its worker processes to listen for requests on that port. In this case, the proxied server listens on port 8080 and maps all requests to the `/data/up1` directory.
```conf
server {
	listen 8080;
	root /data/up1;

	location / {
	}
}
```

Next, create the proxy server by using the `proxy_pass` directive to forward the request to the proxied server.
```conf
server {
	location / {
		proxy_pass http://localhost:8080;
	}

	location ~ \.(gif|jpg|png)$ {
		root /data/images;
	}
}
```

