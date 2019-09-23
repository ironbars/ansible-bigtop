ZooKeeper
=========

This role will install and configure a ZooKeeper ensemble.

Requirements
------------

This role was written for Ansible version 2.6 and later.  However, this should
work on most previous versions, as only core functionality is used.

Role Variables
--------------

### `zk_service_name`

This should probably be left set to `zookeeper`.  It is used to build the names
of ZooKeeper's home and data directories.

### `zk_home_dir`

Home directory of the `zookeeper` user that gets created when the
`zookeeper-server` Bigtop package is installed.  Default will be
`/var/lib/zookeeper`.

### `zk_data_dir`

This is where the ZooKeeper will store snapshots of its database.  Does not
_necessarily_ need to be the same as `zk_home_dir`, but there is likely no
reason to separate them.  Default will be `/var/lib/zookeeper`.

### `zk_max_client_cnxns`

Number of concurrent connections that a _single_ client can have to a _single_
member of the ensemble.  Default is set to 50.

### `zk_tick_time`

Number of milliseconds in each 'tick'.  Default is set to 2000.

### `zk_init_limit`

The amount of time that the initial synchronization phase can take, measured in
'ticks' as defined in `zk_tick_time`.  Default is set to 10.

### `zk_sync_limit`

The amount of time that represents how far out of date a server can be from a
leader, measured in 'ticks' as defined in `zk_tick_time`.  Default is set to 5.

For anything measured in ticks, multiply the value that you set by
`zk_tick_time` to get the amount of time (in milliseconds) that the value
represents.  For example, take a configuration like the following.  

```
zk_tick_time: 2000
zk_init_limit: 10
zk_sync_limit: 5
```

Here, `zk_init_limit` is equal to 20000 ms (i.e. 20 s) and `zk_sync_limit` is
equal to 10000 ms (10 s).  

### `zk_client_port`

The port that ZooKeeper clients will need to connect to (i.e. the port the
server should listen on).

*The following variable has a default set in defaults/main.yml.  However, you
should probably override `zk_quorum`.  The recommendation is to set in
`group_vars/all` (or under `group_vars/all/` if using multiple  files for all
inventory variables).*

### `zk_quorum`

This is a dictionary that uses the host names of your ZooKeeper servers as the
_keys_ and the _values_ should be the ZooKeeper IDs that will correspond to
those servers.  For example, if your ZooKeeper hosts are named zk01, zk02, and
zk03, your `zk_quorum` variable might look like this:

```
zk_quorum:
  zk01: 1
  zk02: 2
  zk03: 3
```

The reason that these are meant to be set at the inventory level is because
hosts that do not run this role might need them as well.  As an example, this
role is meant to be run as part of a Hadoop installation.  Configuration files
that have nothing to do with ZooKeeper itself will need to access these values
if HDFS HA is enabled.  Rather than define these variables in two places (once
at inventory level and as a default as a part of this role), they are only
defined once in `group_vars/all`.

Dependencies
------------

No role dependencies.

Example Playbook
----------------

Example usage:

```
- hosts: servers
  roles:
     - { role: zookeeper }
```

License
-------

BSD

Author Information
------------------

Marc J. Patton
