# Install Longhorn

One concept in Kubernetes is persistence. Rancher does not (as far as I am aware) come with persistence built in. There are options out there, but my favorite is [Longhorn](https://github.com/longhorn/longhorn). I do not know how it works. I assume it is some form of magic. For what I'm doing, it doesn't really matter. We need persistent storage, and therefore we need longhorn.

First, go to the Global dropdown and find your cluster. On the right-hand side of that menu, you will see two options, click the one that says "system":

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Installing-Longhorn/Images/Rancher%20menu%20B.png)

When that opens, click "Apps" on the top menu:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Installing-Longhorn/Images/Rancher%20system%20screen%20A.png)

Press "Launch," and you will be greeted with a bunch of apps in catalog form. search for "Longhorn," and you'll find this:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Installing-Longhorn/Images/Longhorn%20screen.png)

Click Details, and you'll be taken to a screen full of options. Go down to configuration options, and fill it out like so:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Installing-Longhorn/Images/Longhorn%20settings%201.png)

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Installing-Longhorn/Images/Longhorn%20settings%202.png)

Basically, don't mess with anything, and then click launch. The only change is the "minimum cpu", which needs to be changed to "0.1". I don't know why, but this made a huge difference when I first started adding it to my longhorn deployments.

After that, click launch, and wait. When the bar turns green, you are ready to go. Feel free to try out other apps, but make sure you click on the "default" project if you are going to do that. The system project is for things that affect...well... the system.

## Continue Reading...

1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
3. [Installing K3OS](https://github.com/tlfjar/rancher-projects/blob/master/Install-K3OS/Install-K3OS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
5. [Installing RancherOS LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS-Legacy/Install-RancherOS.md)
6. [Installing Rancher LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server-Legacy/Install-Rancher-Server.md)
7. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
8. [Installing Longhorn LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn-Legacy/Installing-Longhorn.md)
