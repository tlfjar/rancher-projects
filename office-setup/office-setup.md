# Office Setup
This is my setup.  Kubernetes is quite flexible, so I do not expect you to have the same setup.  I became enamored early on at the low cost of used servers, and probably overbought.  For what its worth, what you see here maybe cost me a thousand dollars to put together, counting random used cards, 10k hard drives, SSD's, those caddies to put the hard drives in, a pile of cat 6 cable, and the stuff to terminate them.  Check out [Lab Gopher](https://www.labgopher.com/) if you're in the market for some hot bare metal at a steep discount. That's what I did.  Lowe's and random [ebay](https://www.ebay.com/) searches filled in the rest

## Hardware
  - Dell r210 ii, running as a [PFSense](https://github.com/pfsense/pfsense) router with the following interfaces:
    - WAN, has static IP, hooked to a domain that I own, using cloudflare to handle name services.  Also using the ACME package from PFSense to get a valid cert to put on the load balancer.
    - LAN, where everything else is.  I should isolate my kubernetes nodes on a separate interface but...I don't.
    - To make [Rancher](https://github.com/rancher/rancher) function at its best, I run HAProxy as an external load balancer using the package provided in PFSense.  Explaining this is on my list of things to add to this repository, but that may be a while.  It took me long enough to make it work that retreading the steps will be time consuming.  For my system, SSL is terminated at this load balancer.
    
  - Two (2) Dell r620's, 16 physical cores (32 including vcores), 64 gb of RAM, VMware EXSi 6.7 for machines
    - can quickly create and destroy machines.
    - One of my machines has nothing but SSD's, the other nothing but spinning disks.  Specs are basically the same otherwise.  Network interfaces are combined when possible to increase throughput.
    - ESXI is beyond the scope of this repo, but it really isn't difficult to run.

This is what the actual machines look like in my office:

![ServerSideView](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/Server%20Setup.jpg?raw=true)

![ServerTopView](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/Top%20Server%20Setup.jpg?raw=true)


## Virtual Machines
 1. Rancher Server, where the server portion of Rancher runs.  There's only one of these, though it would be better to run multiple copies for hardiness.
 
 ![RancherServer](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/RancherServer.png?raw=true)
 
 2. Three (3) Rancher Etcd/Controlplane systems.  Running more than one is a good thing, and using them for solely the purpose of Etcd and Control Plane is evidently also good.  It would be best if I dedicated multiple nodes to just Control Plane and multiple nodes to just Etcd, but it was enough to get the cluster to build with the system I created.
 
 ![EtcdcpNodes](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/Rancheretcdcp.png?raw=true)
 
 3. Rancher worker, with SSD storage, found on the same server as the Rancher Server, Rancher Etcd/Controlplane systems.  This gives me at least one speedy(er) node.
 
 ![RancherW1](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/Rancherw1.png?raw=true)
 
 4. Three (3) Rancher workers with spinning disks.  These are on an entirely different physical server from the above.
 
 ![RancherW2](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/Rancherw2.png?raw=true)
 
 5. Also, I run a file server with Seafile installed on it.  This is not necessary, but I'm no longer that interested in running large file storage within a cluster.
 
 ![fileserver](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/Images/fileserver.png?raw=true)

Please note that my setup is not, by any stretch of the imagination, the best.  I'm not even sure if it is "good," but it works for me.  The tutorials found in this repo do not expect you to go this far.  Indeed, if this is your first time setting up a cluster using Rancher, it would be best not to do this at all, and just make three nodes that handle all three assignments (Etcd, Controlplane, and Worker).

## Continue Reading...
1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
3. [Installing RancherOS](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS/Install-RancherOS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
5. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
6. [Installing Longhorn](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn/Installing-Longhorn.md)
