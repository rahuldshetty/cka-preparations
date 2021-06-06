Docker Storage System

Main: /var/lib/docker
>aufs
>containers
>image
>volumes

Docker layered architecture -> single instruction converted to layer

Two layers for docker image:
1. Read Only
2. Read Write (used during runtime and temporary)

Copy-on-write: changing contents from read-only layers will be creating a new copy in Read-Write layer.

Mounting:
1. Volume Mounting (mounted at var/lib/docker)
2. Bind Mounting (any folder)

Who controls all the management?
Storage drives
E.g: AUFS(ubuntu), ZFS, BTRFS, Device Mapper, Overlay, Overlay2

Depends on underlying OS

Volume Drivers:
Create volume. 
eg: local | azure | glusterFS | VMware vSphere Storage


Container Runtime interface:
Standard that defined how orchestration management interacts with container runtime like docker, rkt, cri-o.

Container Network Interface:
flannel

Container Storage Interface:
Wrtie own drivers to work with kubernetes.
E.g: DELL EMC, Gluster FS, Amazon EBS

Storage Drivers should implement the methods defined by CSI. 
e.g  RPC: CrateVolume, DeleteVolume, ControllerPublishVolume
