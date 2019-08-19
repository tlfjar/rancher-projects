# rancher-projects
Configs for Rancher, RancherOS, K3OS, and other files necessary for setting up a HA Rancher/ELB setup.

NOTE: This is very much a work in progress, and I'd appreciate any help.  I'm learning this as I go, probably the same as most people that stumble on this REPO.

First, it may help to describe my setup:
  1.  Dell r210 ii, pfsense router with the following interfaces:
    a.  WAN, currently behind another router, but will have a static IP as soon as my ISP binds the r210ii's MAC to that static IP.
    b.  LAN, where most of the computers in the office connect for internet service.
    c.  DMZ, currently unused interface. Have plans to use it as a...well...DMZ, but haven't made that happen yet.
    d.  Swarm, Interface for everything else.  This is where the nodes, the Rancher server, and the external load balancer will be.
  2.  Dell r620, 16 physical cores (32 including vcores), 64 gb of RAM, VMware EXSi for machines
    a.  can quickly create and destroy machines.  This will be the base for all of my nodes, the load balancer, and the Rancher Server.  I use RancherOS primarily, as I find it relatively simple and it works nicely with my comfort level with docker.  See my cloud-config files for how I handle setup.
    
Currently, my setup is entirely internal, as I do not yet have direct control of my static IP.  This works out for testing purposes, though it is clearly not what this setup is designed for.  I expect that it will all transition to external DNS's in the future. In the mean time, I am stuck with Unbound on PFsense and internal certificates, which turns out to be a lot more frustrating than initially anticipated.

I will try to document as best as possible why any given file suddenly appears on this repo, either by comments in the file or by updating this readme.  Feel free to message me or bring up issues if I am screwing up anything.
