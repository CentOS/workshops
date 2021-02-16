# Consuming CentOS Stream

[DevConf.cz 2021 workshop](https://devconfcz2021.sched.com/event/gmSG/consuming-centos-stream)


## Convert CentOS Linux 8 into CentOS Stream 8

TBD


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

