# Installing K3OS (NEW)

[K3OS](https://github.com/rancher/k3os) is an incredibly lightweight distro with kubernetes baked right into it. This allows you to skip several steps in a normal high availability install of Rancher, though there are a few quirks not present when installing on RancherOS...

## Create a Datastore

The very first thing to do is create a datastore on another computer, VM, or the convenient [Amazon RDS](https://aws.amazon.com/rds/), an option for which the team at Rancher [wrote a handy guide](https://rancher.com/docs/rancher/v2.5/en/installation/resources/k8s-tutorials/infrastructure-tutorials/rds/).

If you're like me and want to do things the hard way, here's what you need to do:

### Option 1: MySQL with at least two nodes.

For a time, this was the only effective option for HA on K3OS.  Fortunately, native ETCD support was added in k3s version v1.19.5+k3s1, and the most recent version of K3OS uses v1.21.5+k3s2.  It is the better option, PROVIDED that you don't mind running three or more nodes.  If that isn't an option, or if you just really, really hate time and want to kill as much of it as possible, then continue reading...

1. [Install MySQL](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/linux-installation.html);
2. If you have to, [initialize the data directory.](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/data-directory-initialization.html) This isn't necessary if you use most native packaging options built in to normal Linux distros.
3. [Start the server](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/starting-server.html) and [secure the admin user in mysql](https://dev.mysql.com/doc/mysql-installation-excerpt/5.7/en/default-privileges.html).
4. Get the mysql client tool running by typing `mysql -u root -p`. This may be different, depending on the options chosen in Step 3.
5. Type `CREATE DATABASE rancher;`. It'll return something that hopefully is not an error.
6. Type `CREATE USER 'rancher'@'%' IDENTIFIED BY 'password'`. Ideally, you'll want to set up two users, each with the very specific IP address that your servers will be using instead of '%'. If it is an internal database, thoroughly secured from outside connections, then this isn't necessary, but do it anyway. The habit is good to have. Oh, and use a password that will be easy to include in a connection string.
7. Type `GRANT ALL PRIVILEGES ON rancher.* TO 'rancher'@'%';`. Your version of mysql may or may not require the word "PRIVILEGES." If that goes through alright, go ahead and type `FLUSH PRIVILEGES`. You can now get out of mysql.
8. Make your database reachable from the outside. Easiest way to do that is to simply [follow this guide.](https://phoenixnap.com/kb/mysql-remote-connection) I'd type it out but there are too many permutations that depend on the type of firewall present in your distro. This is where you are most likely to hit a snag of some sort.  If you get an 8080 error from your K3OS nodes after they've been fired up, assume there's an authentication issue between your nodes and MySQL. I promise that knowing this will save you a lot of fruitless web searches down the line.

### Option 2: ETCD with at least three nodes (NEW).

As you read above, this is the easier option, so much so that all you need to do is install K3OS as described below, only use [this config file](https://github.com/tlfjar/rancher-projects/blob/master/Install-K3OS/configetcd1.yml) instead of the one listed below for the first server.  Get it spun up, and then join two other servers to it using [this config file](https://github.com/tlfjar/rancher-projects/blob/master/Install-K3OS/configetcd2.yml). Otherwise, do everything described below.

## Install K3OS

In this setup, you're going to want at least two servers. These can be VM's or two separate boxes entirely. Either way, grab the [latest release of K3OS](https://github.com/rancher/k3os/releases) and pop it into the system. After it does its thing for a moment, go ahead and log in as "rancher." After a second, you'll be greeted with a prompt, where you will want to get [this file](https://github.com/tlfjar/rancher-projects/blob/master/Install-K3OS/config.yml) by typing `wget https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Install-K3OS/config.yml`. Next, edit the file by typing `sudo vi config.yml`. Follow the comments in that file to get everything set up, and note that it will save you a fair bit of time to use the "github" option under "authorized keys." Github has good instructions for how this works, and it will allow you to quickly ssh from Git Bash into the K3OS instances.

## Post-Install Configuration

The main thing you need to do at this point is install [Helm](https://helm.sh/). Unless something is changed that I don't know about, K3OS still does not include a package manager. That's alright, we just need to do things the old-fashioned way. The easiest option in my experience is to just use the following commands:

```

$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh

```

...if that actually works and you can figure out how to get around openssl not being included with K3OS.  If you don't feel like screwing with the install script, do this (changing the version of helm in the event that a newer one is available when you read this):

```

$ wget https://get.helm.sh/helm-v3.7.1-linux-amd64.tar.gz
$ tar -zxvf helm-v3.7.1-linux-amd64.tar.gz
$ sudo mv linux-amd64/helm /usr/local/bin/helm

```

If you use the second option, go ahead and type `helm`.  It should give you Helm's help dialogue.  If it does not, then something has gone haywire.  Feel free to create an issue and I will try to help.

K3s, and by extension K3OS, use a non-standard config file that Helm will not instantly find.  Kubectl will, but not Helm.  To fix this, go ahead and type `export KUBECONFIG=/etc/rancher/k3s/k3s.yaml`.  The issue should now be fixed.

Either way, things will happen, and you will end up with a helm install that is there for the sole purpose of installing Rancher.

## Set up a Load Balancer

...If needed. I have one set up on pfsense, which remains the same even though this is a different setup. If you're looking for a quick option to get things up and running, [follow this guide.](https://rancher.com/docs/rancher/v2.6/en/installation/resources/k8s-tutorials/infrastructure-tutorials/nginx/).

## Continue Reading...

1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
3. [Installing K3OS](https://github.com/tlfjar/rancher-projects/blob/master/Install-K3OS/Install-K3OS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
5. [Installing RancherOS LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS-Legacy/Install-RancherOS.md)
6. [Installing Rancher LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server-Legacy/Install-Rancher-Server.md)
7. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
8. [Installing Longhorn LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn-Legacy/Installing-Longhorn.md)
