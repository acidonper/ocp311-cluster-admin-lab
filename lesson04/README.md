# Configuring Service Accounts

Service accounts provide a flexible way to control API access without sharing a regular user’s credentials.

Principal use cases:

-   Replication controllers make API calls to create or delete pods.
-   Applications inside containers can make API calls for discovery purposes.
-   External applications can make API calls for monitoring or integration purposes.

Please, visit each subsection in order to practice the most common OpenShift service accounts operations.


## Managing Service Accounts

In order to implement service account examples, please implement the following steps:

-   List service accounts on OpenShift Cluster
-   Create a service account named *lesson04* in *lesson03user02project01*
-   Describe the new service account properties
-   Allow the new service accounts to view resources in the *lesson03project02* project

### Solution

-   List service accounts on OpenShift Cluster
```
$ oc get sa --all-namespaces
```

-   Create a service account named *lesson04* in *lesson03user02project01*
```
$ oc create sa lesson04 -n lesson03user02project01  
```

-   Describe the new service account properties
```
$ oc describe sa lesson04 -n lesson03user02project01  
```

-   Allow the new service accounts to view resources in the *lesson03project02* project
```
$ oc policy add-role-to-user view system:serviceaccount:lesson03user02project01:lesson04 -n lesson03user02project02
```

# Interesting Links

-   https://docs.openshift.com/container-platform/3.11/admin_guide/service_accounts.html


# License

BSD

# Author Information

 Asier Cidon - Cloud Consultant

 asier.cidon@redhat.com