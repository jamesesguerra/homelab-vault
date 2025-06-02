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

#### A complete solution for dev, test, prod using Compose
You can construct a docker compose solution that will handle starting containers according to the environment.

First, you're going to create a "base" docker compose solution that will serve as the basis for all other compose files. This file has all the stuff they have in common.
```yaml
version: '3.9'

services:
	drupal:
		image: custom-drupal:latest
	
	postgres:
		image: postgres:14
```

For dev environment, you're going to create a `docker-compose.override.yml` file (filename is important) to "overlay" on new configuration on top of the base compose file.
```yaml
version: '3.9'

services:
	drupal:
		build: .
		ports:
			- "8080:80"
		volumes:
			- drupal-modules:/var/www/html/modules
			- drupal-profiles:/var/www/html/profiles
			- drupal-sites:/var/www/html/sites
			- ./themes:/var/www/html/themes

	postgres:
		environment:
			- POSTGRES_PASSWORD_FILE=/run/secrets/psql-pw
		secrets:
			- psql-pw
		volumes:
			- drupal-data:/var/lib/postgresql/data

volumes:
	drupal-data:
	drupal-modules:
	drupal-profiles:
	drupal-sites:
	drupal-themes:

secrets:
	psql-pw:
	file: psql-fake-password.txt
```

Then, in your CI solution (for testing the code with Jenkins, Github Actions etc.), you would have a separate `docker-compose.test.yml` file, and compose up with `-f`, making sure to specify the path to the base compose file first.
```sh
docker compose -f docker-compose.yml -f docker-compose.test.yml up -d
```

Then, in production, because you wouldn't have docker compose there and would have Docker Swarm instead, you would output the configuration of the combination of the base and your prod file to an output file for Swarm Stack to deploy.
```sh
docker compose -f docker-compose.yml -f docker-compose.prod.yml config > output.yml
docker stack deploy -c output.yml my-stack
```

