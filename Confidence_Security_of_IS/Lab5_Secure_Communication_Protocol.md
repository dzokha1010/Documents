
# Laboratory 5 - Secure Communication Protocol
## I. Introduction


## II. Practical Application
- Thiết lập Website trên localhost và dùng Wireshark bắt gói tin

- Cài đặt OpenSSL thiết lập giao thức SSL cho Website và dùng Wireshark bắt gói tin

  Link: https://www.openssl.org/
  
  Phiên bản cài đặt cho Windows: https://slproweb.com/products/Win32OpenSSL.html

  Cấu hình SSL cho localhost

  B1: Create Private Key
  ```
  openssl genrsa -des3 -out rootSSL.key 2048
  ```
  B2: Create Certificate File
  ```
  openssl req -x509 -new -nodes -key rootSSL.key -sha256 -days 1024 -out rootSSL.pem
  ```
  B3: Create a local domain.
  Thêm dòng sau vào file hosts trong thư mục c:\windows\system32\drivers\etc\
  ```
  localhost 127.0.0.1 domain-new.local
  ```
  B4: Create a Private Key for the New Domain.
  
  ```
  openssl req -new -sha256 -nodes -out domain-new.local.csr -newkey rsa:2048 -keyout domain-new.local.key -subj "/C=US/ST=GA/L=Tifton/O=Client One/OU=Dev/CN=domain-new.local/emailAddress=hello@domain-new.local"
  ```
  B5: Issue the New Certificate Using the Root SSL Certificate
  ```
  openssl x509 -req -in domain-new.local.csr -CA rootSSL.pem -CAkey rootSSL.key -CAcreateserial -out domain-new.local.crt -days 500 -sha256 -extensions "authorityKeyIdentifier=keyid,issuer\n basicConstraints=CA:FALSE\n keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment\n  subjectAltName=DNS:domain-new.local"
  ```
  B6: Thêm đoạn sau vào xampp/apache/conf/extra/httpd-vhosts.conf
  ```
  <VirtualHost *:443>
    ServerName domain-new.local
    DocumentRoot "C:/xampp/htdocs/"
    SSLEngine on
    SSLCertificateFile "C:\xampp\apache\conf\ssl.crt\domain-new.local.crt"
    SSLCertificateKeyFile "C:\xampp\apache\conf\ssl.key\domain-new.local.key"
  </VirtualHost>
  ```
  B7: Get Windows to Trust the Certificate Authority
  1. Windows + R
  2. MMC Enter
  3. File > Add/Remove Snap-in
  4. Click “Certificates” and “Add”
  5. Select “Computer Account” and click “Next”
  6. Select “Local Computer” then click “Finish”
  7. Click “OK” to go back to the MMC window
  8. Double-click “Certificates (local computer)” to expand the view
  9.  Select “Trusted Root Certification Authorities”, right-click “Certificates” and select “All Tasks” then “Import”
  10.  Click “Next” then Browse and locate the “rootSSL.pem”
  11.  Select “Place all certificates in the following store” and select the “Trusted Root Certification Authorities store”. Click “Next” then click “Finish” to complete the wizard
 
- Cài đặt FTP trên Windows và thử nghiệm truyền file và dùng Wireshark bắt gói tin

- Cài đặt SSH trên Windows hoặc SSL cho FTP và thử nghiệm truyền file và dùng Wireshak bắt gói tin
  
