## Configuring Identity Provider for a ROSA instance.

For ROSA, the new __Identity Provider Config__ needs to be done through OCM([Open Container Management](https://open-cluster-management.io/)) and Open-source project on which Red Hat ACM ([Red Hat Advance Cluster Mangement](https://www.redhat.com/en/technologies/management/advanced-cluster-management)) is base on. 


`Open Cluster Management` is actually controling the oauth resource through Hive, by way of an operator, so any change made directly in the OAuth directly in the OpenShift cluster will be overridden.

### Requirement.

* Need login access to console.redhat.com
* Have the ROSA Cli [install](https://docs.openshift.com/rosa/rosa_install_access_delete_clusters/rosa_getting_started_iam/rosa-installing-rosa.html)


###  Accessing your cluster.

1. Go to  https://console.redhat.com/openshift
2. Login


### Configuring Keycloak as an Identity Provider.

Well even if you can do the change on the console, it is advance to use the CLI to do so. By using the CLI we can specifie some credential that can't be define with the UI.

Here the basic command that need to be run.  This will help synchronize the user and the groups.


```
rosa create idp \
    --cluster ${CLUSTER_NAME} \
    --type openid \
    --name ${IDP_NAME} \
    --client-id ${KEYCLOAK_CLIENT_ID} \
    --client-secret ${KEYCLOAK_CLIENT_ID_SECRET} \
    --issuer-url  https://sso-sso-app.apps.cluster.thecatcoders.com/auth/realms/openshift
    --email-claims email \
    --name-claims name \
    --username-claims preferred_username \
    --groups-claims groups
```

:warning: This can take some time to synchronize ( about 1-2 min)
