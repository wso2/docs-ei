# Installing Stream Processor Using AWS Cloud Formation

To install WSO2 Stream Processor using AWS Cloud Formation, follow the
steps below:

1.  Create and upload an SSL certificate into AWS, which is required to
    initiate the SSL handshake for HTTPS.
2.  Create a key pair for the desired region, which is required to SSH
    into instances. Continue to the next step if you want to use an
    existing key pair.
3.  After selecting the preferred template in the preferred region, you
    should land on the AWS CloudFormation **Select Template** page.
4.  Fill in the parameter details and click the **Create Stack** . Enter
    valid WUM credentials in the CloudFormation template to deploy the
    product with the latest updates. Leave it blank to deploy the GA
    version of the product.

        !!! info
    
        Creating the stack takes several minutes (\~20 minutes).  It may
        also take around 15 more minutes for the services to become active.
    
