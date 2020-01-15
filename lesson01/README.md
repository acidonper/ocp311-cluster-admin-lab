# Managing Nodes

In this section, some examples about managing node states and different day to day operations regarding these nodes are included.

Please, visit each subsection in order to practice the most common OpenShift nodes operations.

## Listing nodes

List OpenShift cluster nodes' information obtaining the following information:

-   Name
-   Status
-   Roles
-   Age
-   Version
-   External IP
-   OS Image
-   Kernel version
-   Container Runtime

### Solution

```
$ oc get nodes -o wide
```

## Viewing nodes

Obtain the following information in infra and compute nodes:

-   Labels
-   CPU, Memory and Pods capacity
-   Allocated resources
-   Scheduling state

### Solution

-   Labels
```
$ oc get nodes | grep "infra\|compute" | awk '{print "echo "$1" && oc get node "$1" -o json | jq '\''.metadata.labels'\''" }'  | sh
```
-   CPU, Memory and Pods capacity
```
$ oc get nodes | grep "infra\|compute" | awk '{print "echo "$1" && oc get node "$1" -o json | jq '\''.status.capacity'\''" }'  | sh
```
-   Allocated resources 
```
$ oc get nodes | grep "infra\|compute" | awk '{print "echo "$1" && oc describe node "$1" | grep -A 6 \"Allocated resources\"" }' | sh
$ oc adm top nodes
```
-   Scheduling state
```
$ oc get nodes | grep "infra\|compute" | awk '{print "echo "$1" && oc describe node "$1" | grep Unschedulable" }' | sh
```

## Deleting nodes

Delete node *node3* from the OpenShift cluster. It is important in mind the following information:

-   Create a new file /etc/ansible/hosts_delete in order to allocate node information
-   Use /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml to uninstall the node

### Solution

```
$ oc delete node node3.9590.internal

$ oc get nodes

$ vi /etc/ansible/hosts_delete
[OSEv3:children]
nodes 

[OSEv3:vars]
ansible_user=ec2-user
ansible_become=yes
openshift_deployment_type=openshift-enterprise

[nodes]
node3.9590.internal openshift_node_group_name='node-config-compute' openshift_node_problem_detector_install=true

$ ansible-playbook -i /etc/ansible/hosts_delete /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml
```

## Adding hosts

Create a new node, *node3*, in the OpenShift cluster. It is important in mind the following information:

-   Create a new file /etc/ansible/hosts_new with all information from /var/preserve/hosts
-   Include required information in order to add the new host (*node3*)
-   Use /usr/share/ansible/openshift-ansible/playbooks/adhoc/uninstall.yml to uninstall the node

### Solution

```
$ cp /var/preserve/hosts /etc/ansible/hosts_new

$ vi /etc/ansible/hosts_new

...
[OSEv3:children]
lb
masters
etcd
nodes
nfs
new_nodes
...
...
[new_nodes]
node3.9590.internal openshift_node_group_name='node-config-compute' openshift_node_problem_detector_install=true
...

$ ansible-playbook -i /etc/ansible/hosts_new /usr/share/ansible/openshift-ansible/playbooks/openshift-node/scaleup.yml

$ oc get nodes

$ oc label node/node3.9590.internal logging-infra-fluentd=true

$ oc get pods -o wide --all-namespaces | grep node3

```

## Updating labels on nodes

-   Create a new label on **node3** with key *reinstalled* and value *true* and verify the new node label.
-   Delete *reinstalled* label define on node **node3**.

### Solution

-   Create a new label on node **node3** with key *reinstalled* and value *true* and verify the new node label.
```
$ oc label node node3.9590.internal reinstalled=true

$ oc get node node3.9590.internal --show-labels
```
-   Delete *reinstalled* label defined on node **node3**.
```
$ oc label node node3.9590.internal reinstalled-
```

## Listing pods on nodes

-   List all pods on **node1** and **node3** nodes
-   List all fluend pods on compute nodes
-   List all pods on all nodes

### Solution

-   List all pods on **node1** and **node3** nodes\
```
$ oc adm manage-node node1.9590.internal node3.9590.internal --list-pods
```
-   List all fluend pods on compute nodes
```
$ oc adm manage-node --selector="node-role.kubernetes.io/compute=true" --list-pods --pod-selector=component=fluentd
```
-   List all pods on all nodes
```
$  oc adm manage-node --selector= --list-pods
```

## Marking nodes as unschedulable or schedulable

Mark node **node3** as unschedulable

### Solution

```
$ oc adm manage-node node3.9590.internal --schedulable=false
```

## Evacuating pods on nodes

-   Evacuate node **node2** excluding DaemonSet pods and set 5 seconds for each pod to terminate gracefully
-   Obtain a list of pods which would be moving to other nodes or deleting if **master3** is evacuated


### Solution

-   Evacuate node **node2** excluding DaemonSet pods and set 5 seconds for each pod to terminate gracefully

```
$ oc adm manage-node node2.9590.internal --list-pods

$ oc adm drain node2.9590.internal --grace-period=-5 --ignore-daemonsets=true

$ oc adm manage-node node2.9590.internal --list-pods
```

-   Obtain a list of pods which would be moving to other nodes or deleting if **master3** is evacuated.

```
$ oc adm drain master3.9590.internal --dry-run=true
```

## Updating labels on nodes

Delete *reinstalled* label define in node **node3**.

### Solution

```
$ oc label node node3.9590.internal reinstalled-
```

## Obtain Nodes Configuration

Obtain node-config-compute, node-config-infra and node-config-master configuration parameters

### Solution

```
$ oc get cm node-config-compute -n openshift-node  -o yaml
$ oc get cm node-config-infra -n openshift-node  -o yaml
$ oc get cm node-config-master -n openshift-node  -o yaml
```

## Configuring Node Resources

Modify node-config-compute configuration in order to allow 30 pods maximum per node

### Solution

```
$ oc edit cm node-config-compute -n openshift-node

...
kubeletArguments:
  max-pods: 
    - "30"
...

$ oc get nodes

**Wait for all nodes are Ready and Schedulables
```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/manage_nodes.html

# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com