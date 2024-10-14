
- `/etc/nginx`
	- the default configuration root for the Nginx server
	- files in here instruct Nginx how to behave
- `/etc/nginx/nginx/conf` 
	- the default configuration entry point used by the Nginx daemon
	- sets up global settings for things like worker processes, tuning, logging
	- the default configuration file includes the top-level `http` block, which includes all configuration files in the directory `/etc/nginx/conf.d`
- `/etc/nginx/conf.d` (sites enabled)
	- contains the default HTTP server configuration file
	- files in here that end in `.conf` are included in the top-level `http` block
- `/var/log/nginx`
	- default log location for Nginx
	- `access.log` - logs each request
	- `error.log` - logs errors and debug information

#### Controlling Nginx
The `nginx` command allows you to interact with the Nginx binary, list modules, test configurations, and send signals to the master and worker processes.

- `nginx -h` - shows the Nginx help menu
- `nginx -v` - shows the version
- `nginx -t` - tests the Nginx configuration
- `nginx -s <signal>`
	- `stop` - fast shutdown
	- `quit` - graceful shutdown
	- `reload` - reload the Nginx configuration
	- `reopen` - reopens the log files