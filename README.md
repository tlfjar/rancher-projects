# Rancher 2.x for Lawyers
Running Rancher on ~~RancherOS~~ **K3OS (COMING SOON)** in a remarkably silly environment.

NOTE: This is very much a work in progress, and I'd appreciate any help.  I'm learning this as I go, probably the same as most people that stumble on this REPO.

## Why not just run docker containers and drop the annoyance of kubernetes?

Scalability.  Servers are surprisingly cheap, and the electricity costs in our city are extremely low.  Being able to add additional servers as needed or as whim may direct is a plus.  I am also fond of the way that Rancher handles containers and everything around them.

Survivability is also a benefit.  Things screw up, and it would seem to me that the redundancy afforded by Kubernetes at least might help with that.

Finally, it's kind of enjoyable.  I doubt that I will put anything into production, and I'm not so sure how bright of an idea it is doing that on bare metal.  But the act of at least trying to understand this has been enlightening, if not entirely helpful.

## Goals and Limitations
I will try to document as best as possible why any given file suddenly appears on this repo, either by comments in the file or by updating this readme.  Feel free to message me or bring up issues if I am screwing up anything.

The files will be in accordance with whatever version of an install I am doing at the time.  For example, "rancherserver" is for a regular rancher server, with a self-generated certificate.  I will anonymize other versions a bit more.

As I am not particularly skilled at coding, do not expect much in the way of runfiles and whatnot.  I will do what I can when I figure out how to do it, but I will also do my best to at least put SOMETHING in here to say the steps I took at the command line.

## Table of Contents
1. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
2. [Installing RancherOS](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS/Install-RancherOS.md)
3. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
4. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
5. [Installing Longhorn](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn/Installing-Longhorn.md)

# UPDATE 11/10/2021

So... COVID happened, which was both an adjustment for the legal community and just not fun in general.  As events unfolded, I was messing around with other projects and completely forgot to update this one, despite continuing to work with [Rancher](https://github.com/rancher/rancher).  The team over there has been working really hard over the year-and-a-half since I last updated, and I really need to update this guide with the rather significant changes they've made.  You can still do everything basically the same as I outlined, even with the latest edition of Rancher, but the inclusion of the Cluster Manager streamlines the overall process and makes Section 5 totally obsolete.

As you can see from the first sentence up top, there's been another major change that affects what I previously wrote: [RancherOS is no longer being actively maintained.](https://rancher.com/docs/os/v1.x/en/support/)  Fortunately, this isn't a bad thing at all, because [K3OS](https://github.com/rancher/k3os) is at a point that it has the advantages RancherOS did as a minimal kubernetes platform with less effort and even less overhead.  There's a quirk or two about its installation that needs to be covered, so I will leave the RancherOS section up until I can prepare a proper replacement section.

Long story short, this entire guide needs a rewrite.  Fortunately, the process is easier now, and thus the guide won't have to be as long.  I have also found a few ways to roll steps together by simply adding sections to the config.yaml files.  Since I've already done that in the office, I just need to sanitize the files, get them uploaded, and probably explain what they do.  This may take a bit, so stay tuned!

# UPDATE 3/26/2020

I have completely revamped my system, doubling the number of workers, running the Rancher Server in a high availability environment, and separating my etcd and controlplane nodes from one another.  I'm testing this now, and if it works, I will be updating with the HA setup.

I will also be cleaning up this repository, as staring at it with all of the image files all over the place is annoying.

It also appears that longhorn no longer requires screwing with the files in open-iscsi.  I am testing to confirm this, but that requirement has dissappeared from their instructions.

# UPDATE 12/29/2019

Okay... Technology changes at an absolutely wacky pace, so I will try to give some updates on the latest happenings.  First, [Longhorn](https://github.com/longhorn/longhorn) has been updated, and is working better than it ever has.  I haven't had an issue with it for some time, which is excellent.  My best recommendation is to just install it and not screw with it. Ever. It will do its magic, and we can all be happy.

[Rancher](https://github.com/rancher/rancher) has also updated, and has added a slew of changes.  Most notable (to me) is the integration of [Istio](https://github.com/istio/istio) pretty tightly into the system.  I must admit that I am not entirely clear on the capabilities of Istio, so I can't say what effect that necessarily has.  The addition of Horizontal Pod Autoscalers (HPAs) is also a big deal, though again I'm not overly familiar with it.  That's not everything that's changed, so I'd recommend a review of the [Release Notes](https://github.com/rancher/rancher/releases/tag/v2.3.3).

The previous tutorial still works just fine, with one notable exception:  the interface looks slightly different.  Compare the workloads section cited above with the new one:

![Image of New rancher](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Images/New%20Rancher%20Project%20Screen.png)

Pardon the incredibly small text, which I assume to be a result of cutting this from a bigger window.  Anyway, many things are the same, only "Workloads" at the top has changed to a dropdown box labelled "Resources"

![Resources Menu](https://raw.githubusercontent.com/tlfjar/rancher-projects/master/Images/Resources%20Page.png)

What you need to know about this for the purposes of the above tutorial is that this is where you can find the original "Workloads" button.  If anything else needs changing here, I will note it appropriately.  It is obvious that a lot more power has been injected into Rancher, and it needs to be explored.  I will try to document what I find as I do so.

## TO DO

I need to fully explain some of the things to, um, do with Kubernetes.  I should probably also explain it in the context of running a law firm.  To that end, I will leave you with one tidbit.  If you went through the tutorial, you may have noticed something in the "catalogs" section of Rancher:

![Add Catalog](https://github.com/tlfjar/rancher-projects/blob/master/Images/Catalog%20Screen.png?raw=true)

You may also notice that I have an extra catalog named "docassemble."  It turns out that the maker of [Docassemble](https://github.com/jhpyle/docassemble) created a Helm Chart to deploy said program on a cluster.  It works, and it is good.  To do so, click that "Add Catalog" Button, and fill it out:

![Add Docassemble](https://github.com/tlfjar/rancher-projects/blob/master/Images/Add%20Chart.png?raw=true)

You can now use the same procedure used on longhorn to deploy Docassemble on your cluster.  However, do make sure to read the readme on its [Github page](https://github.com/jhpyle/charts) to make sure you know what you're doing first.

There's other stuff you can do, of course.  You can run a website, manage your clients, streamline tasks, and even run wacky machine learning stuff using [Kubeflow](https://www.kubeflow.org/). Rancher even has an experimental version in apps to try.  If you come up with something awesome, feel free to let me know, and I will get the info out there as best I can.
