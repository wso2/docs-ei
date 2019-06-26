# Setting up a Load balancer

The load balancer automatically distributes incoming traffic across
multiple WSO2 product instances. It enables you to achieve greater
levels of fault tolerance in your cluster and provides the required
balancing of load needed to distribute traffic.

## Before you begin

Note the following:

* These configurations are not required if your clustering setup does not have a load balancer.
* The load balancer ports of the deployment pattern that is shown above are HTTP 80 and HTTPS 443. If your system uses any other ports, be sure to replace 80 and 443 values with the corresponding ports when you follow the configuration steps in this section.
* The load balancer directs requests to the server on a round robin basis. For example, the load balancer will direct requests to node 1 (` xxx.xxx.xxx.xx1`) of the ESB cluster as follows:
    * HTTP requests will be directed to node 1 using the
        `             http://xxx.xxx.xxx.xx1/<service>            ` URL
        via HTTP 80 port.
    * HTTPS requests will be directed to node 1 using the
        `             https://xxx.xxx.xxx.xx1/<service>            ` URL
        via HTTPS 443 port.
    * The management console of node 1 will be accessed using the
        `             https://xxx.xxx.xxx.xx1/carbon/            ` URL
        via HTTPS 443 port.

!!! Tip ""
	It is recommended to use NGINX Plus as your load balancer of choice.

## Configuring the load balancer

Follow the steps below to configure [NGINX
Plus](https://www.nginx.com/products/) version 1.7.11 or [NGINX
community](http://nginx.org/) version 1.9.2 as the load balancer.

1.  Install NGINX Plus or the NGINX community version on your cluster
    network.
2.  Create a VHost file named ei.http.conf in the /etc/nginx/conf.d directory and add the
    following configurations. This configures NGINX Plus to direct the HTTP requests to the two
    ESB nodes (xxx.xxx.xxx.xx1 and xxx.xxx.xxx.xx2) via the HTTP 80 port using
    the http://ei.wso2.com/ URL. If you are setting up NGINX on a Mac OS, you will not have the conf.d directory. 
    
    > Follow the steps given below to add the VHost files mentioned in this step and the preceding steps: 1. Create a directory named conf in the nginx directory, and create the ei.http.conf file in it. 2. Open the nginx/nginx.conf file and add the following entry before the final }. This includes all the files in the conf directory into the NGINX server: `include conf/*.conf;`

		```
		upstream ssl.wso2.ei.com {
		server xxx.xxx.xxx.xx1:8243;
		server xxx.xxx.xxx.xx2:8243;
		ip_hash;
		}  
		server {
			listen 443;
			server_name ei.wso2.com;
			ssl on;
			ssl_certificate /etc/nginx/ssl/server.crt;
			ssl_certificate_key /etc/nginx/ssl/server.key;
			location / {
				proxy_set_header X-Forwarded-Host $host;
				proxy_set_header X-Forwarded-Server $host;
				proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
				proxy_set_header Host $http_host;
				proxy_read_timeout 5m;
				proxy_send_timeout 5m;
				proxy_pass https://ssl.wso2.ei.com;  
				proxy_http_version 1.1;
				proxy_set_header Upgrade $http_upgrade;
				proxy_set_header Connection "upgrade";
					}
				}
		```

