# Installing Rancher 2.x Server

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

This will bring you to another screen with an area that has the command you need to run on the nodes. Make sure to check all of the server role boxes, and then copy what is in the box below it.  Paste that into the Rancher Agents' command prompts, hit enter, and watch as they attach to the server. Hit "next" on the server page, and congratulations, you have a cluster!

## Continue Reading...
1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup.md)
3. [Installing RancherOS](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server.md)
5. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher.md)
5. [Installing Longhorn](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn.md)
