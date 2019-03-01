# GitLab on OpenShift
This document describes the deployment of GitLab 11.3 on Openshift Origin 3.9

## Deploy template to OpenShift
First, deploy the template in this repository to the OpenShift cluster:

```
oc login -u system:admin

oc project openshift

oc create -f gitlab-openshift-template.json
```

## Deploy GitLab

Health checks will fail - this is a known issue with Gitlab 11. For successful
deployment, we need to edit the `DeploymentConfig` and delete the `Health` and
`Readiness` probes.
