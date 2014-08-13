LLNW collectd packaging for RPM
===============================

* Upstream:  https://github.com/llnw/collectd/tree/master/contrib/redhat

Building the package with mock:
-------------------------------

mock builds the package in a chroot and avoids pitfalls of building on the build host.

* Enable [EPEL]
* `yum install mock`
* `ln --force -s /etc/mock/epel-5-x86_64.cfg /etc/mock/default.cfg`
* Fetch the [current version] and save it in `~/rpmbuild/SOURCES/`
* Build the SRPM:
  `mock -r epel-5-x86_64 --buildsrpm --spec ~/rpmbuild/SPECS/collectd.spec --sources ~/rpmbuild/SOURCES/`
* Build the RPM:
  `mock -r epel-5-x86_64 --no-clean --rebuild /var/lib/mock/epel-5-x86_64/result/collectd-X.Y.Z-NN.src.rpm`
* The results will be in `/var/lib/mock/epel-5-x86_64/result/`
* Repeat as needed for other environments

Building the package native:
----------------------------

This is easier but will pollute your build environment with dependencies.

* Enable [EPEL]
* Fetch the [current version] and save it in `~/rpmbuild/SOURCES/`
* rpmbuild -bb collectd.spec
* The results will be in `~/rpmbuild/RPMS/x86_64/`

yajl for RHEL/CentOS 5.x:
-------------------------

The following instructions from the [Xen] project provide a suitable yajl rpm
for RHEL/CentOS 5.x.  This can be used to build and distribute with the
curl-json plugin.

    yum install cmake
    wget http://ftp.redhat.com/pub/redhat/linux/enterprise/6Server/en/os/SRPMS/yajl-1.0.7-3.el6.src.rpm
    rpm -i --nomd5 yajl-1.0.7-3.el6.src.rpm
    cd /usr/src/redhat/SPECS/
    cp -a yajl.spec yajl.spec.orig
    wget http://pasik.reaktio.net/xen/patches/yajl-el6-spec-rpmbuild-fix-for-el5.patch
    patch -p0 < yajl-el6-spec-rpmbuild-fix-for-el5.patch
    rpmbuild -bb yajl.spec --define 'dist .el5'
    cd /usr/src/redhat/RPMS/x86_64/
    rpm -Uvh yajl-1.0.7-3.el5.x86_64.rpm yajl-devel-1.0.7-3.el5.x86_64.rpm

If you use mock, you will also need to provide the yajl deps as a repo available in `/etc/mock/epel-5-x86_64.cfg`.


  [current version]: https://github.com/llnw/collectd/releases/download/collectd-5.4.1-llnw6/collectd-5.4.1.llnw6.tar.gz
  [EPEL]: http://dl.fedoraproject.org/pub/epel/
  [Xen]: http://wiki.xenproject.org/wiki/Xen_4.2_Build_From_Source_On_RHEL_CentOS_Fedora#Notes_for_CentOS_5.8_x64
