# Setting Limit Ranges

A limit range, defined by a LimitRange object, enumerates compute resource constraints in a project at the pod, container, image, image stream, and persistent volume claim level, and specifies the amount of resources that a pod, container, image, image stream, or persistent volume claim can consume.

All resource create and modification requests are evaluated against each LimitRange object in the project. If the resource violates any of the enumerated constraints, then the resource is rejected. If the resource does not set an explicit value, and if the constraint supports a default value, then the default value is applied to the resource.

For CPU and Memory limits, if you specify a max value, but do not specify a min limit, the resource can consume CPU/memory resources greater than max value`.

Please, visit each subsection in order to practice the most common OpenShift service accounts operations.

## Creating Limit Range in a Specific Project

Define the following limits in a new project named _lesson7_:

-   Pods
    -   CPU
        -   Max 2
        -   Min 200m
    -   Memory
        -   Max 2Gi
        -   Min 100Mi
-   Containers
    -   CPU
        -   Max 1
        -   Min 200m
        -   Default 300m
        -   Default Request 200m
    -   Memory
        -   Max 1Gi
        -   Min 100Mi
        -   Default 200Mi
        -   Default Request 100Mi
        -   Max Request Limit 3
-   ImageStreams 10
-   PersistentVolumeClaim
    -   Max 5Gi
    -   Min 1Gi

### Solution

```
$ vi limits.yaml (**Copy ./examples/limits.yaml content)
$ oc apply -f limits.yaml
```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/limits.html

# License

BSD

# Author Information

Asier Cidon - Cloud Consultant

asier.cidon@redhat.com
