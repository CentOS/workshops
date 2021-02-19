# Consuming CentOS Stream

[DevConf.cz 2021 workshop](https://devconfcz2021.sched.com/event/gmSG/consuming-centos-stream)


## Convert CentOS Linux 8 into CentOS Stream 8

This is an easy one, if you brought your CentOS Linux machines with you, you
might be able to complete this way before I even finish talking. Right now
there are 3 steps, though we're very very close to dropping this down to 2:

    $ sudo dnf install centos-release-stream
    $ sudo dnf swap centos-linux-repos centos-stream-repos
    $ sudo dnf distro-sync

## Running Stream in a container

The CentOS Stream 8 container image is available at [quay.io in the CentOS namespace](https://quay.io/repository/centos/centos?tab=tags).

The image is built for 3 architectures:
* amd64
* arm64
* ppc64le

If you want to give it a shot, you can first pull the relevant container image:

    podman pull quay.io/centos/centos:stream8

(make sure to use `:stream8` to be sure to get CentOS Stream 8 and not Stream 9 in the future)

Now the image is on your machine and you can create a container:

    podman run --rm -ti quay.io/centos/centos:stream8 bash
    [root@3dc8ba0b94b2 /]# 𝙸

Let's confirm this is actually Stream variant of the CentOS family of operating systems:

    [root@3dc8ba0b94b2 /]# cat /etc/os-release
    NAME="CentOS Stream"
    VERSION="8"
    ID="centos"
    ID_LIKE="rhel fedora"
    VERSION_ID="8"
    PLATFORM_ID="platform:el8"
    PRETTY_NAME="CentOS Stream 8"
    ANSI_COLOR="0;31"
    CPE_NAME="cpe:/o:centos:centos:8"
    HOME_URL="https://centos.org/"
    BUG_REPORT_URL="https://bugzilla.redhat.com/"
    REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux 8"
    REDHAT_SUPPORT_PRODUCT_VERSION="CentOS Stream"

We can check if some updates already landed in the YUM repositories:

    [root@3dc8ba0b94b2 /]# dnf update
    Last metadata expiration check: 0:01:26 ago on Tue Feb 16 17:16:10 2021.
    Dependencies resolved.
    ================================================================================
     Package                                Arch   Version          Repo       Size
    ================================================================================
    Upgrading:
     device-mapper                          x86_64 8:1.02.175-4.el8 baseos    375 k
     device-mapper-libs                     x86_64 8:1.02.175-4.el8 baseos    408 k
     dnf                                    noarch 4.4.2-6.el8      baseos    540 k
     dnf-data                               noarch 4.4.2-6.el8      baseos    151 k
     glibc                                  x86_64 2.28-148.el8     baseos    3.6 M
     glibc-common                           x86_64 2.28-148.el8     baseos    1.3 M
     glibc-minimal-langpack                 x86_64 2.28-148.el8     baseos     56 k
     hwdata                                 noarch 0.314-8.8.el8    baseos    1.7 M
     json-c                                 x86_64 0.13.1-0.4.el8   baseos     40 k
     libcomps                               x86_64 0.1.11-5.el8     baseos     81 k
     libdnf                                 x86_64 0.55.0-3.el8     baseos    678 k
     libsolv                                x86_64 0.7.16-2.el8     baseos    362 k
     python3-dnf                            noarch 4.4.2-6.el8      baseos    539 k
     python3-dnf-plugins-core               noarch 4.0.18-3.el8     baseos    228 k
     python3-hawkey                         x86_64 0.55.0-3.el8     baseos    113 k
     python3-libcomps                       x86_64 0.1.11-5.el8     baseos     52 k
     python3-libdnf                         x86_64 0.55.0-3.el8     baseos    765 k
     python3-rpm                            x86_64 4.14.3-12.el8    baseos    157 k
     python3-subscription-manager-rhsm      x86_64 1.28.12-1.el8    baseos    365 k
     python3-syspurpose                     x86_64 1.28.12-1.el8    baseos    302 k
     rpm                                    x86_64 4.14.3-12.el8    baseos    541 k
     rpm-build-libs                         x86_64 4.14.3-12.el8    baseos    155 k
     rpm-libs                               x86_64 4.14.3-12.el8    baseos    339 k
     subscription-manager-rhsm-certificates x86_64 1.28.12-1.el8    baseos    260 k
     yum                                    noarch 4.4.2-6.el8      baseos    202 k
    Installing dependencies:
     libevent                               x86_64 2.1.8-5.el8      baseos    253 k
     unbound-libs                           x86_64 1.7.3-15.el8     appstream 503 k
    Installing weak dependencies:
     glibc-langpack-en                      x86_64 2.28-148.el8     baseos    826 k
     python3-unbound                        x86_64 1.7.3-15.el8     appstream 119 k
     rpm-plugin-systemd-inhibit             x86_64 4.14.3-12.el8    baseos     77 k

    Transaction Summary
    ================================================================================
    Install   5 Packages
    Upgrade  25 Packages

    Total download size: 15 M
    Is this ok [y/N]:

There are some and we can install if we want.

This is a great and easy way to try CentOS Stream 8 (or even use it as a base
for your application, even though we suggest using [UBI
8](https://catalog.redhat.com/software/containers/ubi8/5c647760bed8bd28d0e38f9f?gti-tabs=unauthenticated))


## CentOS Stream in your cloud environment

#### AWS

Assuming you have a working EC2 setup, we publish AMIs in the various regions. You can grab the IDs from https://wiki.centos.org/Cloud/AWS

On my workstation I have `awscli` installed and configured for my aws account. I also have some variables seen in the command line that specify things like my subnet, security group, and things that you should come with. 

    $ aws ec2 run-instances --image-id ami-059f1cc52e6c85908 --count 1 --instance-type t2.micro --key-name ${AWS_KEY} --subnet-id ${AWS_SUBNET} --associate-public-ip-address

    $ aws ec2 describe-instances
    
Or visit https://console.aws.amazon.com

#### Other clouds, using the images on cloud.centos.org

https://cloud.centos.org is where we store our images, so if we don't have images seeded into your provider, it's the GenericCloud image is a good place to start.

Sometimes the images need tweaks, other times you can import them directly, do your cloud-init thing and go from there.

## Vagrant

One of the special images we produce is for vagrant: https://app.vagrantup.com/centos/boxes/stream8
One for libvirt, one for virtualbox. You probably saw those on cloud.centos.org while we were looking at other images. 
The `vagrant` folder of this repository has a small `Vagrantfile` to use, or you can start your own with: 

    $ vagrant init centos/stream8
    $ vagrant up

## CentOS Stream for other Architectures

Let's take a detour back into the mirrors and cloud.centos.org

You'll notice that a lot of these things (even the container images) are
available for machines that are not x86_64! So aarch64 folks, those of you with
a fancy POWER workstations, you've got options too.

## What can you do with your newly installed CentOS Stream boxes? 

- Put it on your laptop
- Run a server at home
- Start playing with containers
- Join us on the mailing lists to watch out for exciting new ways to actually
  contribute, in collaboration with RHEL maintainers.

If you find any bugs, those go under the RHEL section with a 'CentOS Stream' version in Red Hat's bugzilla.

