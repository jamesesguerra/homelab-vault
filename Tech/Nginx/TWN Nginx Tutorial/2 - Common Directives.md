
**worker processes** defines how many worker processes Nginx will spawn to handle the connections. This typically should be equal to the number of cores the CPU has, and you can set it to `auto` to let Nginx take care of how many it thinks it should add
```nginx
worker_processes 1;
```

**worker_connections** determines how many simultaneous connections can be opened per worker process
```nginx
events {
	worker_connections 1024;
}
```

the **http** directive allows you to specify how you want to handle HTTP requests with Nginx
```nginx
http {
	server {
		listen 8080;
		server_name localhost

		location / {
			proxy_pass 
		}
	}
}
```
