# Now for the fun(?) part. Using Rancher 2.x

After you've done all of the above, you will (hopefully) be greeted with the following:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/Rancher%20starting%20screen.png)

Now, what you have kind of works, with a ton of caveats.  If you want, you can click on apps and just start adding random stuff to see if it'll at least install.  I do not recommend that, because there's still some important things to do.  Follow along below and your life will be easier.

## Enable catalogs

On the top menu of the global view, as shown above, hover over tools, and click on catalogs:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/Rancher%20add%20catalogs.png)

You will be greeted with a friendly screen showing that only the library catalogs are enabled.  There's some good stuff in the library, and you may never need more than what is in it.  Just in case, though, go ahead and enable the other two catalogs:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/Catalogs.png)

It will take a second, but doing this puts both library and helm charts in your Apps menu.  I usually turn on the helm incubator as well, though I haven't use anything in it yet.  But it doesn't hurt anything.

## Install Longhorn

You should probably go ahead and follow [this guide](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn.md) prior to messing with the ingress.  I wrote the below with the assumption that Longhorn would be installed by now.

## Learn to ingress

Remember when I said to use weave as the provider way back when we set up the cluster?  That was for a reason, namely that weave is the only one that I could get to work with an ingress.  To give an example of how to ingress, lets make a way to connect to our Longhorn UI.

First, on the system project, click on "Workloads" on the menu on top of the screen.  You will be brought to a page that looks like this, albeit with less stuff in it:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/System%20workloads.png)

Click on "Load Balancing":

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/System%20workloads%20A.png)

The next screen will have a button that says "add ingress".  Click it, and you'll be brought to this screen, which you will fill out as shown:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/Rancher%20ingress.png)

Change <yourinternalsitehere> to whatever your internal site is, click save, and then hop over to PFSense.  Make sure that "DNS Resolver" is active, and add "longhorn.<yourinternalsitehere> in the resolver (I can give instructions on that if needed).  Point that address to ANY of the IP addresses used for your cluster agents EXCEPT the one that actually runs the rancher server.  If you want, you could also set up a load balancer, but I'm not going to both y'all with it at the moment.  Just point it to one of the agents.

Go to whatever site it is, and you should see this:

![Image of Rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/Using-Rancher/Images/Longhorn%20ui%20screen.png)

You can apply this concept endlessly, just make sure that you always set an app that you are launching to "ClusterIP." Rancher gets mad at you if you set it to "LoadBalancer."  Those sentences will make way more sense when you start deploying apps.

## Continue Reading...
1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
3. [Installing RancherOS](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS/Install-RancherOS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
5. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
6. [Installing Longhorn](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn/Installing-Longhorn.md)
