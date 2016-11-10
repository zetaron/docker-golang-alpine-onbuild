# Supported tags and respective `Dockerfile` links

- [`1.6.2-alpine-onbuild` (*Dockerfile*)](https://github.com/zetaron/docker-golang-alpine-onbuild/blob/1.6.2/Dockerfile)
- [`1.7.3-alpine-onbuild` (*Dockerfile*)](https://github.com/zetaron/docker-golang-alpine-onbuild/blob/1.7.3/Dockerfile)

*Note*: For more versions of this image please check the base images store page [`golang`](https://store.docker.com/images/3e4f3e51-3930-4dd8-975c-517705d9d4e7)

# How to use this image

## Start a Go instance in your app

The most straightforward way to use this image is to use a Go container as both the build and runtime environment. In your `Dockerfile`, writing something along the lines of the following will compile and run your project:

```dockerfile
FROM golang:1.6.2-alpine-onbuild
```

This image includes multiple `ONBUILD` triggers which should cover most applications. The build will `COPY . /go/src/app`, `RUN go get -d -v`, and `RUN go install -v`.

This image also includes the `CMD ["app"]` instruction which is the default command when running the image without arguments.

You can then build and run the Docker image:

```console
$ docker build -t my-golang-app .
$ docker run -it --rm --name my-running-app my-golang-app
```

*Note:* the default command in `golang:onbuild` is actually `go-wrapper run`, which includes `set -x` so the binary name is printed to stderr on application startup. If this behavior is undesirable, then adding `CMD ["app"]` (or `CMD ["myapp"]` if a [Go custom import path](https://golang.org/s/go14customimport) is in use) will silence it by running the built binary directly.

## Compile your app inside the Docker container

There may be occasions where it is not appropriate to run your app inside a container. To compile, but not run your app inside the Docker instance, you can write something like:

```console
$ docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.6.2-alpine go build -v
```

This will add your current directory as a volume to the container, set the working directory to the volume, and run the command `go build` which will tell go to compile the project in the working directory and output the executable to `myapp`. Alternatively, if you have a `Makefile`, you can run the `make` command inside your container.

```console
$ docker run --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp golang:1.6.2-alpine ash -c make
```

# Credit where credit is due!
This image is based on the official golang:XXX-alpine image and only adds the ONBUILD functionality, which in itself has been taken from the official golang:XXX-onbuild image.
This README has also been copy pasted and was just slightly modified, mainly to contain the credit section :P and removing the sections not related to onbuild.
