# https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Self-signcertificatesforSplunkWeb

openssl genrsa -aes256 -out myCAPrivateKey.key 2048
openssl req -new -key myCAPrivateKey.key -out myCACertificate.csr
openssl x509 -req -in myCACertificate.csr -signkey myCAPrivateKey.key -out myCACertificate.pem -days 3650

openssl genrsa -aes256 -out mySplunkWebPrivateKey.key 2048
openssl rsa -in mySplunkWebPrivateKey.key -out mySplunkWebPrivateKey.key
openssl rsa -in mySplunkWebPrivateKey.key -text

openssl req -new -key mySplunkWebPrivateKey.key -out mySplunkWebCert.csr

openssl x509 -req -in mySplunkWebCert.csr -CA myCACertificate.pem -CAkey myCAPrivateKey.key -CAcreateserial -out mySplunkWebCert.pem -days 1095

cat mySplunkWebCert.pem myCACertificate.pem > mySplunkWebCertificate.pem
