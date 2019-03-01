# Nexus on Openshift
This document describes the deployment of Nexus Repository Manager 3.14.04 on Openshift Origin 3.9.

## Deploy template to Openshift

First, login to the Openshift cluster.
```
oc login -u system:admin
```

Then, select the project, e.g., `devops` into which to deploy the Nexus server:

```
oc project devops
```

Finally, deploy the template in this repository into the project:

```
oc create -f nexus-repository-manager.yaml -n devops
```

## Provision registry secrets

The template makes use of the official, Openshift certified container for `nexus-repository-manager`. This container is available from the Redhat Partner Docker Registry at `registry.connect.redhat.com`. In order for the cluster to access that registry, we first need to provision proper docker credentials
for both, the registry, as well as RedHat's single sign-on service that the Partner Registry delegates to (`sso.redhat.com`).

To add the docker registry credentials for the Partner Registry:

```
oc project devops

oc create secret docker-registry registry-redhat-connect \
 --docker-server=registry.connect.redhat.com \
 --docker-username=<USERNAME> \
 --docker-password=<PASSWORD> \
 --docker-email=<EMAIL>

oc secrets  oc secrets link default registry-redhat-connect --for=pull
```

To add the docker registry credentials for the SSO Service:

```
oc project devops

oc create secret docker-registry registry-redhat-connect-sso \
 --docker-server=sso.redhat.com \
 --docker-username=<USERNAME> \
 --docker-password=<PASSWORD> \
 --docker-email=<EMAIL>

oc secrets  oc secrets link default registry-redhat-connect-sso --for=pull
```

## Deploy Application to Openshift

Either through the Openshift console, or the CLI utility create an instance of the app `nexus` via the template we created. Note: Nexus database requires a minimum of 5GB disk space or the deployment will fail.

## Access Nexus Server
Default credentials on new deployment are `admin` with password `admin123`

## Hosting NuGet Packages
First, login to the Nexus server and create a NuGet API Key (https://nexus.cluster/#user/nugetapitoken).

Then, we need to add the `NuGet API-Key Realm` to the active Realms (https://nexus.cluster/#admin/security/realms) to allow authentication using Nuget Keys.
