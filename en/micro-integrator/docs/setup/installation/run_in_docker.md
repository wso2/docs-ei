
# Run WSO2 Micro Integrator on Docker

To run your Micro Integrator solutions on Docker or Kubernetes, you need
to first create an **immutable** docker image with the required synapse
artifacts, configurations, and third-party dependencies by using the
base Docker image of WSO2 Micro Integrator. You can then deploy and run
the immutable images on Docker or Kubernetes. One advantage of having an
immutable docker image is that you can easily implement a CI/CD pipeline
to systematically test the solution before deploying in production.

## Base Docker Images

Two types of base Docker images are available for the Micro Integrator:

-   The Micro Integrator Docker image (with the latest updates) is
    available in the [WSO2 Docker Registry](https://docker.wso2.com/).
    
    **Note** that you need a valid WSO2 subscription to use the Docker
    image with updates. Therefore, you need to provide your log in
    credentials when downloading the Docker image. If you do not already
    have a subscription, you can get a [free trial subscription](https://wso2.com/subscription/free-trial).

    **Micro Integrator Docker image (with updates)**

    ```bash
    docker.wso2.com/micro-integrator:1.1.0
    ```

    **Log in to WSO2 Docker Registry**

    ```bash
    docker login docker.wso2.com
    ```

-   The community version of WSO2 Micro Integrator's base Docker image is
    available on [DockerHub](https://hub.docker.com/r/wso2/micro-integrator).

    **Base Docker Image and Tag (community version)**

    ```bash
    wso2/micro-integrator:1.1.0
    ```

## Run on Docker using WSO2 Integration Studio

Let's create a Docker project for an integration solution and run it on a Docker container. You will use [WSO2 Integration Studio](../../../develop/WSO2-Integration-Studio) and the [base Docker image](#base-docker-images-wso2-micro-integrator) of the Micro Integrator for this process.

1.  [Create the proxy service](../../../develop/creating-artifacts/creating-a-proxy-service) with the following configuration:

    - Synapse configuration

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <proxy name="HelloWorld" startOnLoad="true" transports="http https" xmlns="http://ws.apache.org/ns/synapse">
            <target>
                <inSequence>
                    <payloadFactory media-type="json">
                        <format>{"Hello":"World"}</format>
                        <args/>
                    </payloadFactory>
                    <respond/>
                </inSequence>
                <outSequence/>
                <faultSequence/>
            </target>
        </proxy>
        ```

    - Graphical view
       ![Sample Proxy Service](../../assets/img/sample-proxy-service.png)

2.  You can now [create a Docker project](../../develop/create-docker-project.md) that includes the Synapse configuration given above.
3.  Use the following details:
    ```bash
    Target Repository : sampleproxy
    Target Tag : 1.0.0
    ```

4.  Execute the following command:
    ```bash
    docker image ls
    ``` 

    The list of currently available Docker images are displayed in the terminal similar to the example given below. The Docker image you created is included in this list as shown below.

    ```bash
    REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
    sampleproxy             1.0.0               49092809b36a        10 minutes ago      315MB
    wso2/micro-integrator   1.1.0               088477c689f6        2 days ago          315MB
    ```
    
5.  Start the container with the following command:

    ```bash
    docker run -d -p 8290:8290 sampleproxy:1.0.0
    ```

    If you want to use the [micro integrator dashboard](../../administer-and-observe/working-with-monitoring-dashboard.md) for monitoring, use following docker command:

    ```bash
    docker run -p 8290:8290 -p 9164:9164 -e   JAVA_OPTS="-DenableManagementApi=true" sampleproxy:1.0.0
    ```

6.  Invoke the proxy service as follows:

    ```bash
    curl http://localhost:8290/services/HelloWorld -XGET
    ```

    You will see the following response:
     
    ```bash
    {"Hello":"World"}
    ```

## Create Docker files manually

If you already have **packaged integration artifacts** in a CAR file, you can manually create the Docker files and deploy on Docker. Follow the steps given below.

!!! Tip
    **Before you begin**: Use WSO2 Integration Studio to [create the integration artifacts](../../../develop/intro-integration-development/#develop_artifacts) and then [export the integration artifacts](../../develop/exporting-artifacts.md) into a CAR file.

1.  **Create the Dockerfile** as shown below. This file contains
    instructions to download the base Docker image of WSO2 Micro
    Integrator from DockerHub (community version) or the WSO2 Docker
    Registry (includes updates), and to copy the integration artifacts
    to the Micro Integrator.  

    The **Dockerfile**:

    ```bash
    FROM <docker_image_name>:1.1.0
    COPY <directoy_path>/<capp_name> /home/wso2mi/repository/deployment/server/carbonapps
    ```
    The information specified in the Docker file is as follows:

    <table>
    <tbody>
    <tr>
        <th>Parameter</th>
        <th>Description</th>
    </tr>
    <tr class="odd">
    <td>FROM</td>
    <td><div class="content-wrapper">
    <p>The 'FROM' tag in the docker file specifies the WSO2 Micro Integrator version that should be downloaded. You can use the updated Docker image or the community version as shown below. The version is 1.1.0 of the WSO2 Micro Integrator. If required, you can use an earlier version by replacing 'latest' with the version number.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 1: Docker image with updates</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>FROM docker.<span class="fu">wso2</span>.<span class="fu">com</span>/micro-integrator:<span class="fl">1.1.</span><span class="dv">0</span></span></code></pre></div>
    </div>
    </div>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 2: Docker image without updates (community version)</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>FROM wso2/micro-integrator:<span class="fl">1.1.</span><span class="dv">0</span></span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    <tr class="even">
    <td>COPY</td>
    <td><div class="content-wrapper">
    <p>The 'COPY' tag in the docker file specifies the directory path to your composite application, followed by the location in your Docker instance to which the composite application should be copied.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 1</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>COPY carbonapps /home/wso2mi/repository/deployment/server/carbonapps</span></code></pre></div>
    </div>
    </div>
    <p>If you have multiple composite application that you want to deploy in Docker using a single Docker image, add another entry to the Dockerfile. For example:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 2</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>COPY carbonapps /home/wso2mi/repository/deployment/server/carbonapps</span>
    <span id="cb4-2"><a href="#cb4-2"></a>COPY &lt;sample_carbon_app&gt; /home/wso2mi/repository/deployment/server/carbonapps</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

2.  **Create an immutable Docker image** for your integration artifacts on WSO2 Micro Integrator by executing the following command from the location of your Dockerfile.

    ```bash
    docker build -t sample_docker_image .
    ```

3.  **Start a Docker container** by running the Docker image as shown below.

    ```bash
    docker run -d -p 8290:8290 sample_docker_image
    ```
