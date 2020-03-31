The amount of memory determines the quality of work of the statistical algorithms.
The recommended value is 75% of the total server memory. For example, if the server has
32 GB of memory, the recommended allocation size is 24 GB.

**Allocate the operating memory size for Tarantool:**

Open for editing the configuration file of Tarantool:

{% termtabs name="Debian 8.x (jessie)" -%}
# vi /etc/default/wallarm-tarantool
{%- tab name="Debian 9.x (stretch)" -%}
# vi /etc/default/wallarm-tarantool
{%- tab name="Debian 10.x (buster)" -%}
# vi /etc/default/wallarm-tarantool
{%- tab name="CentOS 6.x" -%}
# vi /etc/sysconfig/wallarm-tarantool
{%- tab name="CentOS 7.x" -%}
# vi /etc/sysconfig/wallarm-tarantool
{%- endtermtabs %}

Set the allocated memory size in the configuration file of Tarantool via the
`SLAB_ALLOC_ARENA` directive.

For example:

```
SLAB_ALLOC_ARENA=24
```

**Restart Tarantool:**

{% termtabs name="Debian 8.x (jessie)" -%}
# systemctl restart wallarm-tarantool
{%- tab name="Debian 9.x (stretch)" -%}
# systemctl restart wallarm-tarantool
{%- tab name="Debian 10.x (buster)" -%}
# systemctl restart wallarm-tarantool
{%- tab name="CentOS 6.x" -%}
# service wallarm-tarantool restart
{%- tab name="CentOS 7.x" -%}
# systemctl restart wallarm-tarantool
{%- endtermtabs %}
