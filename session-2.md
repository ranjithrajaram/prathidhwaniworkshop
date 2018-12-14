# Session 2

## Skopeo

### Copying images from registry to another[ replace the IP address]

~~~
sudo skopeo copy docker://docker.io/busybox docker://192.168.122.72:5000/docker/busybox --dest-tls-verify=false
~~~

### Inspecting an image before pulling the image

~~~
# sudo skopeo inspect --tls-verify=false docker://registry.access.redhat.com/rhel7
~~~

## Buildah

## Example of using buildah to build container images using Dockerfile for compatibility reasons

~~~
From registry.centos.org/centos
RUN yum install httpd -y
CMD ["/usr/sbin/httpd","-DFOREGROUND"] 
~~~

### Builadh, first native method example

~~~
 #!/usr/bin/env bash

set -o errexit

# Create a container
container=$(sudo buildah from registry.centos.org/centos)

# Labels are part of the "buildah config" command
sudo buildah config --label maintainer="Me <me@gmail.com>" $container

# Install the httpd package

buildah run $container yum install httpd -y
sudo buildah run $container yum clean all

# Entrypoint, too, is a “buildah config” command
sudo buildah config --cmd "/usr/sbin/httpd -DFOREGROUND" $container

# Finally saves the running container to an image
sudo buildah commit --format docker $container hello5:latest
~~~

### Buildah, example of `buildah mount` example

~~~
#!/usr/bin/env bash
set -o errexit
# Create a container
container=$(sudo buildah from registry.centos.org/centos)
mountpoint=$(sudo buildah mount $container)
sudo buildah config --label maintainer="Chris Collins <collins.christopher@gmail.com>" $container
curl -sSL http://ftpmirror.gnu.org/hello/hello-2.10.tar.gz \

     -o /tmp/hello-2.10.tar.gz

tar xvzf src/hello-2.10.tar.gz -C ${mountpoint}/opt

pushd ${mountpoint}/opt/hello-2.10
./configure
make
make install DESTDIR=${mountpoint}
popd
chroot $mountpoint bash -c "/usr/local/bin/hello -v"
sudo buildah config --entrypoint "/usr/local/bin/hello" $container
sudo buildah commit --format docker $container workshop
sudo buildah unmount $container
~~~
