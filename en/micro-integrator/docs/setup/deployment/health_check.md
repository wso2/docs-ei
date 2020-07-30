# Basic Health Check

WSO2 Micro Integrator provides a dedicated API for health checking of the server so that it can be used by a load 
balancer prior to routing traffic to a particular node.

The health check API has two behaviors depending on whether the deployment is mutable or immutable. If the deployment 
is mutable (which is mostly the configuration in centralized deployments), the probe will give a ready status when the 
server starts successfully. If the deployment is immutable, the probe will give a ready status only if all the CApps 
are deployed successfully during server startup. The probe will return the list of fault CApps if there are any 
faulty CApps in that case. The health check API serves at:

`http://localhost:9201/healthz`

!!! Note
    If you're running the server instance with a different port offset other than the default which is 10, the heath
    check API will serve at 9191 + offset.  

## Health checking in Kubernetes

### Readiness Probe

The readiness probe is a vital configuration for deployments in Kubernetes that governs the routing logic. The requests 
will not be routed to a pod that is not ready.

Please add the following configurations to your deployment.yaml file in order to configure the readiness probe for
the server. Initial delay, and the period will have to be fine-tuned according to your deployment.

```yaml
readinessProbe:
  httpGet:
    path: /healthz
    port: 9201
  initialDelaySeconds: 3
  periodSeconds: 1
```

!!! Note
    If you're running the server instance with a different port offset other than the default which is 10, the heath
    check API will serve at 9191 + offset.  

### Liveness Probe

The liveness probe is a primary configuration in Kubernetes since it is used to know when to restart a container. For 
example, if the server stops serving requests on the http port even though the server is alive, the container needs to 
be restarted so that Micro Integrator instances serve the requests flawlessly. The default http socket of WSO2 Micro 
Integrator can be used for health checking for liveness.

Please add the following configurations to your deployment.yaml file in order to configure the liveness probe for
the server. Initial delay, and the period will have to be fine-tuned according to your deployment.

```yaml
livenessProbe:
  tcpSocket:
    port: 8290
  initialDelaySeconds: 15
  periodSeconds: 5
```

!!! Note
    If you're running the server instance with a different port offset other than the default which is 10, the heath
    check API will serve at 8280 + offset.  
