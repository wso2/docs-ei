# Creating Custom Mediators

The ESB profile of WSO2 EI comes with an assortment of
[mediators](_ESB_Mediators_) to filter, transform, route and manipulate
messages. Mediators provide an easy way of extending the ESB.  When you
have a scenario that requires functionality not provided by the existing
mediators, you can write your own custom mediators to implement your
specific business requirements. Your custom mediators then must be
plugged into the ESB profile. After adding them to the ESB profile, they
function together with core mediators that come with the product. The
custom mediators can be distributed in a packaged form that can be
installed in another ESB profile instance.

-   [Writing an ESB
    Mediator](#CreatingCustomMediators-WritinganESBMediator)
-   [Building the
    mediator](#CreatingCustomMediators-Buildingthemediator)
-   [Deploying the custom
    mediator](#CreatingCustomMediators-Deployingthecustommediator)

### Writing an ESB Mediator

There are two ways of writing an ESB mediator:

-   **Using the** **[Class Mediator](_Class_Mediator_)** - This does not
    allow mediator specific XML configurations. See [Writing Custom
    Mediator Implementations](_Writing_Custom_Mediator_Implementations_)
    for more information.
-   **Writing the mediator with factory and serialize methods** - This
    allows mediator to have its own XML configuration. See [Writing
    Custom Configuration Implementations for
    Mediators](_Writing_Custom_Configuration_Implementations_for_Mediators_)
    for more information.

The easiest way to write a mediator is to extend your mediator class
from the
[`          org.apache.synapse.mediators.AbstractMediator         `](http://synapse.apache.org/apidocs/org/apache/synapse/mediators/AbstractMediator.html)
class. For example, you can see the following articles in the WSO2
library:

-   [Writing a Mediator in WSO2 EI - Part
    1](http://wso2.org/library/2898)
-   [Writing a Mediator in WSO2 EI - Part
    2](http://wso2.org/library/2936)

!!! tip

You can use the [Class mediator](_Class_Mediator_) and [custom
mediators](_Creating_Custom_Mediators_) for user-specific custom
developments when there is no built-in mediator that already provides
the required functionality. However, class and custom mediators incur a
high maintenance overhead. [Custom
mediators](_Creating_Custom_Mediators_) in particular might introduce
version migration complications when upgrading WSO2 EI. Therefore, avoid
using them unless  the scenario is frequently re-used and heavily
user-specific. For best results, use [WSO2 EI Tooling to
debug](_Debugging_Mediation_) Class and custom mediators.


### Building the mediator

After you write the mediator, you must build it and make it an OSGI
bundle so that it will work with the ESB.

##### Basic approach

Create a regular JAR that links to the Synpase core JAR and place it in
the `         <EI_HOME>/        ` `         lib        ` directory. The
platform will automatically make it an OSGI bundle and deploy it to the
server.

##### Advanced approach

If you want to control the way your mediator is created as an OSGI
bundle, you must write the POM files so that you can export and import
the packages you need, as shown in the examples below.

Following is a POM file that creates the mediator using [Class
Mediator](_Class_Mediator_) .

``` java
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    
        <modelVersion>4.0.0</modelVersion>
        <groupId>org.test</groupId>
        <artifactId>org.test</artifactId>
        <version>1.0.0</version>
        <packaging>bundle</packaging>
        <name>My Samples - Test mediator</name>
        <url>http://www.test.com</url>
    
        <repositories>
            <repository>
                <id>wso2-maven2-repository</id>
                <url>http://dist.wso2.org/maven2</url>
            </repository>
            <repository>
                <id>apache-Incubating-repo</id>
                <name>Maven Incubating Repository</name>
                <url>http://people.apache.org/repo/m2-incubating-repository</url>
            </repository>
            <repository>
                <id>apache-maven2-repo</id>
                <name>Apache Maven2 Repository</name>
                <url>http://repo1.maven.org/maven2/</url>
            </repository>
        </repositories>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>2.0</version>
                    <configuration>
                        <source>1.5</source>
                        <target>1.5</target>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.apache.felix</groupId>
                    <artifactId>maven-bundle-plugin</artifactId>
                    <version>1.4.0</version>
                    <extensions>true</extensions>
                    <configuration>
                        <instructions>
                            <Bundle-SymbolicName>org.test</Bundle-SymbolicName>
                            <Bundle-Name>org.test</Bundle-Name>
                            <Export-Package>
                                org.test.mediator.*,
                            </Export-Package>
                            <Import-Package>
                                *; resolution:=optional
                            </Import-Package>
                        </instructions>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    
        <dependencies>
            <dependency>
                <groupId>org.apache.synapse</groupId>
                <artifactId>synapse-core</artifactId>
                <version>2.1.1-wso2v5</version>
            </dependency>
        </dependencies>
    </project>
```

!!! info

The Maven bundle plug-in was used for creating the OSGI bundle here.
Make sure you export the correct package that contains the mediator
code. Otherwise, your mediator will not work. If you are adding
third-party libraries to the class mediator, be sure to use the
maven-shade-plugin to add the dependencies. See the sample shade plugin
given below.

![](images/icons/grey_arrow_down.png){.expand-control-image} Example
maven-shade-plugin

``` java
    <plugin>
            <artifactId>maven-shade-plugin</artifactId>
            <version></version>
            <executions>
              <execution>
                <phase>package</phase>
                <goals>
                  <goal>shade</goal>
                </goals>
                <configuration>
                  <artifactSet>
                    <includes>
                      <include></include>
                      <include></include>
                    </includes>
                  </artifactSet>
                </configuration>
              </execution>
            </executions>
            <configuration>
              <finalName></finalName>
            </configuration>
    </plugin>
```
    

Following is a POM file that creates the mediator with its own XML
configuration using the `         Serialize        ` and
`         Factory        ` classes.

``` java
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    
        <modelVersion>4.0.0</modelVersion>
        <groupId>org.test</groupId>
        <artifactId>org.test</artifactId>
        <version>1.0.0</version>
        <packaging>bundle</packaging>
        <name>My Samples - Test mediator</name>
        <url>http://www.test.com</url>
    
        <repositories>
            <repository>
                <id>wso2-maven2-repository</id>
                <url>http://dist.wso2.org/maven2</url>
            </repository>
            <repository>
                <id>apache-Incubating-repo</id>
                <name>Maven Incubating Repository</name>
                <url>http://people.apache.org/repo/m2-incubating-repository</url>
            </repository>
            <repository>
                <id>apache-maven2-repo</id>
                <name>Apache Maven2 Repository</name>
                <url>http://repo1.maven.org/maven2/</url>
            </repository>
        </repositories>
    
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>2.0</version>
                    <configuration>
                        <source>1.5</source>
                        <target>1.5</target>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.apache.felix</groupId>
                    <artifactId>maven-bundle-plugin</artifactId>
                    <version>1.4.0</version>
                    <extensions>true</extensions>
                    <configuration>
                        <instructions>
                            <Bundle-SymbolicName>org.test</Bundle-SymbolicName>
                            <Bundle-Name>org.test</Bundle-Name>
                            <Export-Package>
                                org.test.mediator.*,
                            </Export-Package>
                            <Import-Package>
                                *; resolution:=optional
                            </Import-Package>
                            <Fragment-Host>synapse-core</Fragment-Host>
                        </instructions>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    
        <dependencies>
            <dependency>
                <groupId>org.apache.synapse</groupId>
                <artifactId>synapse-core</artifactId>
                <version>2.1.1-wso2v5</version>
            </dependency>
        </dependencies>
    </project>
```

In this case, it is necessary to make the mediator an OSGI fragment of
the synapse-core bundler. To achieve this, use the
`         <Fragment-Host>synapse-core</Fragment-Host>        ` .

Create
`         <PROJECT_HOME>/main/resources/META-INF.services/org.apache.synapse.config.xml.MediatorFactory        `
and
`         <PROJECT_HOME>/main/resources/META-INF.services/org.apache.synapse.config.xml.MediatorSerializer        `
files with the following content as shown below, to add service provider
definitions to your Maven project.

![create the 2 text
files](attachments/4884105/52756984.png "create the 2 text files")

**Content of the org.apache.synapse.config.xml.MediatorFactory file**

``` text
    org.wso2.carbon.mediator.cache.config.xml.CacheMediatorFactory 
```

**Content of the org.apache.synapse.config.xml.MediatorSerializer file**

``` text
    org.wso2.carbon.mediator.cache.config.xml.CacheMediatorSerializer  
```

### Deploying the custom mediator

For the above example, after you create the mediator, place the JAR file
in the `         <EI_HOME>\dropins        ` directory. However, there
are three places inside the WSO2 EI distribution for placing the JAR
file of a customer mediator. They are:

-   `          <EI_HOME>\extensions         `
-   `          <EI_HOME>\dropins         `
-   `          <EI_HOME>\lib         `

!!! tip

The recommended way is to copy the JAR files into the
`         extensions        ` directory. If you deploy a mediator
through a Carbon Application (CAR file), it can only be accessed from
the Sequences and Proxy Services within the same CAR file. You need to
restart the server after copying the JAR file of the custom mediator
into any of these directories.


#### `         Extensions        ` directory

If you created a regular non-OSGI mediator, build it and copy the JAR
file into this directory. The system will convert the JAR file of the
mediator into an OSGI-JAR and deploy it into the server. This way is
easy and simple.

However, in this method, you cannot use any OSGI features within the
mediator implementation. For example, you cannot make certain packages
private or import specific versions of packages. Also, in this method,
other than Class mediators, you can use a mediator, which has its own
XML configuration.

#### `         Dropins        ` directory

If your custom mediator is a Class mediator, it would be a normal
bundle. Instead, if it is a mediator with an XML configuration, then it
should be a fragment of the Synapse core, and thereby, you need to
create an OSGI bundle for that. This requires basic knowledge about OSGI
and the Maven bundle plug-in. If you created your custom mediator as an
OSGI bundle, you can place its JAR file in the
`         dropins        ` directory.

The benefit of creating the custom mediator as an OSGI bundle is
that you can use the OSGI features in its implementation as preferred.

#### `         Lib        ` directory

You can only copy JAR files of custom mediators, which are created using
Class mediators into this directory. If you copy a regular JAR file into
this directory, the system automatically converts it to an OSGI bundle.
