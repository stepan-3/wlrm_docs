{% termtabs name="Debian 8.x (jessie)" -%}
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# echo 'deb http://repo.wallarm.com/debian/wallarm-node jessie/' >/etc/apt/sources.list.d/wallarm.list
# apt-get update
{%- tab name="Debian 9.x (stretch)" -%}
# apt-get install dirmngr
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node stretch/' >/etc/apt/sources.list.d/wallarm.list"
# apt-get update
{%- tab name="Ubuntu 14.04 LTS (trusty)" -%}
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# echo 'deb http://repo.wallarm.com/ubuntu/wallarm-node trusty/' >/etc/apt/sources.list.d/wallarm.list
# apt-get update
{%- tab name="Ubuntu 16.04 LTS (xenial)" -%}
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# echo 'deb http://repo.wallarm.com/ubuntu/wallarm-node xenial/' >/etc/apt/sources.list.d/wallarm.list
# apt-get update
{%- tab name="Ubuntu 18.04 LTS (bionic)" -%}
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# sh -c "echo 'deb http://repo.wallarm.com/ubuntu/wallarm-node bionic/' >/etc/apt/sources.list.d/wallarm.list"
# apt-get update
{%- tab name="CentOS 7.x" -%}
# yum install -y epel-release
# rpm -i https://repo.wallarm.com/centos/wallarm-node/7/x86_64/Packages/wallarm-node-repo-1-2.el7.centos.noarch.rpm
{%- endtermtabs %}
