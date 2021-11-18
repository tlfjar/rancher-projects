## Installing Rancher 2.x Server (NEW)

This part is suspiciously simple. First, you need to add the rancher repo: `helm repo add rancher-latest https://releases.rancher.com/server-charts/latest`. After that, you need to creat a namespace for it: `kubectl create namespace cattle-system`.

Now on to actually installing Rancher. Personally, I terminate tls at an external ingress, which makes things easier. The install command for that is as follows:

```
helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.my.org \
  --set replicas=3 \
  --set tls=external
```

If you do this, make absolutely sure that the only thing that can connect to your nodes is the load balancer.

You can also use one of [these three options](https://rancher.com/docs/rancher/v2.5/en/installation/install-rancher-on-k8s/#5-install-rancher-with-helm-and-your-chosen-certificate-option) or use [Cert Manager.](https://rancher.com/docs/rancher/v2.5/en/installation/install-rancher-on-k8s/#4-install-cert-manager) The docs do a way better job of explaining what to do here than I possibly can. I haven't tested all of these options, so I don't really have a recommendation.

Congratulations, you now have Rancher installed! Connect to it using the address on the load balancer (or whatever else is needed in the event that you didn't use external tls termination), and move on to the "using rancher" section.

## Continue Reading...

1. [Home](https://github.com/tlfjar/rancher-projects/blob/master/README.md)
2. [My Office Setup](https://github.com/tlfjar/rancher-projects/blob/master/office-setup/office-setup.md)
3. [Installing K3OS](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS/Install-RancherOS.md)
4. [Installing Rancher](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server/Install-Rancher-Server.md)
5. [Installing RancherOS LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-RancherOS-Legacy/Install-RancherOS.md)
6. [Installing Rancher LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Install-Rancher-Server-Legacy/Install-Rancher-Server.md)
7. [Using Rancher 2.x](https://github.com/tlfjar/rancher-projects/blob/master/Using-Rancher/Using-Rancher.md)
8. [Installing Longhorn LEGACY](https://github.com/tlfjar/rancher-projects/blob/master/Installing-Longhorn-Legacy/Installing-Longhorn.md)
