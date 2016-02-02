= Fun with Docker Networking
== Overview
Docker added overlay networking capabilty since 1.9 and later switched to embeded DNS based service discovery instead of /etc/hosts.
I have played with 1.10.0-rc1 and later moved to 1.10.0-rc2. 1.10.0-rc2 fixed a few issues I encounted in rc1. Just a word of caution, "Upgrade" from 1.10.0-rc1 to 1.10.0-rc2
is destructive, i.e. not a real upgrade, see https://github.com/docker/docker/issues/19842. 

==Set Up
I have set up a cluster of Ubuntu 14.04.3 VMs on top of a Openstack Kilo deployment. Install Docker is fairly easy:


