# Quick Start with WSO2 Micro Integrator

Let's get started with WSO2 Micro Integrator by running a simple use
case on your local environment. In this example, we use a REST API to
simulate a simple HTTP service ( **HelloWorld** service) deployed in an
instance of WSO2 Micro Integrator. You can deploy this HelloWorld
service on a **Docker** container or on **Kubernetes** . When invoked,
the **HelloWorld** service will return the following response:
`         {"Hello":"World"}        `

<a href=""><img src="../../assets/img/micro-integrator-01.png"></a>

### Set up the workspace

-   Install [Docker](https://www.docker.com/) (version 17.09.0-ce or
    higher).
-   Install [Curl](https://curl.haxx.se/) .
-   To set up the integration workspace for this quick guide, we will
    use an integration project that was built using [WSO2 Integration
    Studio](_WSO2_Integration_Studio_) :
    1.  Download the [project file](attachments/119133388/119137072.zip)
        for this guide, and extract it to a known location. Let's call
        this `            <MI_QSG_HOME>           ` .
    2.  Go to the `             <MI_GS_HOME>            ` directory .
        The following project files, and Docker/Kubernetes configuration
        files are available.

        <table>
        <tbody>
        <tr class="odd">
        <td>hello-world-config-project</td>
        <td><div class="content-wrapper">
        <p>This is the <strong>ESB Config Project</strong> folder with the integration artifacts (synapse artifacts) for the HelloWorld service. This service consists of the following REST API:</p>
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
        <strong>HelloWorld.xml</strong>
        </div>
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>&lt;api context=<span class="st">&quot;/hello-world&quot;</span> name=<span class="st">&quot;HelloWorld&quot;</span> xmlns=<span class="st">&quot;http://ws.apache.org/ns/synapse&quot;</span>&gt;</span>
        <span id="cb1-2"><a href="#cb1-2"></a>    &lt;resource methods=<span class="st">&quot;GET&quot;</span>&gt;</span>
        <span id="cb1-3"><a href="#cb1-3"></a>        &lt;inSequence&gt;</span>
        <span id="cb1-4"><a href="#cb1-4"></a>            &lt;payloadFactory media-type=<span class="st">&quot;json&quot;</span>&gt;</span>
        <span id="cb1-5"><a href="#cb1-5"></a>                &lt;format&gt;{<span class="st">&quot;Hello&quot;</span>:<span class="st">&quot;World&quot;</span>}&lt;/format&gt;</span>
        <span id="cb1-6"><a href="#cb1-6"></a>                &lt;args/&gt;</span>
        <span id="cb1-7"><a href="#cb1-7"></a>            &lt;/payloadFactory&gt;</span>
        <span id="cb1-8"><a href="#cb1-8"></a>            &lt;respond/&gt;</span>
        <span id="cb1-9"><a href="#cb1-9"></a>        &lt;/inSequence&gt;</span>
        <span id="cb1-10"><a href="#cb1-10"></a>        &lt;outSequence/&gt;</span>
        <span id="cb1-11"><a href="#cb1-11"></a>        &lt;faultSequence/&gt;</span>
        <span id="cb1-12"><a href="#cb1-12"></a>    &lt;/resource&gt;</span>
        <span id="cb1-13"><a href="#cb1-13"></a>&lt;/api&gt;</span></code></pre></div>
        </div>
        </div>
        </div></td>
        </tr>
        <tr class="even">
        <td>hello-world-config-projectCompositeApplication</td>
        <td><div class="content-wrapper">
        <p>This is the <strong>Composite Application Project</strong> folder, which contains the packaged CAR file of the HelloWorld service.</p>
        </div></td>
        </tr>
        <tr class="odd">
        <td>Dockerfile</td>
        <td><div class="content-wrapper">
        This Docker configuration file is configured to build a Docker image for WSO2 Micro Integrator with the HelloWorld service.<br />

        <div id="expander-1958317770" class="expand-container">
        <div id="expander-control-1958317770" class="expand-control">
        <img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> Dockerfile
        </div>
        <div id="expander-content-1958317770" class="expand-content">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>FROM wso2/micro-integrator:<span class="fl">1.0.</span><span class="dv">0</span></span>
        <span id="cb2-2"><a href="#cb2-2"></a></span>
        <span id="cb2-3"><a href="#cb2-3"></a>COPY hello-world-config-projectCompositeApplication/target/hello-world-config-projectCompositeApplication_<span class="fl">1.0.0.</span>car /home/wso2carbon/wso2mi/repository/deployment/server/carbonapps</span></code></pre></div>
        </div>
        </div>
        </div>
        </div>
        <p><strong>Note</strong> that this file is configured to use the community version of the WSO2 Micro Integrator base Docker image (from <a href="https://hub.docker.com/r/wso2/micro-integrator">DockerHub</a> ). If you want to use the Micro Integrator that includes the latest product updates, you can update the image name in this Docker file as explained <a href="Installing-WSO2-Micro-Integrator_119133387.html#InstallingWSO2MicroIntegrator-RunningtheMicroIntegratoroncontainers">here</a> .</p>
        </div></td>
        </tr>
        <tr class="even">
        <td>k8s-deployment.yaml</td>
        <td><div class="content-wrapper">
        <p>This is sample Kubernetes configuration file that is configured to deploy WSO2 Micro Integrator in a Kubernetes cluster.</p>
        <div id="expander-2010680575" class="expand-container">
        <div id="expander-control-2010680575" class="expand-control">
        <img src="images/icons/grey_arrow_down.png" class="expand-control-image" /> k8s-deployment.yaml
        </div>
        <div id="expander-content-2010680575" class="expand-content">
        <div class="code panel pdl" style="border-width: 1px;">
        <div class="codeContent panelContent pdl">
        <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>apiVersion: apps/v1</span>
        <span id="cb3-2"><a href="#cb3-2"></a>kind: Deployment</span>
        <span id="cb3-3"><a href="#cb3-3"></a>metadata:</span>
        <span id="cb3-4"><a href="#cb3-4"></a>  name: mi-helloworld-deployment</span>
        <span id="cb3-5"><a href="#cb3-5"></a>  labels:</span>
        <span id="cb3-6"><a href="#cb3-6"></a>    event: mi-helloworld</span>
        <span id="cb3-7"><a href="#cb3-7"></a>spec:</span>
        <span id="cb3-8"><a href="#cb3-8"></a>  strategy:</span>
        <span id="cb3-9"><a href="#cb3-9"></a>    type: Recreate</span>
        <span id="cb3-10"><a href="#cb3-10"></a>  replicas: <span class="dv">2</span></span>
        <span id="cb3-11"><a href="#cb3-11"></a>  selector:</span>
        <span id="cb3-12"><a href="#cb3-12"></a>    matchLabels:</span>
        <span id="cb3-13"><a href="#cb3-13"></a>      event: mi-helloworld</span>
        <span id="cb3-14"><a href="#cb3-14"></a>  template:</span>
        <span id="cb3-15"><a href="#cb3-15"></a>    metadata:</span>
        <span id="cb3-16"><a href="#cb3-16"></a>      labels:</span>
        <span id="cb3-17"><a href="#cb3-17"></a>        event: mi-helloworld</span>
        <span id="cb3-18"><a href="#cb3-18"></a>    spec:</span>
        <span id="cb3-19"><a href="#cb3-19"></a>      containers:</span>
        <span id="cb3-20"><a href="#cb3-20"></a>      -</span>
        <span id="cb3-21"><a href="#cb3-21"></a>        image: wso2-mi-hello-world</span>
        <span id="cb3-22"><a href="#cb3-22"></a>        name: helloworld</span>
        <span id="cb3-23"><a href="#cb3-23"></a>        imagePullPolicy: IfNotPresent</span>
        <span id="cb3-24"><a href="#cb3-24"></a>        ports:</span>
        <span id="cb3-25"><a href="#cb3-25"></a>        -</span>
        <span id="cb3-26"><a href="#cb3-26"></a>          name: web</span>
        <span id="cb3-27"><a href="#cb3-27"></a>          containerPort: <span class="dv">8290</span> </span>
        <span id="cb3-28"><a href="#cb3-28"></a>---</span>
        <span id="cb3-29"><a href="#cb3-29"></a>apiVersion: v1</span>
        <span id="cb3-30"><a href="#cb3-30"></a>kind: <span class="bu">Service</span></span>
        <span id="cb3-31"><a href="#cb3-31"></a>metadata:</span>
        <span id="cb3-32"><a href="#cb3-32"></a>  name: mi-helloworld-service</span>
        <span id="cb3-33"><a href="#cb3-33"></a>  labels:</span>
        <span id="cb3-34"><a href="#cb3-34"></a>    event: mi-helloworld</span>
        <span id="cb3-35"><a href="#cb3-35"></a>spec:</span>
        <span id="cb3-36"><a href="#cb3-36"></a>  type: NodePort</span>
        <span id="cb3-37"><a href="#cb3-37"></a>  ports:</span>
        <span id="cb3-38"><a href="#cb3-38"></a>    -</span>
        <span id="cb3-39"><a href="#cb3-39"></a>      name: web</span>
        <span id="cb3-40"><a href="#cb3-40"></a>      port: <span class="dv">8290</span></span>
        <span id="cb3-41"><a href="#cb3-41"></a>      targetPort: <span class="dv">8290</span> </span>
        <span id="cb3-42"><a href="#cb3-42"></a>      nodePort: <span class="dv">32100</span></span>
        <span id="cb3-43"><a href="#cb3-43"></a>  selector:</span>
        <span id="cb3-44"><a href="#cb3-44"></a>    event: mi-helloworld</span></code></pre></div>
        </div>
        </div>
        </div>
        </div>
        </div></td>
        </tr>
        </tbody>
        </table>

### Run on Docker

Once you have [set up your
workspace](#QuickStartwithWSO2MicroIntegrator-Setuptheworkspace) , you
can run the HelloWorld service on Docker:

#### Build a Docker image

Open a terminal, navigate to the `         <MI_QSG_HOME>        `
directory (which stores the Dockerfile), and execute the following
command to build a Docker image with WSO2 Micro Integrator and the
integration artifacts.

``` java
    docker build -t hello_world_docker_image .
```

This command executes the following tasks:

1.  The base Docker image of WSO2 Micro Integrator is downloaded from
    **DockerHub** and a custom, deployable Docker image of the Micro
    Integrator is created in a Docker container.
2.  The CAR file with the integration artifacts that define the
    HelloWorld service is deployed.

#### Run Docker container

From the `         <MI_QSG_HOME>        ` directory, execute the
following command to start a Docker container for the Micro Integrator.

``` java
    docker run -d -p 8290:8290 hello_world_docker_image
```

The Docker container with the Micro Integrator is started.

#### Invoke the Micro Integrator (on Docker)

Open a terminal, and execute the following command to invoke the
HelloWorld service:  

``` java
    curl http://localhost:8290/hello-world
```

Upon invocation, you should be able to observe the following response:

``` java
    {"Hello":"World"}
```

### Run on Kubernetes

Once you have [set up your
workspace](#QuickStartwithWSO2MicroIntegrator-Setuptheworkspace) , you
can run the HelloWorld service on Kubernetes:

#### Install Minikube

Let's use [Minikube](https://github.com/kubernetes/minikube) to run a
Kubernetes cluster for this example.

1.  [Install
    Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)
    . Once you have completed this step, you should have **kubectl**
    configured to use Minikube from your terminal.
2.  Start Minikube from your terminal:

    ``` java
            minikube start
    ```

3.  Execute the command given below to start using Minikube's built-in
    Docker daemon. You need this Docker daemon to be able to create
    Docker images for the Minikube environment.

    ``` java
            eval $(minikube docker-env)
    ```

Now you can build a Docker image for your HelloWorld service in Minikube
as explained below.

#### Build a Docker image

Open a terminal, navigate to the `         <MI_QSG_HOME>        `
directory (which stores the Dockerfile), and execute the following
command to build a Docker image (with WSO2 Micro Integrator and the
integration artifacts) **in the Minikube environment** .

``` java
    docker build -t wso2-mi-hello-world .
```

#### Run container (on Minikube)

Follow the steps given below to start a Docker container for Docker
image on Minikube.

1.  Navigate to the `           <MI_QSG_HOME>          ` directory
    (which stores the `           k8s-deployment.yaml          ` file),
    and execute the following command:

    ``` java
            kubectl create -f k8s-deployment.yaml
    ```

2.  Check whether all the Kubernetes artifacts are deployed successfully
    by executing the following command:  

    ``` java
            kubectl get all
    ```

    You will get a result similar to the following. Be sure that the
    deployment is in 'Running' state.

    ``` java
            NAME                                            READY   STATUS    RESTARTS   AGE
            pod/mi-helloworld-deployment-56f58c9676-djbwh   1/1     Running   0          14m
            pod/mi-helloworld-deployment-56f58c9676-xj4fq   1/1     Running   0          14m
        
            NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
            service/kubernetes              ClusterIP   10.96.0.1       <none>        443/TCP          25m
            service/mi-helloworld-service   NodePort    10.110.50.146   <none>        8290:32100/TCP   14m
            NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
            deployment.apps/mi-helloworld-deployment   2/2     2            2           14m
        
            NAME                                                  DESIRED   CURRENT   READY   AGE
            replicaset.apps/mi-helloworld-deployment-56f58c9676   2         2         2       14m
    ```

#### Invoke the Micro Integrator (on Minikube)

Open a terminal, and execute the command given below to invoke the
HelloWorld service. Be sure to replace MINIKUBE\_IP with the IP of your
Minikube installation.

``` java
    curl http://MINIKUBE_IP:32100/hello-world
```

Upon invocation, you should be able to observe the following response:

``` java
    {"Hello":"World"}
```
