Ansible for Bigtop
==================
**WARNING: Some of the more recent changes here have not been fully tested.**

This repo is a collection of Ansible roles and playbooks for the deployment of a
Hadoop cluster using packages built by [Apache Bigtop](http://bigtop.apache.org).
These scripts are eventually intended to be submitted to that project, as
currently there are only recipes for Puppet.  

Usage
-----
Assuming that this repo has been cloned to `/etc/ansible` on a machine that can
reach your cluster:  

```
$ ansible-playbook hadoop-playbook.yml
```

This command should be suitable to install and configure the following Hadoop
components:  

- Zookeeper
- HDFS
- YARN
- Hive

Also included in the `utils` directory are playbooks to ease administration
tasks (i.e. starting, stopping, and restarting services).  In particular, the
`start-hadoop.yml` playbook can fully bootstrap a cluster by formatting HDFS and
starting all necessary services.

```
$ ansible-playbook utils/start-hadoop.yml
```

You can start specific services using Ansible tags:  

```
$ ansible-playbook --tags 'zookeeper,hdfs' utils/start-hadoop.yml
```

You can also start, stop, or restart specific subcomponents of each service
(e.g. just HDFS NameNodes, or just the Hive Metastore):

```
$ ansible-playbook --tags 'namenodes' utils/restart-hadoop.yml

OR

$ ansible-playbook --tags 'namenodes' utils/services/hdfs/restart-hdfs.yml
```

```
$ ansible-playbook --tags 'hive-metastore' utils/restart-hadoop.yml

OR

$ ansible-playbook --tags 'hive-metastore' utils/services/hive/restart-hive.yml
```

Requirements
------------

OS: CentOS 7 (fully tested on 7.4, 7.5, and 7.6)  
Hadoop: 2.x  
Java > 1.6 (but < 1.9)

Starting at the top, the following variables must properly defined in
`group_vars/all/all`:  

- `cluster_name`: name of your cluster (this will be your default HDFS
  namespace)
- `dns_domain`: domain name of your cluster

Also of note is that these playbooks have only been tested using Java 1.8
(OpenJDK).  To use Oracle Java, change the names of these packages in the
`java_pkgs` variable.  

Limitations
-----------
Currently, only RedHat-based distributions are supported.  There are plans to
support Debian-based distributions in the future.  Fair warning, however: that
particular task is not necessarily high priority.  Other limitations:

- can only build a cluster that uses HDFS HA
- can only build a cluster that uses non-HA YARN

...amongst others

TODO
----
Add roles for:

- ResourceManagers
- NodeManagers
- Hadoop Clients
- Hive Metastore
- Hive Server
- Hive Clients

Once these are done, move on to other services

Also, add support for:

- non-HA HDFS
- HA YARN
- Debian-bases OSes

Need documentation for all roles!  