3. Create a VHost file (ei.https.conf) in the nginx/conf.d directory or in the nginx/conf directory if you are on a Mac OS and add the following configurations. This configures NGINX Plus to direct the HTTPS requests to the two ESB nodes (xxx.xxx.xxx.xx1 and xxx.xxx.xxx.xx2) via the HTTPS 443 port using the https://ei.wso2.com/ URL.
	* NGINX Community version
	```
	upstream ssl.wso2.ei.com {
	server xxx.xxx.xxx.xx1:8243;
	server xxx.xxx.xxx.xx2:8243;
	ip_hash;
	}  
	server {
		listen 443;
		server_name ei.wso2.com;
		ssl on;
		ssl_certificate /etc/nginx/ssl/server.crt;
		ssl_certificate_key /etc/nginx/ssl/server.key;
		location / {
			proxy_set_header X-Forwarded-Host $host;
			proxy_set_header X-Forwarded-Server $host;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header Host $http_host;
			proxy_read_timeout 5m;
			proxy_send_timeout 5m;
			proxy_pass https://ssl.wso2.ei.com;  
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
				}
			}
	```
	* NGINX Plus
	```
	upstream ssl.wso2.ei.com {
    server xxx.xxx.xxx.xx1:8243;
    server xxx.xxx.xxx.xx2:8243;
            sticky learn create=$upstream_cookie_jsessionid
            lookup=$cookie_jsessionid
            zone=client_sessions:1m;
					}

					server {
						listen 443;
    		server_name ei.wso2.com;
    	ssl on;
    	ssl_certificate /etc/nginx/ssl/server.crt;
    	ssl_certificate_key /etc/nginx/ssl/server.key;
    	location / {
				proxy_set_header X-Forwarded-Host $host;
				proxy_set_header X-Forwarded-Server $host;
				proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
				proxy_set_header Host $http_host;
				proxy_read_timeout 5m;
				proxy_send_timeout 5m;
               proxy_pass https://ssl.wso2.ei.com;

               proxy_http_version 1.1;
               proxy_set_header Upgrade $http_upgrade;
               proxy_set_header Connection "upgrade";
        }
			}
	```
4. Configure NGINX to access the management console as https://ui.ei.wso2.com/carbon via the HTTPS 443 port. To do this, create a VHost file ( ui.ei.https.conf ) in the nginx/conf.d/ directory or in the nginx/conf directory if you are on a Mac OS and add the following configurations into it. Make sure that the SSL files you create in step 6 are in the /etc/nginx/ssl/ directory. If it is not, update the path given below.

	```
	server {
    listen 443;
    server_name ui.ei.wso2.com;
    ssl on;
    ssl_certificate /etc/nginx/ssl/server.crt;
    ssl_certificate_key /etc/nginx/ssl/server.key;

    location / {
			proxy_set_header X-Forwarded-Host $host;
			proxy_set_header X-Forwarded-Server $host;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header Host $http_host;
			proxy_read_timeout 5m;
			proxy_send_timeout 5m;
               proxy_pass https://xxx.xxx.xxx.xx1:9443/;

               proxy_http_version 1.1;
               proxy_set_header Upgrade $http_upgrade;
               proxy_set_header Connection "upgrade";
        }
    error_log  /var/log/nginx/ui-error.log ;
    access_log  /var/log/nginx/ui-access.log;
	}
	```
5. Create a directory named ssl inside the nginx directory.
6. Follow the instructions below to create SSL certificates for both ESB nodes.
	1. Execute the following command to create the Server Key:
				```
				sudo openssl genrsa -des3 -out server.key 1024
				```
	2. Execute the following command to request to sign the certificate:
				```
				sudo openssl req -new -key server.key -out server.csr
				```
	3. Execute the following commands to remove the passwords:
				```
				sudo cp server.key server.key.org
				sudo openssl rsa -in server.key.org -out server.key
				```
	4. Execute the following command to sign your SSL Certificate:
				```
				sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
				```
	5. Execute the following command to add the certificate to the <EI_HOME>/repository/resources/security/client-truststore.jks file:
				```
				keytool -import -trustcacerts -alias server -file server.crt -keystore client-truststore.jks
				```
7. Execute the following command to restart the NGINX Plus server:
	* NGINX Community
		```
		sudo nginx -s stop
		sudo nginx
		```
	* NGINX Plus
		```
		sudo service  nginx  restart
		```
	!!! Info ""
		Execute the following command if you do not need to restart the server when you are simply making a modification to the VHost file:
		```
		sudo service nginx reload
		```

## Configuring the WSO2 EI server

Depending on the deployment pattern you are setting up, you may have one or several WSO2 EI nodes installed in your deployment.

See [Deploying WSO2 Enterprise Integrator](deploying_wso2_ei.md) for instructions on connecting your server to the load balancer.
