
## NFS Storage class

I don't want to install portworx, because it needs at least 4 cores and 4GB of memory when my server only has 4 cores (8 threads) and 32GB of RAM.  
So we'll use NFS for obvious reasons.

There is an NFS provisioner recommended by the Kubernetes documentation : [NFS Subdir External Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner)

It will create persistent volumes automatically and that's all I need.