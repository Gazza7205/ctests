# STS (Secure Token Store) Gateway

## Prerequisite:
1. A Gateway license (`LICENSE.xml`) in this directory
2. The database connection details should be provided in the chart

## Usage:
`apim-charts/examples/otk-scenarios/sts/gateway> ./ssg-sts-deploy.sh`

## Options:
To change release name. Edit `ssg-sts-deploy.sh`:
 - `STS_RELEASE_NAME="ssg-sts"` (e.g. `STS_RELEASE_NAME="username-ssg-sts-v4"` for a shared environment to identify pod owner and version)
 - `EDGE_RELEASE_NAME="ssg-edge"`
 
## LoadBalancer / Ingress
The default sample uses the GCP's load balancer to expose the ports. To use other service
types, uncomment the ingress block

## Trying It Out With Docker for Desktop
Replace the following in the ssg-sts-values-env-01.yaml file,
```
    annotations:
      cloud.google.com/load-balancer-type: "Internal"
```
with,
```
    annotations: {}
```

## Health Check:
The sample is using an OTK specific readiness check which targets the 200 OK response code
from an endpoint that the OTK solution provides. This provides a more accurate measure of 
"ready" for the STS Gateway.

## Potential Environment Issue:
The ssg-sts-deploy.sh uses --wrap=0 in the following two commands
```
STS_KEY=$(base64 ./ssg-sts.p12 --wrap=0)
EDGE_KEY=$(base64 ./ssg-edge.p12 --wrap=0)
```
We have seen in our test enviroment that depending on the OS, the '--wrap=0' is not required. 
And when specified in incompatible environment, you may get errors like, 
```
Error: unable to build kubernetes objects from release manifest: error validating "": error validating data: unknown object type "nil" in Secret.data.SSG_SSL_KEY"
```