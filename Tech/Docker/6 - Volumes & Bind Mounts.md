### Why volumes & bind mounts?
- containers are immutable and ephemeral, their design system is meant for redeploying changes instead of changing directly
- unique data generated in the container will get lost if the container is removed
- the solutions allow you to persist data so that a new container can read the data generated before
- 2 ways:
	- **volumes** - perists data outside of the container
	- **bind mount** - mounting a directory on the host into the container, allowing them to be in sync

#### Volumes
- specify the `VOLUME` stanza in the Dockerfile
- this is saying, create a new volume on the host and mount it to that directory on the container, so that when the container writes to that directory, it's essentially writing to the volume
- when using existing images, check Docker hub documentation to see where the image expects the volume to be (what directory is specified in the `VOLUME` stanza in the Dockerfile)

#### Bind Mounts
- basically two locations (one on the host, another on the container) pointing to the same file(s)
- can't be specified in the Dockerfile, just on `docker run`
- good for developer experience but shouldn't be used in production

### Useful commands
- `volume ls` - list all volumes
- `inspect <container>` - navigate to the Volumes part to see which volume it's using by configuration in the Dockerfile, or the Mounts section to see what volume is actually assigned
- `run -v name:<container-dir>` - specify a named volume when running a container 
- `run -v /path/to/dir/or/file:<container-dir>` - specify a directory / file when using bind mounts
