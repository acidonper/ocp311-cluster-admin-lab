# Setting Quotas

A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per project. It can limit the quantity of objects that can be created in a project by type, as well as the total amount of compute resources and storage that may be consumed by resources in that project.

Please, visit each subsection in order to practice the most common OpenShift service accounts operations.

## Define a quota in a specific project

Define the following limits in a new project *lesson05quotas*:

-   Maximum number of pods: 1
-   Maximun resources in a request range:
    -   CPU 1
    -   Memory 500Mi 
    -   Required Storage 50Gi
-   Maximum resource in a limit range:
    -   CPU 2
    -   Memory 2Gi
-   Maximum number of persistentvolumeclaims: 1
-   Maximum number of imagestreams: 5
-   Maximum number of secrets: 5
-   Maximum number of configmaps: 5

### Solution

```
$ oc new-project lesson05quotas

$ vi quotas.yaml (**Copy ./examples/quotas.yaml content)

$ oc create -f quotas.yml -n lesson05quotas
```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/quota.html


# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com