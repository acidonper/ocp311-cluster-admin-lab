# Managing Projects

In OpenShift Container Platform, projects are used to group and isolate related objects. As an administrator, give developers access to certain projects, allow them to create their own or give them administrative rights within individual projects could be tasks which have to be performed.

Please, visit each subsection in order to practice the most common OpenShift projects operations.


## Modifying the Template for New Projects

-   Create a new project template named *lesson02group-projects* on OpenShift based on create-bootstrap-project-template with a new RoleBinding in order to promote *lesson02group* group's users to administrators 
-   Create a new project name *lesson03project01* using above template created

### Solution

-   Create a new project template named *lesson02group-projects* on OpenShift based on create-bootstrap-project-template with a new RoleBinding in order to promote *lesson02group* group's users to administrators 

```
$ vi default-project-template.yaml (**Copy ./examples/default-project-template.yaml content)

$ oc create -f default-project-template.yaml
```

-   Create a new project name *lesson03project01* using above template created

```
$ oc process -p PROJECT_NAME=lesson03project01 -n openshift lesson02group-projects | oc create -f -
```

## Disabling Self-provisioning

Prevent an authenticated user group from self-provisioning new projects.

### Solution

```
$ oc patch clusterrolebinding.rbac self-provisioners -p '{"subjects": null}'

$ oc patch clusterrolebinding.rbac self-provisioners -p '{ "metadata": { "annotations": { "rbac.authorization.kubernetes.io/autoupdate": "false" } } }'
```

## Setting the Project-wide Node Selector

In order to implement node selectors in the OpenShift Cluster, It is required to perform the following tasks:

-   Include label *customselector* with value *true* on node **node3**
-   Create an individual project named *lesson03project* with a node selector which makes use of the new label
-   Create a new project template named *lesson03-projects-selector* on OpensShift  based on create-bootstrap-project-template which defines the previous selector
-   Create a new project name *lesson03project02* using above template created

### Solution

-   Include label *customselector* with value *true* on node **node3**
```
$ oc label node node3.9590.internal customselector=true
```

-   Create an individual project with a node selector
```
oc adm new-project lesson03project --node-selector='customselector=true'
```

-   Create a new project template named *lesson03-projects-selector* on OpensShift  based on create-bootstrap-project-template which defines the previous selector

```
$ vi default-project-template-selector.yaml (**Copy ./examples/default-project-template-selector.yaml content)

$ oc create -f default-project-template-selector.yaml
```

-   Create a new project name *lesson03project02* using above template created

```
$ oc process -p PROJECT_NAME=lesson03project02 -n openshift lesson03-projects-selector | oc create -f -
```


## Enabling Self-Provisioned Projects for a specific User

Give user *lesson02user02* self-provisioner projects rights and test it creating *lesson02user02* project.

### Solution

```
$ oc login https://loadbalancer.9590.example.opentlc.com/ -u lesson02user02 -p lesson02user02

$ oc new-project lesson03user02project01

$ oc adm policy add-cluster-role-to-user self-provisioner lesson02user02

$ oc new-project lesson03user02project01

```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/managing_projects.html


# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com