# Installing Streaming Integrator Using Kubernetes
WSO2 Streaming Integrator can be deployed natively on kubernetes using Siddhi Kubernetes Operator.

Streaming Integrator can be configured in SiddhiProcess yaml and passed to the CRD(Custom Resource Definition) for deployment.
Siddhi logic can be directly written inline in SiddhiProcess yaml or passed as .siddhi files via config maps.

To install WSO2 Streaming Integrator using Kubernetes, follow the steps
below:

### Prerequisites

* A Kubernetes cluster v1.10.11 or higher.
    * [Minikube](https://github.com/kubernetes/minikube#installation)
    * [Google Kubernetes Engine(GKE) Cluster](https://console.cloud.google.com/)
    * [Docker for Mac](https://docs.docker.com/docker-for-mac/install/)
    * Or any other Kubernetes cluster.
* Admin privilages to install Siddhi operator.

#### Minikube

Siddhi operator automatically creates NGINX ingress. Therefore it to work we can either enable ingress on Minikube using the following command.
```
minikube addons enable ingress
```
or disable Siddhi operator's [automatically ingress creation](https://siddhi.io/en/v5.0/docs/siddhi-as-a-kubernetes-microservice/#deploy-siddhi-apps-without-ingress-creation).

#### Google Kubernetes Engine Cluster

To install Siddhi operator, you have to give cluster admin permission to your account. In order to do that execute the following command (by replacing "your-address@email.com" with your account email address).

```
kubectl create clusterrolebinding user-cluster-admin-binding --clusterrole=cluster-admin --user=your-address@email.com
```


#### Docker For Mac

Siddhi operator automatically creates NGINX ingress. Therefore it to work we can either enable ingress on Docker for mac following the official [documentation](https://kubernetes.github.io/ingress-nginx/deploy/#docker-for-mac) or disable Siddhi operator's [automatically ingress creation](https://siddhi.io/en/v5.0/docs/siddhi-as-a-kubernetes-microservice/#deploy-siddhi-apps-without-ingress-creation).


## Install Siddhi Operator for Streaming Integrator

To install the Siddhi Kubernetes operator for streaming integrator run the following commands.

```
kubectl apply -f https://github.com/wso2/streaming-integrator/releases/download/v1.0.0-m1/00-prereqs.yaml
kubectl apply -f https://github.com/wso2/streaming-integrator/releases/download/v1.0.0-m1/01-siddhi-operator.yaml
```

You can verify the installation by making sure the following deployments are running in your Kubernetes cluster.
```
$ kubectl get deployment

NAME                 DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
streaming-integrator   1         1         1            1           1m
siddhi-parser          1         1         1            1           1m
```

## Deploy Streaming Integrator

Siddhi app which consists the streaming integration logic can be deployed on kuberbetes using the siddhi operator.

Here we will creating a very simple Siddhi stream processing application that consumes events via HTTP, filers the input events on the type 'monitored' and logs the output on the console. This can be created using a SiddhiProcess yaml file as given below.

<script src="https://gist.github.com/AnuGayan/f11e235ee0dbc3df94126820f7bad282.js"></script>

To change the default configurations in streaming integrator you can provide the configuration that needed to overide through SiddhiProcess yaml

Here we have updated the above SiddhiProcess yaml to have the modified configuration for auth.configs in order to change the default configurations in streaming integrator.

!!! info 
    we can overide the default deployment.yaml file by using this appoach

<script src="https://gist.github.com/AnuGayan/249a3363a8aa47c6ae2d760d19ee5cad.js"></script>


### Invoke Siddhi Applications

To invoke the Siddhi Apps, first obtain the external IP of the ingress load balancer using `kubectl get ingress` command as follows and then, add the host siddhi and related external IP (ADDRESS) to the /etc/hosts file in your machine.

```
$ kubectl get ingress
NAME      HOSTS     ADDRESS     PORTS     AGE
siddhi    siddhi    10.0.2.15    80       14d
```

!!! info "Minikube"
    For Minikube, you have to use Minikube IP as the external IP. Hence, run minikube ip command to get the IP of the Minikube cluster.

Use the following CURL command to send events to monitor-app deployed in Kubernetes.

```
curl -X POST \
  http://siddhi/streaming-integrator-0/8080/checkPower \
    -H 'Accept: */*' \
    -H 'Content-Type: application/json' \
    -H 'Host: siddhi' \
    -d '{
        "deviceType": "dryer",
        "power": 600
        }'
```    
To monitor the associated logs for the above siddhi app, list down the available pods by executing the following commond.

```
$ kubectl get pods

NAME                                        READY    STATUS    RESTARTS    AGE
streaming-integrator-app-0-b4dcf85-npgj7     1/1     Running      0        165m
streaming-integrator-5f9fcb7679-n4zpj        1/1     Running      0        173m
```

and then execute the following command in order to monitor the logs of the relevent pod.

```
 kubectl logs -f streaming-integrator-app-0-b4dcf85-npgj7
```

!!! info 
    For more details regarding the Siddhi Operator, Please refer to the following [document](https://siddhi.io/en/v5.0/docs/siddhi-as-a-kubernetes-microservice/#!).