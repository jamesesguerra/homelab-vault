Add a server block. A **server block** defines a new context that specifies what NGINX will listen for.
- the `listen` directive instructs NGINX to listen on port 80
- the `default_server` directive tells NGINX to use this server as the default for requests that are sent to port 80, even if the `server_name` doesn't match the HTTP host header
- the `server_name` value defines that the requests only with that matching host name will be routed to this server block
```nginx
server {
	listen  80   default_server;
	server_name  www.example.com;
}
```

To static web pages and images, create two folders `/data/www` and `/data/images`. You can then add `location` block directives to handle requests to fetch these documents.

For matching requests, the URI will be added to the path specified in the root directive, ie to `/data/www` and `/data` to form a path to the requested object on the file system.

```nginx
server {
	location / {
		root /data/www;
	}

	location /images {
		root /data;
	}
}
```