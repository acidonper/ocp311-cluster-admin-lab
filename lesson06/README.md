# Managing Pods

A multi-project quota, defined by a ClusterResourceQuota object, allows quotas to be shared across multiple projects. Resources used in each selected project will be aggregated and that aggregate will be used to limit resources across all the selected projects.

Please, visit each subsection in order to practice the most common OpenShift service accounts operations.

## Creating a cluster quota based on project labels

Create a new cluster quota which matches with projects labelled as *customcustomselector=true* with the following restrictions:
-   Maximum number of pods: 2
-   Maximun resources in a request range:
    -   CPU 1
    -   Memory 500Mi
-   Maximum number of persistentvolumeclaims: 1
-   Maximum number of imagestreams: 5
-   Maximum number of secrets: 5
-   Maximum number of configmaps: 5

### Solution

```
$ oc create clusterresourcequota for-name --project-label-selector=customcustomselector=true --hard=pods=2 --hard=secrets=5 --hard=configmaps=5 --hard=openshift.io/imagestreams=5 --hard=persistentvolumeclaims=1 --hard=requests.cpu=1 --hard=requests.memory="500Mi"
```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/multiproject_quota.html


# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com