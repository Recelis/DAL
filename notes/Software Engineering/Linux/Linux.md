# Linux

## Linux Kernel

## Linux Namespaces

[docs](https://www.howtogeek.com/devops/what-are-linux-namespaces-and-what-are-they-used-for/)

This is a feature of the `Linux kernel` that allows `Containerisation` to occur by isolating resources

There are 7 global namespaces that come with Ubuntu.

- Mount `mnt`
  Virtually partitions the FileSystem.

- Process `pid`
  The process namespace cuts off a branch from the `process tree`. Processes spawn as children of PID1 so a process namespace does not allow you to view up the tree.

- Interprocess Communication `ipc`
  This namespace allows processes to talk to each other or not.

- Network `net`
  This manages whether or not your process can have access to network devices. But you still have to create your own `virtual network device` and connect it to the `global network device`.

- User
  Allows the user to have `root` access within their own namespace without having it for the parent system.

- UTS
  Controls hostname and domain info.

- CGroup
  Similar to `namespaces`, this allows you to create resource limits like CPU, memory, disk space, network traffic to a group of processes.

## Viewing Namespaces

Use the `lsns` command with sudo. Anything with PID1 is part of the global 7 namespaces.

## Creating new namespaces

Use the `unshare` command.

To create custom namespaces, you'll have to fork your own based on one of the global 7.

<!-- Try doing this in Ubuntu creating and viewing new namespaces. Giving yourself full fs -->
