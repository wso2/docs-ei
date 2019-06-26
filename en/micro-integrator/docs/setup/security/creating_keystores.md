# Creating New keystores
WSO2 Carbon-based products are shipped with a default keystore named  wso2carbon.jks , which is stored in the  <PRODUCT_HOME>/repository/resources/security  directory.

There are two ways to create keystores for a WSO2 product. You can either generate a keystore using an already existing public key certificate (CA-signed), or you can create the public key certificate at the time of generating the keystore. See the instructions given below.

## Creating a keystore using an existing certificate
Secure Sockets Layer (SSL) is a protocol that is used to secure communication between systems. This protocol uses a public key, a private key and a random symmetric key to encrypt data. As SSL is widely used in many systems, certificates may already exist that can be reused. In such situations, you can use an already existing CA-signed certificate to generate your keystore for SSL by using OpenSSL and Java keytool.

1. First, you must export certificates to the  PKCS12/PFX  format. Give strong passwords whenever required. In WSO2 products, it is a must to have the same password for both the keystore and private key.

    Execute the following command to export the entries of a trust chain into a keystore of .pfx format:

    ```
    openssl pkcs12 -export -in <certificate file>.crt -inkey <private>.key -name "<alias>" -certfile <additional certificate file> -out <pfx keystore name>.pfx
    ```

2. Convert the PKCS12/PFX formatted keystore to a Java keystore using the following command:

    ```
    openssl pkcs12 -export -in <certificate file>.crt -inkey <private>.key -name "<alias>" -certfile <additional certificate file> -out <pfx keystore name>.pfx
    ```

## Creating a new keystore using a new certificate
You can follow the steps in this section to create a new keystore with a private key and a new public key certificate. We will be using the keytool that is available with your JDK installation. Note that the pubic key certificate we generate for the keystore is self-signed. Therefore, if you need a public key certificate that is CA-signed, you need to generate a CA-signed certificate and import it to the keystore as explained in the next section. Alternatively, you can choose the option of generating a new keystore using a CA-signed public certificate as explained previously.

1. Open a command prompt and go to the <PRODUCT_HOME>/repository/resources/security/ directory. All keystores should be stored here.
2. Create the keystore that includes the private key by executing the following command:

    ```
    keytool -genkey -alias newcert -keyalg RSA -keysize 2048 -keystore newkeystore.jks -dname "CN=<testdomain.org>, OU=Home,O=Home,L=SL,S=WS,C=LK" -storepass mypassword -keypass mypassword
    ```
    This command will create a keystore with the following details:

    * Keystore name: newkeystore.jks
    * Alias of public certificate: newcert
    * Keystore password: mypassword
    * Private key password: mypassword (this is required to be the same as keystore password)

    > Note that if you did not specify values for the '-keypass' and the '-storepass' in the above command, you will be asked to give a value for the '-storepass' (password of the keystore). As a best practice, use a password generator to generate a strong password. You will then be asked to enter a value for -keypass. Click Enter because we need the same password for both the keystore and the key. Also, if you did not specify values for -dname, you will be asked to provide those details individually.

3. Open the <PRODUCT_HOME>/repository/resources/security/ directory and check if the new keystore file is created. Make a backup of it and move it to a secure location. This is important as it is the only place with your private key.

You now have a keystore (.jks file) with a private key and a self-signed public key certificate.
