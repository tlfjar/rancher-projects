# Installing RancherOS

Now, to the meat of things.  I am running rancher on [RancherOS](https://github.com/rancher/os/releases) (Latest stable version, and the vmware-specific ISO), using [this cloud-config.yaml](RancherServer/cloud-config.yaml) as the configuration file.  You will want to change the network parts of that file and the ssh keys to fit your environment.  You can download it using this call:

`wget https://raw.githubusercontent.com/tlfjar/rancher-projects/master/RancherServer/cloud-config.yaml`

Edit the file using the following:

`vi cloud-config.yaml` and then press `i` to edit ("insert," technically).  When you're done, press the "escape" key and then type `:wq`

The installation call is as follows:

`sudo ros install -c cloud-config.yaml -d /dev/sda`

If you have your file saved elsewhere, you can replace the `cloud-config.yaml` portion above with the link.  If you are using a private Github repository, I recommend that you download the file using the `wget` command first, as it seems that the token changes pretty quickly.

You can now ssh into your server (after configuring the IP you set as static) using your ssh method of choice.  I personally use putty, because Windows.

## Post-Install Configuration

You may have noticed that I added an open-iscsi install into the cloud-config.yaml file above.  To make that work, we need to make one change to open-iscsi's configuration file.  Do the following:

`sudo vi /etc/iscsi/iscsid.conf` Once again press `i`, comment `iscsid.startup = /bin/systemctl start iscsid.socket`, and uncomment `iscsid.startup = /sbin/iscsid`.  Press the "Escape" key and then type `:wq` to exit.

NOTE: that was probably totally unnecessary, but it is good practice for doing it three more times shortly, and it doesn't hurt anything.

You'll want to do this a total of four times, once for the VM/Computer that will run Rancher Server, and three more times for the VMs/Computers that will run Rancher Agent.
