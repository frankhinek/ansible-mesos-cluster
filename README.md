# ansible-mesos-cluster

Ansible playbook to build multi-node Apache Mesos clusters.


## Cluster Configuration
Before you run the playbook you will need to modify the variables in in `group_vars/all`.

* `mesos_pkg_version` is the Debian package published by Mesosphere.
* `marathon_pkg_version` is the Debian package published by Mesosphere.
* `marathon_install_type` determines whether you install marathon from source or the Mesosphere package.
* `mesos_quorum_count` should be set based on the number of Mesos Masters. See the note below on how to determine the quorum count.
* `mesos_cluster_name` is arbitrary when testing but you can change to match your naming conventions.

```
# Select the Mesos package version install:
mesos_pkg_version: 0.20.0-1.0.ubuntu1404

# Choose whether to install Marathon using a Mesosphere package or from GitHub source
marathon_install_type: "package" # valid options are "package" or "source"
marathon_pkg_version: 0.6.1-1.1 # if install type is "package" which version to use

# The Mesos quorum value is based on the number of Mesos Masters. Take the
# number of masters, divide by 2, and round-up to nearest integer. For example,
# if there are 1 or 2 masters the quorum count is 1. If there are 3 or 4
# masters then the quorum count is 2. For 5 or 6 masters enter 3 and so on.
mesos_quorum_count: "2"

mesos_local_address: "{{ansible_eth0.ipv4.address}}"
mesos_cluster_name: "Cluster01"
zookeeper_client_port: "2181"
zookeeper_url: "zk://{{ groups.mesos_masters | join(':' + zookeeper_client_port + ',') }}:{{ zookeeper_client_port }}/mesos"
```
