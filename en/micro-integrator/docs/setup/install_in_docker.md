
# Installing WSO2 EI on Docker

To run your Micro Integrator solutions on Docker or Kubernetes, you need
to first create an **immutable** docker image with the required synapse
artifacts, configurations, and third-party dependencies by using the
base Docker image of WSO2 Micro Integrator. You can then deploy and run
the immutable images on Docker or Kubernetes. One advantage of having an
immutable docker image is that you can easily implement a CI/CD pipeline
to systematically test the solution before deploying in production.

-   The Micro Integrator Docker image (with the latest products) is
    available in the [WSO2 Docker Registry](https://docker.wso2.com/) .
    **Note** that you need a valid WSO2 subscription to use the Docker
    image with updates. Therefore, you need to provide your log in
    credentials when downloading the Docker image. If you do not already
    have a subscription, you can get a [free trial
    subscription](https://wso2.com/subscription/free-trial) .

    **Micro Integrator Docker image (with updates)**

    ``` java
    docker.wso2.com/micro-integrator:1.0.0
    ```

    **Log in to WSO2 Docker Registry**

    ``` java
    docker login docker.wso2.com
    ```

-   The community version of Micro Integrator's base Docker image is
    available on
    [DockerHub](https://hub.docker.com/r/wso2/micro-integrator) .

    **Base Docker Image and Tag (community version)**

    ``` java
    wso2/micro-integrator:1.0.0
    ```

Given below are the basic steps you need to follow to run the Micro
Integrator on Docker:

1.  **[Export the integration
    artifacts](Working-with-WSO2-Integration-Studio_119133415.html#WorkingwithWSO2IntegrationStudio-ExportingtheESBartifacts)**
    into a CAR file.

2.  **Create the Dockerfile** as shown below. This file contains
    instructions to download the base Docker image of WSO2 Micro
    Integrator from DockerHub (community version) or the WSO2 Docker
    Registry (includes updates), and to copy the integration artifacts
    to the Micro Integrator.  

    ``` java
    FROM <docker_image_name>:1.0.0
    COPY <directoy_path>/<capp_name> /home/wso2ei/wso2mi/repository/deployment/server/carbonapps
    ```
    The information specified in the Docker file is as follows:

    <table>
    <tbody>
    <tr class="odd">
    <td>FROM</td>
    <td><div class="content-wrapper">
    <p>The 'FROM' tag in the docker file specifies the WSO2 Micro Integrator version that should be downloaded. You can use the updated Docker image or the community version as shown below. The version is 1.0.0 of the WSO2 Micro Integrator. If required, you can use an earlier version by replacing 'latest' with the version number.</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 1: Docker image with updates</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb1" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb1-1"><a href="#cb1-1"></a>FROM docker.<span class="fu">wso2</span>.<span class="fu">com</span>/micro-integrator:<span class="fl">1.0.</span><span class="dv">0</span></span></code></pre></div>
    </div>
    </div>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 2: Docker image without updates (community version)</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb2" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb2-1"><a href="#cb2-1"></a>FROM wso2/micro-integrator:<span class="fl">1.0.</span><span class="dv">0</span></span></code></pre></div>
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
    <div class="sourceCode" id="cb3" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb3-1"><a href="#cb3-1"></a>COPY carbonapps /home/wso2ei/wso2mi/repository/deployment/server/carbonapps</span></code></pre></div>
    </div>
    </div>
    <p>If you have multiple composite application that you want to deploy in Docker using a single Docker image, add another entry to the Dockerfile. For example:</p>
    <div class="code panel pdl" style="border-width: 1px;">
    <div class="codeHeader panelHeader pdl" style="border-bottom-width: 1px;">
    <strong>Example 2</strong>
    </div>
    <div class="codeContent panelContent pdl">
    <div class="sourceCode" id="cb4" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence" style="brush: java; gutter: false; theme: Confluence"><pre class="sourceCode java"><code class="sourceCode java"><span id="cb4-1"><a href="#cb4-1"></a>COPY carbonapps /home/wso2ei/wso2mi/repository/deployment/server/carbonapps</span>
    <span id="cb4-2"><a href="#cb4-2"></a>COPY &lt;sample_carbon_app&gt; /home/wso2ei/wso2mi/repository/deployment/server/carbonapps</span></code></pre></div>
    </div>
    </div>
    </div></td>
    </tr>
    </tbody>
    </table>

3.  **Create an immutable Docker image** for your integration artifacts
    on WSO2 Micro Integrator by executing the following command from the
    location of your Dockerfile.

    ``` java
            docker build -t sample_docker_image .
    ```

4.  **Start a Docker container** by running the Docker image as shown
    below.

    ``` java
            docker run -d -p 8290:8290 sample_docker_image
    ```