# Adding SSL certificates to keystores

Now, let's look at how you can get a CA-signed certificate for your keystores. Note that you do not need to create a new keystore everytime you need add a CA-signed certificate.

## Step 1: Generating SSL certificates for keystore

First, you need to generate a certificate signing request (CSR) for your keystore (.jks file). This CSR file can then be certified by a certification authority (CA), which is an entity that issues digital certificates. These certificates certify the ownership of a public key.

1. Execute the following command to generate the CSR:
    ```
    keytool -certreq -alias certalias -file newcertreq.csr -keystore newkeystore.jks
    ```
    As mentioned before, use the same alias that you used during the keystore creation process. You will be asked to give the keystore password. Once the password is given, the command will output the newcertreq.csr file to the <PRODUCT_HOME>/repository/resources/security/ directory. This is the CSR that you must submit to a CA.

2. You must provide this CSR file to the CA. For testing purposes, try the 90 days trial SSL certificate from Comodo.

    >It is preferable to have a wildcard certificate or multiple domain certificates if you wish to have multiple subdomains like gateway.sampledomain.org , publisher.sampledomain.org , identity.sampledomain.org , etc., for the deployment. For such requirements, you must modify the CSR request by adding subject alternative names. Most of the SSL providers give instructions to generate the CSR in such cases.

3. After accepting the request, a signed certificate is provided along with a root certificate and several intermediate certificates (depending on the CA) as a bundle (.zip file).

    > **Sample certificates provided by the CA (Comodo)**

    >Root certificate of the CA: AddTrustExternalCARoot.crt  
    >Intermediate certificates:  COMODORSAAddTrustCA.crt, COMODORSADomainValidationSecureServerCA.crt  
    >SSL Certificate signed by CA: test_sampleapp_org.crt

## Step 2: Importing SSL certificates to a keystore

Follow the steps given below to import the CA-signed certificate to your keystore.

1. Before importing the CA-signed certificate to the keystore, you must add the root CA certificate and the two (related) intermediate certificates by executing the commands given below. Note that the sample certificates given above are used as examples.
    ```
    keytool -import -v -trustcacerts -alias ExternalCARoot -file AddTrustExternalCARoot.crt -keystore newkeystore.jks -storepass mypassword
    keytool -import -v -trustcacerts -alias TrustCA -file COMODORSAAddTrustCA.crt -keystore newkeystore.jks -storepass mypassword
    keytool -import -v -trustcacerts -alias SecureServerCA -file COMODORSADomainValidationSecureServerCA.crt -keystore newkeystore.jks -storepass mypassword
    ```
    Optionally we can append the -storepass <keystore password> option to avoid having to enter the password when prompted later in the interactive mode.

2. After you add the root certificate and all other intermediate certificates, add the CA-signed SSL certificate to the keystore by executing the following command:
    ```
    keytool -import -v -alias newcert -file <test_sampleapp_org.crt> -keystore newkeystore.jks -keypass mypassword -storepass mypassword
    ```
    In this command, use the same alias (i.e., 'newcert') that you used while creating the keystore

Now you have a Java keystore, which includes a CA-signed public key certificate that can be used for SSL in a production environment. Next, you may need to add the same CA-signed public key certificate to the client-truststore.jks file. This will provide security and trust for backend communication/inter-system communication of WSO2 products via SSL.

## Step 3: Importing SSL certificates to a truststore
In SSL handshake, the client needs to verify the certificate presented by the server. For this purpose, the client usually stores the certificates it trusts, in a trust store. To enable secure and trusted backend communication, all WSO2 products are shipped with a trust store named client-truststore.jks, which resides in the same directory as the default keystore (<PRODUCT_HOME>/repository/resources/security/).
Follow the steps given below to import the same CA-signed public key certificate (which you obtained in the previous step) into your WSO2 product's default truststore (client-truststore.jks).

1. Get a copy of the client-truststore.jks file from the <PRODUCT_HOME>/repository/resources/security/ directory.
2. Export the public key from your .jks file using the following command.
    ```
    keytool -export -alias certalias -keystore newkeystore.jks -file <public key name>.pem
    ```
3. Import the public key you extracted in the previous step to the client-truststore.jks file using the following command.
    ```
    keytool -import -alias certalias -file <public key name>.pem -keystore client-truststore.jks -storepass wso2carbon
    ```
    > Note that 'wso2carbon' is the keystore password of the default client-truststore.jks file.

Now, you have an SSL certificate stored in a Java keystore and a public key added to the client-truststore.jks file. Note that both these files should be in the <PRODUCT_HOME>/repository/resources/security/ directory. You can now replace the default wso2carbon.jks keystore in your product with the newly created keystore by updating the relevant configuration files in your product.
