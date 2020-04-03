The amount of memory determines the quality of work of the statistical algorithms.
The recommended value is 75% of the total server memory. For example, if the server has
32 GB of memory, the recommended allocation size is 24 GB.

**Allocate the operating memory size for Tarantool:**

Open for editing the configuration file of Tarantool:

=== "Debian 8.x (jessie)"
    vi /etc/default/wallarm-tarantool

=== "Debian 9.x (stretch)"
    vi /etc/default/wallarm-tarantool

=== "Debian 10.x (buster)"
    vi /etc/default/wallarm-tarantool

=== "Ubuntu 14.04 LTS (trusty)"
    vi /etc/default/wallarm-tarantool

=== "Ubuntu 16.04 LTS (xenial)"
    vi /etc/default/wallarm-tarantool

=== "Ubuntu 18.04 LTS (bionic)"
    vi /etc/default/wallarm-tarantool

=== "CentOS 6.x"
    vi /etc/sysconfig/wallarm-tarantool

=== "CentOS 7.x"
    vi /etc/sysconfig/wallarm-tarantool

=== "Amazon Linux 2"
    vi /etc/sysconfig/wallarm-tarantool

Set the allocated memory size in the configuration file of Tarantool via the
`SLAB_ALLOC_ARENA` directive.

For example:

```
SLAB_ALLOC_ARENA=24
```

**Restart Tarantool:**

=== "Debian 8.x (jessie)"
    systemctl restart wallarm-tarantool

=== "Debian 9.x (stretch)"
    systemctl restart wallarm-tarantool

=== "Debian 10.x (buster)"
    systemctl restart wallarm-tarantool

=== "Ubuntu 14.04 LTS (trusty)"
    service wallarm-tarantool restart

=== "Ubuntu 16.04 LTS (xenial)"
    service wallarm-tarantool restart

=== "Ubuntu 18.04 LTS (bionic)"
    service wallarm-tarantool restart

=== "CentOS 6.x"
    service wallarm-tarantool restart

=== "CentOS 7.x"
    systemctl restart wallarm-tarantool

=== "Amazon Linux 2"
    systemctl restart wallarm-tarantool
