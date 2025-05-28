#### Ways to think about the different Dockerfile statements
- **overwrite vs additive**
	- some statements overwrite their previous use but some don't
		- overwrite:
			- `CMD`, `WORKDIR`, `ENTRYPOINT`
		- additive:
			- `EXPOSE`
- **buildtime vs runtime**
	- does the statement change the *image* or the *container*
	- runtime:
		- `CMD` - Docker stores the command here to execute when the container starts up
	- buildtime
		- `RUN`, `COPY`
	- both:
		- `ENV` - environment variables are accessible in the image and in the container

#### Main difference between CMD and ENTRYPOINT
`CMD` can be overriden by specifying an optional command at the end of `docker run` while overriding the `ENTRYPOINT` requires you to pass an argument called `--entrypoint`

**CMD** - `docker run ubuntu date` (override with the `date` command)
**ENTRYPOINT** - `docker run --entrypoint date ubuntu`

By default, `CMD` is used. But `ENTRYPOINT` is meant to complement `CMD`.

#### Use cases of ENTRYPOINT + CMD
If you specify both the `CMD` and `ENTRYPOINT` statements, Docker combines the two in the format: `ENTRYPOINT [space] CMD`.

This isuseful for 2 reasons:
1. **CLI tools**
When creating Docker containers in which you want to replicate CLI tools as close as possible, you want to make it so that you only need to supply the arguments. But when you use `CMD` like `CMD ["curl"]`, you'd have to run the docker container like this to curl a website
```sh
docker run --name cmd_curl curl curl google.com
```
But when using `ENTRYPOINT` with `CMD` like this
```Dockerfile
ENTRYPOINT ["curl"]

CMD ["--help"]
```
you get to just supply the arguments and run a more concise command
```sh
docker run --name entrypoint_curl curl google.com
```

2. **For container startup scripts**
If you wanna run a startup script before starting your app, you might consider adding the run command of your app inside of that script and running that script in the `CMD` statement. But this has negative effects as your app would be a **child process** of the shell process that started it, essentially making it so that **Linux shutdown signals** might not be communicated properly to your app to shut down gracefully.

To solve this, run the script in the `ENTRYPOINT` command, then pass the execution to the command that will start your app binary.
```dockerfile
ENTRYPOINT ["./startup.sh"]
CMD ["python", "app.py"]
```
To pass the execution to the command, add this at the end of the script:
```sh
exec "$@"
```

#### Exec vs Shell form
The 3 Dockerfile statements that can run a program are:
- `RUN` - shell
- `ENTRYPOINT` - exec
- `CMD` - exec

When running commands / programs, you can either use exec form or shell form

**exec form**
- Docker run programs directly
- command is in JSON array syntax
- used with `ENTRYPOINT` and `CMD` by default unless you need shell features with `CMD`

**shell form**
- Docker runs the commands in a shell, essentially adding shell features to your commands (I/O redirection, command chaining etc.)
- command is just a string
- used with `RUN` by default
