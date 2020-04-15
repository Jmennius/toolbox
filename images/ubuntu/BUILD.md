## Using prebuilt images

Create a container with
```sh
toolbox create -i docker.io/jmennius/ubuntu-toolbox:${version}
```
and enter it with
```sh
toolbox enter -c ubuntu-toolbox-${version}
```

In case you have built your toolbox with this PR,
you can use `--distro` and `--release` options.


## Building and using Ubuntu images

You can build your own images by running
```sh
buildah build --pull -t ubuntu-toolbox:${version} -f images/ubuntu/${version}/Containerfile images/ubuntu
```
where `version` is the desired Ubuntu image version.

You can then create a new toolbox with
```sh
toolbox create -i localhost/ubuntu-toolbox:${version}
```
and enter it with
```sh
toolbox enter -c ubuntu-toolbox-${version}
```


## Refresh all images on Docker Hub

As of now images are hosted on Docker Hub.
All images share `extra-packages` and `README.md`.

```sh
# login into docker hub
podman login docker.io

versions=(16.04 18.04 20.04 21.10 22.04)

# build & push
for version in ${versions}; do
	buildah build --pull -t ubuntu-toolbox:${version} -f images/ubuntu/${version}/Containerfile images/ubuntu
	podman push localhost/ubuntu-toolbox:${version} docker.io/jmennius/ubuntu-toolbox:${version}
done

# push latest tag
podman push localhost/ubuntu-toolbox:${versions[-1]} docker.io/jmennius/ubuntu-toolbox:latest
```
