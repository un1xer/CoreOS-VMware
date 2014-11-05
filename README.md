# CoreOS VMware images deployment

Latest releases of CoreOS' VMware-images now include open-vm-tools.  
This is a tiny project to use these instead of the 'vmware_insecure' images, and be prepared for the future.

See this [github-comment](https://github.com/coreos/coreos-overlay/issues/499#issuecomment-58461747):  
*This only applies to the vmware image, the older vmware\_insecure image will eventually be killed off.*

## What

You'll need 2 files from CoreOS' release, e.g. at http://alpha.release.core-os.net/amd64-usr/current/

- coreos_production_vmware.vmx
- coreos_production_vmware_image.vmdk.bz2

See `deploy_coreos04_on_esxi.sh` for some details.

To enable a user, etcd and fleet add an ISO-image to the VM, as a config-drive with the label `config-2`. 
See file `user_data_04` and file `04_make_ISO.sh`.  
If you need `mkisofs` on OS X, install 'dvdrtools' with brew: `brew install dvdrtools`.

I choose 'coreos04' in the scripts, because I already had 3 CoreOS-hosts running as 'vmware_insecure' images. But number 04 seems to be cooperating: 

<code>hbokh@coreos04 ~ $ fleetctl list-machines  
MACHINE		IP		METADATA  
6ed4b296...	192.168.1.3	-  
85941ee5...	192.168.1.4	-  
85f337f8...	192.168.1.2	-  
90924c46...	192.168.1.1	-  </code>

Note: The $private_ipv4 and $public_ipv4 substitution variables referenced in other documents are not supported on VMware. See this [link](https://coreos.com/docs/running-coreos/platforms/vmware/).  
So you'll need a different and unique ISO for every separate CoreOS guest on VMware...


## Issues

- No bunzip2 on VMware ESXi... Yuo'll have to deflate the .vmdk on a different system and SCP it over.

## Thanks

Inspired by & taken from: [deploy_coreos_on_esxi.sh](https://github.com/lamw/vghetto-scripts/blob/master/shell/deploy_coreos_on_esxi.sh), by William Lam.