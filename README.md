LLNW collectd packaging for RPM
===============================

* Upstream:  https://github.com/llnw/collectd/tree/master/contrib/redhat

Building the package:
---------------------

* Enable [EPEL]
* `yum install mock`
* `ln --force -s /etc/mock/epel-5-x86_64.cfg /etc/mock/default.cfg`
* Fetch the [current version] and save it in `~/rpmbuild/SOURCES/`
* Build the SRPM:
  `mock -r epel-5-x86_64 --buildsrpm --spec ~/rpmbuild/SPECS/collectd.spec --sources ~/rpmbuild/SOURCES/`
* Build the RPM:
  `mock -r epel-5-x86_64 --no-clean --rebuild /var/lib/mock/centos-6-x86_64/result/collectd-X.Y.Z-NN.src.rpm`
* Repeat as needed for other environments

yajl for RHEL/CentOS 5.x:
------------------------

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

You will also need to install the [mock package dependency not in a repository].


  [current version]: https://github.com/llnw/collectd/releases/download/collectd-5.4.1-llnw6/collectd-5.4.1.llnw6.tar.gz
  [EPEL]: http://dl.fedoraproject.org/pub/epel/
  [Xen]: http://wiki.xenproject.org/wiki/Xen_4.2_Build_From_Source_On_RHEL_CentOS_Fedora#Notes_for_CentOS_5.8_x64
  [mock package dependency not in repo]: https://fedoraproject.org/wiki/Using_Mock_to_test_package_builds#Building_packages_that_depend_on_packages_not_in_a_repository
