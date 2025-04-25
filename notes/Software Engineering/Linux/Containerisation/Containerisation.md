# Containerisation

Containers are environments in a Linux operating systems which are scoped using `Linux namespaces`. This means that resources cannot be shared outside of a container.

<!-- Look into Linux namespaces more. -->

## Docker

Docker is basically a front-end for `containerd` which creates and manages the running of `containers`.

## containerd

This is a program written primarily in `go` for managing multiple containers.

## runc

Containerd calls `runc`, another go program which actually creates and destroys `containers`.

## OCI Open Container Initiative

This is the standards to which containers are built by.
