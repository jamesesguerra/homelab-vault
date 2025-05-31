**Docker compose** is for establishing and configuring relationships between containers, making it easy to save `docker run` settings in a file. It isn't a production-grade tool, but it's very ideal for local testing and development.

#### YAML Configuration File
```yaml
# same as
# docker run --name jekyll -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve

services:
	jekyll:
		image: bretfisher/jekyll-serve
		volumes:
		- .:/site
		ports:
		- '80:4000'
```

#### Most common commands
- `docker compose up` - create networks, volumes, and start containers
- `docker compose down` - tear down networks, volumes, and stop containers 

#### Named volumes
To specify named volumes in a docker compose file (so that data persists across compose restarts), specify them like this:
```yaml
services:
	drupal:
		volumes:
		- drupal-modules:/var/www/html/modules

volumes:
	drupal-modules:
```
To remove the volumes after the compose project has stopped:
```sh
docker compose down -v
```

#### Image building in Docker Compose
You can also run containers from images that haven't been built yet:
```yaml
service:
	proxy:
		build:
			context: . # this directory
			dockerfile: nginx.Dockerfile # the name of the dockerfile to use
		image: nginx-custom # the name of the image to use
```
Docker won't build the image every time you run `docker compose up`. If the image is already in the cache, it uses that instead. To build the image again while running `docker compose up`:
```sh
docker compose up --build
```
To remove images that the Docker compose built:
```sh
docker compose down --rmi local
```