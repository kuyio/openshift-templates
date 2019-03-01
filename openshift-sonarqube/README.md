# SonarQube on OpenShift
This document describes the deployment of SonarQube 7.1 on Openshift Origin 3.9

## Deploy template to OpenShift
First, deploy the template in this repository to the OpenShift cluster:

```
oc login -u system:admin

oc project openshift

oc create -f sonarqube-postgresql-template.json
```

## Removing the Template
In case your want to upload a new version of it:

```
oc delete template/sonarqube -n openshift
```
