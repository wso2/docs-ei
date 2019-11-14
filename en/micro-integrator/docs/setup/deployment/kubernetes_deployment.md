# Kubernetes Deployment

## k8s-ei-operator
Provides native support for the Kubernetes Ecosystem with WSO2 Micro Integrator.
The k8s-ei-operator allows you to deploy your integration solutions as an **Integration** kind into the Kubernetes environment built on top of [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio). 

## Prerequisites
The k8s-ei-operator is built with operator-sdk v0.7.0 and supported in the following environment.

-   [Kubernetes](https://kubernetes.io/docs/setup/) cluster and client v1.11+   
-   [Docker](https://docs.docker.com/)

## Install EI Operator
Please do the following steps to install the EI Kubernetes operator in your Kubernetes environment.

1.  Clone the **k8s-ei-operator** GitHub repository.

    ```bash
    git clone https://github.com/wso2/k8s-ei-operator.git
    ```
    
2.  Change directory to k8s-ei-operator.

    ```bash
    cd k8s-ei-operator
    ```
    
3.  Setup Service Account.

    ```bash
    kubectl create -f deploy/service_account.yaml
    ```
    
5.  Setup RBAC.

    ```bash
    kubectl create -f deploy/role.yaml
    kubectl create -f deploy/role_binding.yaml
    ```
    
6.  Deploy CustomResourceDefinition into the Kubernetes cluster to understand custom resource type.

    ```bash
    kubectl create -f deploy/crds/integration_v1alpha1_integration_crd.yaml
    ```
    
7.  Deploy the k8s-ei-operator.

    ```bash
    kubectl create -f deploy/operator.yaml
    ```
    
8. Apply configuration for the ingress controller

    ```bash
    kubectl apply -f deploy/config_map.yaml
    ```

You can verify the installation by making sure the following deployment are running in your Kubernetes cluster.

```bash
$ kubectl get deployment

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
k8s-ei-operator     1/1     1            1          1m
```

## Deploy and Run Integration Solutions with EI Operator
EI operator supports deployment and running your integration solutions in a Kubernetes environment. 

Here we have demonstrated an example of **HelloWorld** service which response as `{"Hello":"World"}` to the user request. This scenario can be created using `integration_cr.yaml` format as given below.

```yaml
apiVersion: "integration.wso2.com/v1alpha2"
kind: "Integration"
metadata:
  name: "hello-world"
spec:
  replicas: 1
  image: "<Docker image for the Hello World Scenario>"
```

!!! Note
You can refer the [K8s-ei-operator Hello World](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/deployment/k8s-samples/hello-world/) example implemented via our integration tooling called [Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/) to implement and test the application.

To deploy the above integration solution in your kubernetes cluster, execute the following command.

```bash
kubectl apply -f integration_cr.yaml
``` 

If the `hello-world` integration is deployed successfully, it should create `hello-world` integration, `hello-world-deployment`, `hello-world-service` and `ei-operator-ingress` as follows,

```bash
$ kubectl get integration

NAME          STATUS    SERVICE-NAME    AGE
hello-world   Running   hello-service   2m

$ kubectl get deployment

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-deployment   1/1     1            1           2m

$ kubectl get services
NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)       AGE
hello-world-service      ClusterIP   10.101.107.154   <none>        8290/TCP      2m
kubernetes               ClusterIP   10.96.0.1        <none>        443/TCP       2d
k8s-ei-operator          ClusterIP   10.98.78.238     <none>        443/TCP       1d

$ kubectl get ingress
NAME                  HOSTS     ADDRESS     PORTS     AGE
ei-operator-ingress   wso2ei    10.0.2.15   80, 443   2m
```

### Invoke Integration Solution with Ingress Controller

To invoke the integration solution, need to obtain the **External IP** of the ingress load balancer using `kubectl get ingress` command as following.

```bash
$ kubectl get ingress
NAME                  HOSTS     ADDRESS     PORTS     AGE
ei-operator-ingress   wso2ei    10.0.2.15   80, 443   2m
```
Then, add the host **wso2ei** and related external IP (ADDRESS) to the `/etc/hosts` file in your machine.

For **Minikube**, you have to use Minikube IP as the external IP. Hence, run `minikube ip` command to get the IP of the Minikube cluster.

Use the following CURL command to run the `hello-world` deployed in Kubernetes.

```bash
curl http://wso2ei/hello-world-service/services/HelloWorld

{"Hello":"World"}%
```

### Invoke Integration Solution without Ingress Controller
We can invoke out integration solutions without the ingress controller by **[port-forward](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/#forward-a-local-port-to-a-port-on-the-pod)** method for the services. To do that, use the following commands.

i. Port forward

```bash
kubectl port-forward service/hello-world-service 8290:8290
```

ii. Invoke the proxy service

```bash
curl http://localhost:8290/services/HelloWorld

{"Hello":"World"}%
```

### View Integration Process Logs
We can see the output of the running integration solutions by the pod's logs. To monitor that first we need to get the associated **pod id**. 

First use the `kubectl get pods` command to list down all the deployed pods.

```bash
$ kubectl get pods

NAME                               READY   STATUS    RESTARTS   AGE
hello-deployment-c68cbd55d-j4vcr   1/1     Running   0          3m
k8s-ei-operator-6698d8f69d-6rfb6   1/1     Running   0          2d
```

To view the logs of the associated pod, run the `kubectl logs <pod name>` command. This will print the output of the given pod id.

```bash
$ kubectl logs hello-deployment-c68cbd55d-j4vcr

...
[2019-10-28 05:29:24,225]  INFO {org.wso2.micro.integrator.initializer.deployment.application.deployer.CAppDeploymentManager} - Successfully Deployed Carbon Application : HelloWorldCompositeApplication_1.0.0{super-tenant}
[2019-10-28 05:29:24,242]  INFO {org.apache.synapse.transport.passthru.core.PassThroughListeningIOReactorManager} - Pass-through HTTP Listener started on 0.0.0.0:8290
[2019-10-28 05:29:24,242]  INFO {org.apache.axis2.transport.mail.MailTransportListener} - MAILTO listener started
[2019-10-28 05:29:24,250]  INFO {org.apache.synapse.transport.passthru.core.PassThroughListeningIOReactorManager} - Pass-through HTTPS Listener started on 0.0.0.0:8253
[2019-10-28 05:29:24,251]  INFO {org.wso2.micro.integrator.initializer.StartupFinalizer} - WSO2 Micro Integrator started in 4 seconds
```

## Change the Default Configuration of EI Operator Ingress Controller
You can change the ingress level configurations in EI operator by `ei-operator-config` Config Map. You can update the `host` prop to change the host for ingress as given below. Ingress will use this host to register every service routes. Default it will use `wso2ei`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: <YOUR-HOST-NAME>
  autoIngressCreation: "true" 
```

## Deploy Integration Solution without Ingress creation
By default, EI operator create an NGINX ingress and exposes HTTP/HTTPS transport protocols through it. If user needs to create a deployment without the default ingress, have to change `autoIngressCreation` prop value as `false` in `ei-operator-config` config map as follows.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: wso2ei
  autoIngressCreation: "false" 
```

##  Run Integration Solution with HTTPS
We can use **ingressTLS** prop in config map to expose ingress NGINX HTTPS transport of your integration application in Kubernetes. If a user has defined **ingressTLS** in the config map, ingress controller will use this TLS and terminates with the given HTTP.

```yaml
ingressTLS: wso2-tls
```

### Sample on Running a Integration Solution with HTTPS transport
First, you need to generate a self-signed certificate and private key using the following command. For more details about the certificate creation refers [this](https://github.com/kubernetes/ingress-nginx/blob/master/docs/user-guide/tls.md#tls-secrets).

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout wso2.key -out wso2.crt -subj "/CN=wso2/O=wso2"
```

Then, create a kubernetes secret called `wso2-tls`, which deliberates to add to the TLS configurations in the ingress spec using the following command.
```bash
kubectl create secret tls wso2-tls --key wso2.key --cert wso2.crt
```

Then add this secret alias called `wso2-tls` to the `ei-operator-config` config map as follows.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: wso2ei
  autoIngressCreation: "false" 
  ingressTLS: wso2-tls
```

Now you can invoke the deployed applications from following URL format.
```bash
https://<HOST-NAME>/<SERVICE-NAME>/<SERVICE-CONTEXT>
```

According the **Hello World** example we can invoke the hello-world proxy as folows,
```bash
curl --cacert wso2.crt https://wso2ei/hello-world-service/services/HelloWorld

{"Hello":"World"}%
```

##  Disable Server-side HTTPS enforcement through Redirect
By default, the ingress controller redirects HTTP requests to the HTTPS port 443 using a **308 Permanent Redirect response** if TLS is enabled for that Ingress. To allow HTTP and HTTPS both request we can configure the config map by adding following prop. 

```yaml
sslRedirect: "false"
```
 
 Overall our `ei-operator-config` config map looks like as follows,
 ```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: wso2ei
  autoIngressCreation: "false" 
  ingressTLS: wso2-tls
  sslRedirect: "false"
```

## Run Inbound Endpoints as HTTP and HTTPS services
Micro Integrator supports to create [Inbound Endpoints](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/inbound-endpoints/about-inbound-endpoints/) which are used to separate endpoint listeners for each HTTP inbound endpoint so that messages are handled separately. Also, we can create any number of inbound endpoints in any port by using it. 

So, we can expose the inbound endpoint ports from the Kubernetes cluster by passing the `inboundPorts` prop inside our `integration_cr.yaml` custom resource file as follows.

```yaml
apiVersion: "integration.wso2.com/v1alpha2"
kind: "Integration"
metadata:
  name: "inbound-samples"
spec:
  replicas: 1
  image: "<Docker image for the inbound endpoint Scenario>"
  inboundPorts:
    - 8000
    - 9000
    ... 
```

Use the following methods to invoke the inbound endpoints in HTTP and HTTPS transports.
**INTEGRATION-NAME** is the value we have used as the metadata name in `integration_cr.yaml`. 

```bash
#Request as HTTP
curl http://<HOST-NAME>/<INTEGRATION-NAME>-inbound/<PORT>/<CONTEXT>

#Request as HTTPS
curl --cacert <CERT_FILE> https://<HOST-NAME>/<INTEGRATION-NAME>-inbound/<PORT>/<CONTEXT>
```

!!! Note
Here we are not allowed to create **HTTPS Inbound Endpoints** inside the integration solutions because every service can be secured through the ingress level encryption by adding the TLS for the ingress-controller. 

## Using Private Registry Image as an Integration Solution
EI Operator allows to use any cutom docker image in private registry as an integration solution. To do that users have to pass the credentials to pull the image for the kubernetes containers as a kubernetes secret. EI operator uses `imagePullSecret` to read the kubernetes secret of the registry credentials. 

You can use the following command to create a kubernetes secret with the credentials of private image.

```bash
kubectl create secret docker-registry <secret-alias> --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-password> --docker-email=<your-email>
```

-   `<secret-alias>` is the name for the secret
-   `<your-registry-server>` is your Private Docker Registry FQDN. (https://index.docker.io/v1/ for DockerHub)
-   `<your-name>` is your Docker username.
-   `<your-password>` is your Docker password.
-   `<your-email>` is your Docker email.

Now you can add the `imagePullSecret` prop to the `integration_cr.yaml` custom resource file as follows.
```yaml
apiVersion: "integration.wso2.com/v1alpha2"
kind: "Integration"
metadata:
  name: "hello-world"
spec:
  replicas: 1
  image: "<Docker image for the Hello World Scenario>"
  imagePullSecret: <secret-alias>
```

## Update existing Integration Deployments in the Cluster
EI Operator allows to update the Docker images used in kubernetes pods(replicas) with the latest update of the tag. To pull the latest tag, we need to delete the associat pod with it pod id as follows.

```bash 
kubectl delete pod <POD-NAME>
```

When run the above command, kubernetes will spawn an another temporary pod which has the same behaviour of the pod we have deleted. Then deleted pod will restart with pulling the latest tag from the docker image path. 

!!! Note 
Here we are recommending to use a different image path for the updated integration solution due to re-pulling the docker image from the existing deployment might be caused to lose some traffic from the outsiders. 
