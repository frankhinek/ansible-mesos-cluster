# ansible-mesos-cluster

Ansible playbook to build multi-node Apache Mesos clusters.


## Cluster Configuration
Before you run the playbook you will need to modify the variables in in `group_vars/all`.

* `mesos_pkg_version` is the Debian package published by Mesosphere.
* `marathon_pkg_version` is the Debian package published by Mesosphere.
* `marathon_install_type` determines whether you install marathon from source or the Mesosphere package.
* `mesos_cluster_name` is arbitrary when testing but you can change to match your naming conventions.

```
---
# Select the Mesos package version install:
mesos_pkg_version: 0.20.0-1.0.ubuntu1404

# Choose whether to install Marathon using a Mesosphere package or from GitHub source
marathon_install_type: "package" # valid options are "package" or "source"
marathon_pkg_version: 0.6.1-1.1 # if install type is "package" which version to use

mesos_local_address: "{{ansible_eth0.ipv4.address}}"
mesos_cluster_name: "Cluster01"
zookeeper_client_port: "2181"
zookeeper_url: "zk://{{ groups.zookeepers | join(':' + zookeeper_client_port + ',') }}:{{ zookeeper_client_port }}/mesos"
```
