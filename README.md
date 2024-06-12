# Paketo Jammy Base Builder

## `paketobuildpacks/builder-jammy-base`

This builder uses the [Paketo Jammy Base
Stack](https://github.com/paketo-buildpacks/jammy-base-stack) (Ubuntu Jammy
Jellyfish build and run images) with buildpacks for Java,
Java Native Image, Go, Python, .NET, Node.js, Apache HTTPD, NGINX and Procfile.

To see which versions of build and run images, buildpacks, and the lifecycle
that are contained within a given builder version, see the
[Releases](https://github.com/paketo-buildpacks/builder-jammy-base/releases) on this
repository. This information is also available in the `builder.toml`.

For more information about this builder and how to use it, visit the [Paketo
builder documentation](https://paketo.io/docs/builders/).  To learn about the
stack included in this builder, visit the [Paketo stack
documentation](https://paketo.io/docs/stacks/) and the [Paketo Jammy Base Stack
repo](https://github.com/paketo-buildpacks/jammy-base-stack).

## To build multi platform support ("linux/amd64", "linux/arm64",...) on a Mac in Docker desktop.


To build with support for multi architecture edit `builder.toml` to point to `multiplatform` jammy-base-stack images (refer to jammy-base-stack project to build if necessary) by editing the [stack] section appropriately:
```
[stack]
  build-image = "<REGISTRY>/.../build-jammy-base:jammy"
  run-image = "<REGISTRY>/../run-jammy-base:jammy"
```
Enable Experimantal features in Docker desktop:
- from the dashboard -> click on Settings -> select features in Development from left menu -> select Experimantal features tab -> check the Access experimental features box

Change `<REGISTRY>` with your registry host and run:
```
pack builder create "<REGISTRY>/amd64/paketo/builder-jammy-base:jammy" --config "./builder.toml" --target "linux/amd64"      
docker push  "<REGISTRY>/amd64/paketo/builder-jammy-base:jammy"  

pack builder create "<REGISTRY>/arm64/paketo/builder-jammy-base:jammy" --config "./builder.toml" --target "linux/arm64"   

docker push  "<REGISTRY>/arm64/paketo/builder-jammy-base:jammy"   

docker manifest create <REGISTRY>/tanzubuild/paketo/builder-jammy-base:jammy \
        --amend <REGISTRY>/arm64/paketo/builder-jammy-base:jammy \
        --amend <REGISTRY>/amd64/paketo/builder-jammy-base:jammy   

docker manifest push <REGISTRY>/tanzubuild/paketo/builder-jammy-base:jammy
```
Now you can use the multiplatform builder `<REGISTRY>/tanzubuild/paketo/builder-jammy-base:jammy`
    
