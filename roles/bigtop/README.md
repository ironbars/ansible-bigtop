Bigtop
======

Setup role for installing packages from the Apache Bigtop project.

Requirements
------------

None

Role Variables
--------------

Currently, only RedHat derived distributions are supported, and the default
values reflect this.  

### `bigtop_java_version`

Version of Java to install.  This is meant to be part of the name of the
installable package.  This is set to `1.8.0` by default.

### `bigtop_java_pkgs`

List of Java packages to install.

### `bigtop_release`

Bigtop release version.

### `bigtop_repo`

This defines a repository from which to install Bigtop packages.  Right now,
this points to an official Bigtop repo, but this could easily be changed to
install from a local repo.

Dependencies
------------

None

Example Playbook
----------------


    - hosts: all
      roles:
         - { role: bigtop, bigtop_java_version: 1.9.0 }

License
-------

BSD

Author Information
------------------

Marc J. Patton
