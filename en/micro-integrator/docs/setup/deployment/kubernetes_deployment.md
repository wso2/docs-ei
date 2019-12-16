# Kubernetes Deployment

## k8s-ei-operator
Provides native support for the Kubernetes ecosystem with WSO2 Micro Integrator.
The k8s-ei-operator allows you to deploy your integration solutions as an **Integration** kind into the Kubernetes environment built on top of [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio). 

## Prerequisites
The k8s-ei-operator is built with operator-sdk v0.7.0 and supported in the following environments.

-   [Kubernetes](https://kubernetes.io/docs/setup/) cluster and client v1.11+   
-   [Docker](https://docs.docker.com/)

## Install the EI Operator
Follow the steps given below to install the EI Kubernetes operator in your Kubernetes environment.

1.  Clone the **k8s-ei-operator** GitHub repository.

    ```bash
    git clone https://github.com/wso2/k8s-ei-operator.git
    ```
    
2.  Change the directory to k8s-ei-operator.

    ```bash
    cd k8s-ei-operator
    ```
    
3.  Set up the service account.

    ```bash
    kubectl create -f deploy/service_account.yaml
    ```
    
5.  Set up RBAC.

    ```bash
    kubectl create -f deploy/role.yaml
    kubectl create -f deploy/role_binding.yaml
    ```
    
6.  Deploy a CustomResourceDefinition into the Kubernetes cluster to understand custom resource type.

    ```bash
    kubectl create -f deploy/crds/integration_v1alpha1_integration_crd.yaml
    ```
    
7.  Deploy the k8s-ei-operator.

    ```bash
    kubectl create -f deploy/operator.yaml
    ```
    
8. Apply the configurations for the ingress controller.

    ```bash
    kubectl apply -f deploy/config_map.yaml
    ```

You can verify the installation by making sure the following deployments are running in your Kubernetes cluster.

```bash
kubectl get deployment

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
k8s-ei-operator     1/1     1            1          1m
```

## Deploy and run integration solutions with the EI operator
The EI operator supports the deployment and running of your integration solutions in a Kubernetes environment. 

Here, we have demonstrated an example of a **HelloWorld** service, which outputs the response as `{"Hello":"World"}` to the user request. This scenario can be created using the `integration_cr.yaml` format as given below.

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
    You can refer the [K8s-ei-operator Hello World](https://ei.docs.wso2.com/en/latest/micro-integrator/setup/deployment/k8s-samples/hello-world/) example implemented via [WSO2 Integration Studio](https://ei.docs.wso2.com/en/latest/micro-integrator/develop/WSO2-Integration-Studio/) to try out the application.

To deploy the above integration solution in your kubernetes cluster, execute the following command:

```bash
kubectl apply -f integration_cr.yaml
``` 

If the `hello-world` integration is deployed successfully, it should create the `hello-world` integration, `hello-world-deployment`, `hello-world-service`, and `ei-operator-ingress` as follows:

```bash
kubectl get integration

NAME          STATUS    SERVICE-NAME    AGE
hello-world   Running   hello-service   2m

kubectl get deployment

NAME                     READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-deployment   1/1     1            1           2m

kubectl get services
NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)       AGE
hello-world-service      ClusterIP   10.101.107.154   <none>        8290/TCP      2m
kubernetes               ClusterIP   10.96.0.1        <none>        443/TCP       2d
k8s-ei-operator          ClusterIP   10.98.78.238     <none>        443/TCP       1d

kubectl get ingress
NAME                  HOSTS     ADDRESS     PORTS     AGE
ei-operator-ingress   wso2ei    10.0.2.15   80, 443   2m
```

### Invoke the integration solution with Ingress controller

To invoke the integration solution, obtain the **External IP** of the ingress load balancer using the `kubectl get ingress` command as follows:

```bash
kubectl get ingress
NAME                  HOSTS     ADDRESS     PORTS     AGE
ei-operator-ingress   wso2ei    10.0.2.15   80, 443   2m
```
Then, add the **wso2ei** host and related external IP (ADDRESS) to the `/etc/hosts` file in your machine.

For **Minikube**, you have to use Minikube IP as the external IP. Hence, run `minikube ip` command to get the IP of the Minikube cluster.

Use the following CURL command to run the `hello-world` deployed in Kubernetes:

```bash
curl http://wso2ei/hello-world-service/services/HelloWorld

{"Hello":"World"}%
```

### Invoke the integration solution without Ingress Controller
You can invoke out integration solutions without the ingress controller using the **[port-forward](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/#forward-a-local-port-to-a-port-on-the-pod)** method for services. Use the following commands:

1. Port forward:

    ```bash
    kubectl port-forward service/hello-world-service 8290:8290
    ```

2. Invoke the proxy service:

    ```bash
    curl http://localhost:8290/services/HelloWorld

    {"Hello":"World"}%
    ```

### View integration process logs
See the output of the running integration solutions using the pod's logs. 

1. First, you need to get the associated **pod id**. Use the `kubectl get pods` command to list down all the deployed pods.

    ```bash
    kubectl get pods

    NAME                               READY   STATUS    RESTARTS   AGE
    hello-deployment-c68cbd55d-j4vcr   1/1     Running   0          3m
    k8s-ei-operator-6698d8f69d-6rfb6   1/1     Running   0          2d
    ```

2.  To view the logs of the associated pod, run the `kubectl logs <pod name>` command. This will print the output of the given pod ID.

    ```bash
    kubectl logs hello-deployment-c68cbd55d-j4vcr

    ...
    [2019-10-28 05:29:24,225]  INFO {org.wso2.micro.integrator.initializer.deployment.application.deployer.CAppDeploymentManager} - Successfully Deployed Carbon Application : HelloWorldCompositeApplication_1.0.0{super-tenant}
    [2019-10-28 05:29:24,242]  INFO {org.apache.synapse.transport.passthru.core.PassThroughListeningIOReactorManager} - Pass-through HTTP Listener started on 0.0.0.0:8290
    [2019-10-28 05:29:24,242]  INFO {org.apache.axis2.transport.mail.MailTransportListener} - MAILTO listener started
    [2019-10-28 05:29:24,250]  INFO {org.apache.synapse.transport.passthru.core.PassThroughListeningIOReactorManager} - Pass-through HTTPS Listener started on 0.0.0.0:8253
    [2019-10-28 05:29:24,251]  INFO {org.wso2.micro.integrator.initializer.StartupFinalizer} - WSO2 Micro Integrator started in 4 seconds
    ```

## Change the default configuration of the EI Operator-Ingress Controller
You can change the ingress-level configurations in the EI operator by using the `ei-operator-config` configuration map. You can update the `host` property to change the host for the ingress as given below. Ingress will use this host to register all the service routes. By default, it will use `wso2ei`.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: <YOUR-HOST-NAME>
  autoIngressCreation: "true" 
```

## Deploy the integration solution without Ingress creation
By default, the EI operator creates an NGINX ingress, through which it exposes HTTP/HTTPS transport protocols. If user needs to create a deployment without the default ingress, you have to change the `autoIngressCreation` property value to `false` in the `ei-operator-config` config mapping as follows.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ei-operator-config
data:
  host: wso2ei
  autoIngressCreation: "false" 
```

##  Run the integration solution with HTTPS
We can use the **ingressTLS** property in the configuration mapping to expose an ingress NGINX HTTPS transport of your integration application in Kubernetes. If a user has defined **ingressTLS** in the configuration mapping, the ingress controller uses this TLS and terminates with the given HTTP.

```yaml
ingressTLS: wso2-tls
```

### Sample: Running an integration solution with HTTPS
First, you need to generate a self-signed certificate and private key using the following command. For more details about certificate creation, see [this link](https://github.com/kubernetes/ingress-nginx/blob/master/docs/user-guide/tls.md#tls-secrets).

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout wso2.key -out wso2.crt -subj "/CN=wso2/O=wso2"
```

Then, create a Kubernetes secret called `wso2-tls`, which deliberates to add to the TLS configurations in the ingress spec using the following command.

```bash
kubectl create secret tls wso2-tls --key wso2.key --cert wso2.crt
```

Then, add this secret alias called `wso2-tls` to the `ei-operator-config` configuration mappint as follows:

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

Now, you can invoke the deployed applications from following URL format.

```bash
https://<HOST-NAME>/<SERVICE-NAME>/<SERVICE-CONTEXT>
```

According to the **Hello World** example, we can invoke the hello-world proxy as folows:

```bash
curl --cacert wso2.crt https://wso2ei/hello-world-service/services/HelloWorld

{"Hello":"World"}%
```

##  Disable Server-side HTTPS enforcement through Redirect
By default, the ingress controller redirects HTTP requests to the HTTPS port 443 using a **308 Permanent Redirect response** if TLS is enabled for that Ingress. To allow HTTP and HTTPS both request we can configure the config map by adding following property. 

```yaml
sslRedirect: "false"
```
 
The final `ei-operator-config` configuration mapping will look as follows:

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

## Run inbound endpoints as HTTP and HTTPS services
[Inbound Endpoints](https://ei.docs.wso2.com/en/latest/micro-integrator/references/synapse-properties/inbound-endpoints/about-inbound-endpoints/) in the Micro Integrator are used for separating endpoint listeners. That is, for each HTTP inbound endpoint, messages are handled separately. Also, we can create any number of inbound endpoints on any port. 

So, we can expose the inbound endpoint ports from the Kubernetes cluster by passing the `inboundPorts` property inside our `integration_cr.yaml` custom resource file as follows:

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
**INTEGRATION-NAME** is the value we have used as the metadata name in the `integration_cr.yaml` file. 

```bash
#Request as HTTP
curl http://<HOST-NAME>/<INTEGRATION-NAME>-inbound/<PORT>/<CONTEXT>

#Request as HTTPS
curl --cacert <CERT_FILE> https://<HOST-NAME>/<INTEGRATION-NAME>-inbound/<PORT>/<CONTEXT>
```

!!! Note
    Here, we are not allowed to create **HTTPS Inbound Endpoints** inside integration solutions because every service can be secured through ingress-level encryption by using TLS for the ingress-controller. 

## Using the private registry image as an integration solution
The EI operator allows the use of any custom Docker image in a private registry as an integration solution. To achieve this, users have to pass the credentials to pull the image to the Kubernetes containers as a Kubernetes secret. The EI operator uses `imagePullSecret` to read the Kubernetes secret of the registry credentials. 

You can use the following command to create a Kubernetes secret with the credentials of the private image:

```bash
kubectl create secret docker-registry <secret-alias> --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-password> --docker-email=<your-email>
```

-   `<secret-alias>` is the name for the secret
-   `<your-registry-server>` is your Private Docker Registry FQDN. (https://index.docker.io/v1/ for DockerHub)
-   `<your-name>` is your Docker username.
-   `<your-password>` is your Docker password.
-   `<your-email>` is your Docker email.

Now, you can add the `imagePullSecret` property to the `integration_cr.yaml` custom resource file as follows:

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

## Update existing integration deployments in the cluster

The EI operator allows you to update the Docker images used in Kubernetes pods(replicas) with the latest update of the tag. To pull the latest tag, we need to delete the associated pod with its pod ID as follows:

```bash 
kubectl delete pod <POD-NAME>
```

When you run the above command, Kubernetes will spawn another temporary pod, which has the same behaviour of the pod we have deleted. Then the deleted pod will restart by pulling the latest tag from the Docker image path. 

!!! Note 
    Here we are recommending to use a different image path for the updated integration solution. Otherwise, (because the Docker image is re-pulled from the existing deployment) some traffic from outsiders might get lost. 
