
Steps we followed on CentOS 7 OS are captured below .



Below mentioned commands are executed to add the Rundeck download details to yum repo :

rpm -Uvh http://repo.rundeck.org/latest.rpm


Install Rundeck using yum comm and along with Java

sudo yum install rundeck java



If Rundeck needs update it can be updated from following command

sudo yum update rundeck


Edit /etc/rundeck/rundeck-config.properties file

grails.serverURL=http://<hostname>:4440


To start Rundeck:

sudo service rundeckd start


Verify that the service started correctly by tailing the logs:

tail -f /var/log/rundeck/service.log

Once the service is up and running, we can see following line in the logs

Grails application running at http://localhost:4440 in environment: production


Rundeck has a CLI It can be installed with following commands,

sudo wget https://binary.com/rundeck/rundeck-rom/rpm -O bintray.repo
sudo mv binary.repo /etc/yum.repos.d/
sudo yum install runback-cli


Project can be created with following steps
Export RD_URL ex:

export RD_URL=http://<hostname>:4440

rd projects create -p <project_name> -- \

--project.label="<some lable of the project>" \

--project.ssh-keypath=</path/of/private-key


Roman's work starts from here
  
```  
To upgrade rundeck:



yum upgrade rundeck rundeck-config

systemctl restart rundeckd

systemctl status rundeckd



Create DNS request using bellow URL:


Create certificates request:


Once you got certificates:


1.- install Rundeck. (Skip this step if you have rundeck already)
rpm -Uvh https://repo.rundeck.org/latest.rpm
yum install rundeck
2.- create keystore: (if you already have a certificate in .key/.crt or .pk12 formats, skip to 2b)
keytool -keystore /etc/rundeck/ssl/keystore -alias rundeck -genkey -keyalg RSA -keypass password -storepass password
2b.- in case you have your own certificate, do below:
If you have .crt and .key files, create a .p12 file:

openssl pkcs12 -export -in YOUR.crt -inkey YOUR.key -out NEW.p12
Convert it to a .jks (also if you have only the .p12 file):

keytool -importkeystore -destkeystore keystore -srckeystore NEW.p12 -srcstoretype pkcs12
3.- copy keystore as truststore.
4.- edit /etc/rundeck/ssl/ssl.properties file:
keystore=/etc/rundeck/ssl/keystore keystore.password=password key.password=password truststore=/etc/rundeck/ssl/truststore truststore.password=password
5.- edit /etc/rundeck/framework.properties file:
framework.server.port = 4443 framework.server.url = https://localhost:4443
6.- edit /etc/rundeck/rundeck-config.properties file:
grails.serverURL=https://localhost:4443
7.- edit/create /etc/sysconfig/rundeckd file:
export RUNDECK_WITH_SSL=true
8.- start the rundeck service.
systemctl restart rundeckd
  
```
