# udagramDeploy



#Create the root cert
$cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=AzureRootCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My"  `
-KeyUsageProperty Sign -KeyUsage CertSign

 # Create Client Cert
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=AzureClientCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")

---

### Commands to run in ELK Server

sudo apt update

sudo apt install docker.io

sudo apt install python3-pip

sudo pip3 install docker

sudo sysctl -w vm.max_map_count=262144

sudo docker pull sebp/elk:761 

sudo docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -it --name elk sebp/elk:761



---



### Command Sheet for installing Filebeat:

Install & start Apache2 on the webserver

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

sudo dpkg -i filebeat-7.4.0-amd64.deb

cd /etc/filebeat

edit filebeat.yml and change the value for output.elasticsearch and setup.kibana to reflect the IP of your Elk server

sudo filebeat modules enable system

sudo filebeat modules enable apache

sudo filebeat setup

sudo service filebeat start
