Add a server block
```conf
server {

}
```

To static web pages and images, create two folders `/data/www` and `/data/images`. You can then add `location` block directives to handle requests to fetch these documents.

For matching requests, the URI will be added to the path specified in the root directive, ie to `/data/www` and `/data` to form a path to the requested object on the file system.

```conf
server {
	location / {
		root /data/www;
	}

	location /images {
		root /data;
	}
}
```