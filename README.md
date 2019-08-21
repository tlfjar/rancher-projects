# Rancher 2.x for Lawyers
Running Rancher on RancherOS in a remarkably silly environment.

NOTE: This is very much a work in progress, and I'd appreciate any help.  I'm learning this as I go, probably the same as most people that stumble on this REPO.

# Why not just run docker containers and drop the annoyance of kubernetes?

Scalability.  Servers are surprisingly cheap, and the electricity costs in our city are extremely low.  Being able to add additional servers as needed or as whim may direct is a plus.  I am also fond of the way that Rancher handles containers and everything around them.

Survivability is also a benefit.  Things screw up, and it would seem to me that the redundancy afforded by Kubernetes at least might help with that.

Finally, it's kind of enjoyable.  I doubt that I will put anything into production, and I'm not so sure how bright of an idea it is doing that on bare metal.  But the act of at least trying to understand this has been enlightening, if not entirely helpful.

# Goals and Limitations
I will try to document as best as possible why any given file suddenly appears on this repo, either by comments in the file or by updating this readme.  Feel free to message me or bring up issues if I am screwing up anything.

The files will be in accordance with whatever version of an install I am doing at the time.  For example, "rancherserver" is for a regular rancher server, with a self-generated certificate.  I will anonymize other versions a bit more.

As I am not particularly skilled at coding, do not expect much in the way of runfiles and whatnot.  I will do what I can when I figure out how to do it, but I will also do my best to at least put SOMETHING in here to say the steps I took at the command line.

# Office Setup
First, it may help to describe my setup:
  - Dell r210 ii, pfsense router with the following interfaces:
    - WAN, currently behind another router, but will have a static IP as soon as my ISP binds the r210ii's MAC to that static IP.
    - LAN, where most of the computers in the office connect for internet service.
    - DMZ, currently unused interface. Have plans to use it as a...well...DMZ, but haven't made that happen yet.
    - Swarm, Interface for everything else.  This is where the nodes, the Rancher server, and the external load balancer will be.
  
  - Dell r620, 16 physical cores (32 including vcores), 64 gb of RAM, VMware EXSi 6.7 for machines
    - can quickly create and destroy machines.  
    - This will be the base for all of my nodes and the Rancher Server.  
    - I use RancherOS primarily, as I find it relatively simple and it works nicely with my comfort level with docker.  
    - See my cloud-config files for how I handle setup.
    
Currently, my setup is entirely internal, as I do not yet have direct control of my static IP.  This works out for testing purposes, though it is clearly not what this setup is designed for.  I expect that it will all transition to external DNS's in the future. In the mean time, I am stuck with Unbound on PFsense and internal certificates, which turns out to be a lot more frustrating than initially anticipated.

# Software Setup

To be continued...
