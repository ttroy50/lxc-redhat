# lxc-redhat: LXC template for Redhat

This template can create a RedHat RHEL container on Ubuntu.

It is based off the centos lxc container template in Ubuntu 14.04 
with some additional changes copied from the oracle and fedora templates.

It should support RHEL 5, 6 and 7 but has only been tested on RHEL7.1 for now.

It assumes you are using a RHEL DVD mounted on the host as the install media. The mount point is assumed to be

```
/media/rhel-server-$release.$minorrelease-x86_64-dvd
/media/supp-server-$release.$minorrelease-rhel-$release-x86_64-dvd
```

Where, 

  * $release is the major release number e.g. 7
  * $minorrelease is the minor release number e.g. 1


Once created the install media will be mounted from the host into the container in the same location. 

## RHEL 7.1

To create a new RHEL 7.1 container:

Mount the RHEL 7.1 DVD at the following locations

```
/media/rhel-server-7.1-x86_64-dvd
/media/supp-server-7.1-rhel-7-x86_64-dvd
```

```
$ sudo lxc-create -t redhat -n redhat71 -- --release=7 --minorrelease=1 --clean
```

Options available include

```
usage:
    $1 -n|--name=<container_name>
        [-p|--path=<path>] [-c|--clean] [-R|--release=<redhat_release>] [-a|--arch=<arch of the container>]
        [-h|--help]
Mandatory args:
  -n,--name         container name, used to as an identifier for that container from now on
Optional args:
  -p,--path         path to where the container rootfs will be created, defaults to /var/lib/lxc/name.
  -c,--clean        clean the cache
  -R,--release      redhat release for the new container. if the host is redhat, then it will defaultto the host's release.
  -M,--minorrelease redhat minor release. If not set defaults to 3 for release=6 (i.e. RHEL6.3) and 1 for release=7 (i.e. RHEL7.1)
     --fqdn         fully qualified domain name (FQDN) for DNS and system naming
     --repo         repository to use (url)
  -a,--arch         Define what arch the container will be [i686,x86_64]
  -h,--help         print this help
```

### Mounting DVD as repo

Once created you can mount the install media into the container by creating a file /etc/yum.repos.d/rhel-media.repo


```
[rhel-media]
metadata_expire=-1
name=RHEL7.1 Media
baseurl=file:///media/rhel-server-7.1-x86_64-dvd
gpgcheck=0
enabled=1

[rhel-media-supp]
metadata_expire=-1
name=RHEL7.1 Supp Media
baseurl=file:///media/supp-server-7.1-rhel-7-x86_64-dvd
gpgcheck=0
enabled=1
```