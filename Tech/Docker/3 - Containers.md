### Important Commands
- `run`
	- `-d`, `--detach` - don't bind the output of the container to the current terminal
	- `-p`, `--publish` - publiish a port on the host and route traffic from the port to a port on the container
	- `-t`, `--tty` - simulate a real terminal (similar to what SSH does)
	- `-i`, `--interactive` - keep the terminal alive in the active session
- `logs` - show the logs of a container
- `rm` - remove a container
	- `-f`, `--force` - force the deletion of a container even if it's running
- `top` - show the running processes of a container 
- `inspect` - check the configuration of a container
- `stats` - display a live stream of stats of a running container (CPU, memory)
- `exec` - run additional command in an already running container

#### What happens when you run a container?
1. Docker checks for the image in the local cache and uses it if available
2. If not, it looks for that image in a remote repository (Docker Hub by default)
3. It checks out the latest version of that image (if you don't specify a tag) and pulls it
4. It creates a new container based on that image and prepares to start
5. Gives it a virtual IP on a private network in docker engine
6. Starts it using a command specified in the Dockerfile

#### Are containers like VMs?
No, containers are really just restricted processes running on the OS kernel. This is why the executable files in those containers have to be built for the kernel they're running on. So when you think of images, think of them as always kernel (Linux / Windows) and architecture (amd64, arm64) specific.


#### What happens if you run the container with a custom command?
You can start a container with an optional command of your choosing:
```sh
docker run -it --name proxy nginx bash
```

But once you exit the bash shell, the container stops. This is because the container only runs for as long as the command that started that container runs.