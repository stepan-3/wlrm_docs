{% termtabs name="Debian 8.x (jessie-backports)" -%}
# apt-get install dirmngr
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/ignore-release-date
# echo 'deb http://archive.debian.org/debian jessie-backports main' > /etc/apt/sources.list.d/jessie-backports.list
# echo 'deb http://repo.wallarm.com/debian/wallarm-node jessie/2.14/' > /etc/apt/sources.list.d/wallarm.list
# echo 'deb http://repo.wallarm.com/debian/wallarm-node jessie-backports/2.14/' >> /etc/apt/sources.list.d/wallarm.list
# apt-get update
{%- tab name="Debian 9.x (stretch)" -%}
# apt-get install dirmngr
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node stretch/2.14/' >/etc/apt/sources.list.d/wallarm.list"
# apt-get update
{%- tab name="Debian 9.x (stretch-backports)" -%}
# apt-get install dirmngr
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node stretch/2.14/' >/etc/apt/sources.list.d/wallarm.list"
# sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node stretch-backports/2.14/' >> /etc/apt/sources.list.d/wallarm.list"

[warning][IMPORTANT]uncomment the following line in /etc/apt/sources.list:
deb http://deb.debian.org/debian stretch-backports main contrib non-free

# apt-get update
{%- tab name="Debian 10.x (buster)" -%}
# apt-get install dirmngr
# apt-key adv --keyserver keys.gnupg.net --recv-keys 72B865FD
# sh -c "echo 'deb http://repo.wallarm.com/debian/wallarm-node buster/2.14/' > /etc/apt/sources.list.d/wallarm.list"
# apt-get update
{%- tab name="CentOS 6.x" -%}
# yum install --enablerepo=extras -y epel-release centos-release-SCL
# rpm -i https://repo.wallarm.com/centos/wallarm-node/6/2.14/x86_64/Packages/wallarm-node-repo-1-4.el6.noarch.rpm
{%- tab name="CentOS 7.x" -%}
# yum install -y epel-release
# rpm -i https://repo.wallarm.com/centos/wallarm-node/7/2.14/x86_64/Packages/wallarm-node-repo-1-4.el7.noarch.rpm
{%- endtermtabs %}