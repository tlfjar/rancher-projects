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

The r620 has an idsdm (basically a redundant SD card for booting certain hypervisor), on which I have installed EXSi 6.7.  This was a joe-blow install, using the free edition.  Make sure, after it is installed, to put in the free edition's license number so that you don't accidentally provision a vm that is too big for the free edition.  

I'd use proxmox, but my setup doesn't currently allow for it.  Proxmox will tear up an SD card, and the ones that I have are too small to run it anyway.  I could set up a drive just for Proxmox, but I haven't.  It is on my to-do list.

## Rancher Server

### Installing RancherOS

Now, to the meat of things.  I am running rancher on [RancherOS](https://github.com/rancher/os/releases) (Latest stable version, and the vmware-specific ISO), using [this cloud-config.yaml](RancherServer/cloud-config.yaml) as the configuration file.  You will want to change the network parts of that file and the ssh keys to fit your environment.  You can download it using this call:

`wget https://raw.githubusercontent.com/tlfjar/rancher-projects/master/RancherServer/cloud-config.yaml`

Edit the file using the following:

`vi cloud-config.yaml` and then press `i` to edit ("insert," technically).  When you're done, press the "escape" key and then type `:wq`

The installation call is as follows:

`sudo ros install -c cloud-config.yaml -d /dev/sda`

If you have your file saved elsewhere, you can replace the `cloud-config.yaml` portion above with the link.  If you are using a private Github repository, I recommend that you download the file using the `wget` command first, as it seems that the token changes pretty quickly.

You can now ssh into your server (after configuring the IP you set as static) using your ssh method of choice.  I personally use putty, because Windows.

### Post-Install Configuration

You may have noticed that I added an open-iscsi install into the cloud-config.yaml file above.  To make that work, we need to make one change to open-iscsi's configuration file.  Do the following:

`sudo vi /etc/iscsi/iscsid.conf` Once again press `i`, comment `iscsid.startup = /bin/systemctl start iscsid.socket`, and uncomment `iscsid.startup = /sbin/iscsid`.  Press the "Escape" key and then type `:wq` to exit.

NOTE: that was probably totally unnecessary, but it is good practice for doing it three more times shortly, and it doesn't hurt anything.

### Installing Rancher 2.x Server

The easiest way to do this is with Rancher's built-in certificate.  To do this, type the following:

```
docker run -d --restart=unless-stopped \
-p 80:80 -p 443:443 \
rancher/rancher:latest
```

For testing purposes, I use an internally generated, self-signed certificate.  PFSense has this ability built-in.  This is a good idea because if you are like me, you'll probably want to totally destroy your install every time it screws up, and that will break the link to the server every single time if you installed the server's cert in windows. 

To do this, we need to set up three files: `cert.pem`, `key.pem`, and `cacert.pem`.  The simplest method is to take the certificates/keys generated from PFSense (beyond scope of this document, but PFSense is extremely well-documented [here](https://docs.netgate.com/pfsense/en/latest/)), open them with [Notepad++](https://notepad-plus-plus.org/), copy them, and then create the files on a private repository.  If you do that, just download each file **INTO /etc/**. This will require `sudo`, but will save you the hastle of having to figure out what the full path to your file is.  Here is a generic call:

```
wget https://INSERT_LINK_HERE -o /etc/cert.pem
wget https://INSERT_LINK_HERE -o /etc/key.pem
wget https://INSERT_LINK_HERE -o /etc/cacerts.pem
```
After this, run the following command:

```
docker run -d --restart=unless-stopped \
	-p 80:80 -p 443:443 \
	-v /<CERT_DIRECTORY>/<FULL_CHAIN.pem>:/etc/rancher/ssl/cert.pem \
	-v /<CERT_DIRECTORY>/<PRIVATE_KEY.pem>:/etc/rancher/ssl/key.pem \
	-v /<CERT_DIRECTORY>/<CA_CERTS.pem>:/etc/rancher/ssl/cacerts.pem \
	rancher/rancher:latest
```

Give it a bit, and you should be able to connect to rancher using its IP address or the FQDN and hostname you set up in PFSense.  Log in to it, follow the prompts, and then do the following:
1. Click "Clusters" on the menu bar;
2. Click "Add Cluster";
3. Select "Custom";
4. Name it whatever;
5. Select "none" for cloud provider;
6. **Change the Network Provider to Weave**
7. Leave everything else alone; and
8. Click "Next."

This will bring you to another screen with an area that has the command you need to run on the nodes. Make sure to check all of the server role boxes, and then **STOP AND DO THE STEPS BELOW**

## Rancher Agents

### Installing RancherOS

Repeat the steps above, just use these three cloud-config.yaml files, edited with your network configuration:

[cloud-configA1.yaml](rancher-agents/cloud-configA1.yaml)
[cloud-configA2.yaml](rancher-agents/cloud-configA2.yaml)
[cloud-configA3.yaml](rancher-agents/cloud-configA3.yaml)

### Post-Install Configuration

Once again, we need to modify the open-iscsi files, as follows:

`sudo vi /etc/iscsi/iscsid.conf` Once again press `i`, comment `iscsid.startup = /bin/systemctl start iscsid.socket`, and uncomment `iscsid.startup = /sbin/iscsid`.  Press the "Escape" key and then type `:wq` to exit.

### Installing Rancher Agent

Remember how I told you to stop above?  Go back to that window, copy the call that Rancher gives you, and run it on all three agents.  Wait until the Rancher server window shows that three agents (or nodes, I can't remember exactly what it says) have registered, and then click "Next" or "Finish."  After a small eternity, you will have yourself a functioning cluster.  Congratulations!

## How to make this more efficient

There are a few things I have been messing with in an attempt to make this more.  First, and most obvious, is using the `sed` command to automatically edit the config file for open-iscsi.  I cannot get it to work.

Second, I would like to simply add the rancher agent install into the agent cloud-configs.  This works fine, but I have to get the `sed` command mentioned above to work or all sorts of screwy things happen.  Doing this also necessitates making that initial setup in Rancher to get the call in the first place.  An alternative to this would be to set up the cluster in K3OS, and then manage it with Rancher.  I have done this, but it is way outside of the scope of this project...for now.  My main apprehension is that I don't know how to use weave instead of the default in K3OS, which is profoundly important because weave just works in my current setup.  I don't need an external load balancer, a separate DNS to resolve everything, or any of that stuff.  I don't know why, and I probably should, but Weave works for ingresses, and ingresses are the only way that I use to connect to the services in the cluster.

Third, I would like to do my server install directly in the cloud-config file.  Because I am using self-signed certs, I cannot do that right now, unfortunately.  I suppose it is possible to write the .pem files directly using the `write_files` command, but I'm simply not secure enough in my abilities to make that function correctly.  If I get it working through experimentation, I will update here, and it may all end up being redundant when my ISP finally gives me controll of my static IP.

Fourth, I'd love to figure out how to fold this all into the RancherOS ISO, and totally automate the install.  I do not yet know how to pull that off, so I'm putting this into my really, really long-term goals.

# Now for the fun(?) part. Using Rancher 2.x

After you've installed the agents and your cluster has done all of the booting, it's time to install some stuff.  I've already mentioned [Longhorn](https://github.com/longhorn/longhorn), so lets go ahead and get that set up.  


First, do not bother with Nextcloud (or Owncloud).  Yes, they have their own helm charts, freely available in Rancher's App section. But they are kwirky and don't seem to like how ingresses work. I have tried just bypassing the helm chart and using the docker image directly, including one that should work better with the ingress, which is ostensibly also a web server.  No dice there either.

The solution was [Seafile](https://seafile.com).  It does not have a helm chart, but it is simple enough to use as a regular deployment in Rancher.  For convenience, and to avoid some hair-pulling, I went ahead and included 
