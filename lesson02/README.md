# Managing Users

In this section, some examples about managing users and teams are included.

Please, visit each subsection in order to practice the most common OpenShift users operations.


## Creating a User

-   Create a new user named *lesson02user01* and define **lesson02user01** password in httpaswd authentication method
-   Give the new user view role in OpenShift cluster

### Solution

-   Create a new user named *lesson02user01* and define **lesson02user01** password in httpaswd authentication method

```
$ htpasswd -b /root/htpasswd.openshift lesson02user01 lesson02user01

$ ansible masters -i /etc/ansible/hosts -m copy -a "src=/root/htpasswd.openshift dest=/etc/origin/master/htpasswd"
```

-   Give the new user view role in OpenShift cluster

```
$ oc adm policy add-cluster-role-to-user edit lesson02user01
```

## Viewing User and Identity Lists

List users and identities in OpenShift Cluster

### Solution
```
$ oc get user
$ oc get identity
```

## Creating Groups

-   Create a new user named *lesson02user02* and define **lesson02user02** password in httpaswd authentication method.
-   Create a new group name *lesson02group* and include *lesson02user01* and *lesson02user02*
-   Give the new group view role in OpenShift cluster (Test if lesson02user02 is able to access all project information)


### Solution

-   Create a new user named *lesson02user02* and define **lesson02user02** password in httpaswd authentication method.

```
$ htpasswd -b /root/htpasswd.openshift lesson02user01 lesson02user01
$ ansible masters -i /etc/ansible/hosts -m copy -a "src=/root/htpasswd.openshift dest=/etc/origin/master/htpasswd"
```

-   Create a new group name *lesson02group* and include *lesson02user01* and *lesson02user02*

```
$ oc adm groups new lesson02group lesson02user01 lesson02user02

$ oc get groups
```

-   Give the new group view role in OpenShift cluster
```
$ oc adm policy add-cluster-role-to-group view lesson02group

$ oc login https://loadbalancer.9590.example.opentlc.com/ -u lesson02user02 -p lesson02user02

$ oc get all -n lesson01

```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/manage_users.html

# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com