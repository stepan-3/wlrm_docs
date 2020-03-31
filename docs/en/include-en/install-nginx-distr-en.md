{% termtabs name="Debian 8.x (jessie-backports)" -%}
# apt-get install --no-install-recommends nginx wallarm-node-nginx libnginx-mod-http-wallarm -t jessie-backports
{%- tab name="Debian 9.x (stretch)" -%}
# apt-get install --no-install-recommends nginx wallarm-node-nginx libnginx-mod-http-wallarm
{%- tab name="Debian 9.x (stretch-backports)" -%}
# apt-get install --no-install-recommends nginx wallarm-node-nginx libnginx-mod-http-wallarm -t stretch-backports
{%- tab name="Debian 10.x (buster)" -%}
# apt-get install --no-install-recommends nginx wallarm-node-nginx libnginx-mod-http-wallarm
{%- tab name="CentOS 6.x" -%}
# yum install nginx wallarm-node-nginx nginx-mod-http-wallarm
{%- tab name="CentOS 7.x" -%}
# yum install nginx wallarm-node-nginx nginx-mod-http-wallarm
{%- endtermtabs %}
