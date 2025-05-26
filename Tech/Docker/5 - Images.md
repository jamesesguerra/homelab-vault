### What's in an image?
- app binaries and dependencies
- image metadata and information on how to run it

#### Image Layers
- images are constructed using a union filesystem wherein they're built up with subsequent layers
- every image starts off with a blank slate, called "scratch", and all sets of changes that happen on top of that on the file system in the image is another layer
- every layer gets a unique SHA that helps the system identify if that layer is indeed the same as another layer
- image layers are cached, and new images can reuse the image layers that have already been cached to save time and space
- if image layers are the same, they're never stored more than once
- when you run a container, Docker creates a new writable layer (often called the **container layer**) on top of the read-only image layers, and so the running container would be a union of that image layer and the read-only image layers beneath

![[image-cache.png]]

### Image Tagging
- images are identified using 3 parts: `<username>/<repository>:<tag>`
- tags are like Git tags in that they're a pointer to a specific image ID, or version of an image when its built

### Dockerfile components
- `FROM`
	- all images must have this stanza
	- usually from a lightweight Linux distribution
	- main reason why you need it is to provide a userland (filesystem, OS utilities, and package managers) for your app, but containers always use the host kernel
	- you can start from scratch with `FROM scratch`, in which case the container would be very lightweight
- `ENV`
	- a way to set environment variables
- `RUN`
	- optional stanza to do app-specific activities like installing software, unzipping, inside the container at build time
- `EXPOSE`
	- expose TCP / UDP ports to the Docker virtual network
- `CMD`
	- the command that will be run every time you start / restart a container from the image
- `WORKDIR`
	- change directory
- `COPY`
	- copy files on the host into the container

### Useful commands
- `history <image>` - view the image layers of a certain image 
- `image inspect <image>` - view image metadata
- `tag <image> <new-tag>` - tag images with new tags'


### Best Practices
- each layer is based on the result of the one before it, therefore it would be useful to keep the stanzas that change the least in earlier in the Dockerfile, and the ones that change often later as changing an instruction would invalidate the layers that come after it

