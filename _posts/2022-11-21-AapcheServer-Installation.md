---
layout: post
title:  "Apache Server Installation"
categories: [ blueseam, tutorial ]
image: assets/images/apache.png
---

# Apache Server Installation

### 1. System(OS) Configuration

##### 1.1. Preferences Information
```
apache01  172.16.25.111
apache02  172.16.25.112
```

##### 1.2. System Update
```
# dnf makecache

# dnf -y update

# dnf install -y epel-release
```

##### 1.3. Firewalld Disable (for Test)
```
# systemctl stop firewalld && systemctl disable firewalld
```


### 2. Apache Installation
```
[root@apache01 ~]# dnf list httpd
Extra Packages for Enterprise Linux 9 - x86_64                                                                                               453 kB/s |  11 MB     00:25    
Extra Packages for Enterprise Linux 9 - Next - x86_64                                                                                        780 kB/s | 1.4 MB     00:01    
마지막 메타자료 만료확인 0:00:01 이전인: 2022년 11월 17일 (목) 오전 11시 18분 18초.
사용 가능한 꾸러미
httpd.x86_64                                                                      2.4.53-7.el9                                                                      appstream

[root@apache01 ~]# dnf install -y httpd
마지막 메타자료 만료확인 0:03:32 이전인: 2022년 11월 17일 (목) 오전 11시 18분 18초.
종속성이 해결되었습니다.
=============================================================================================================================================================================
 꾸러미                                          구조                                버전                                       레포지터리                              크기
=============================================================================================================================================================================
설치 중:
 httpd                                           x86_64                              2.4.53-7.el9                               appstream                               50 k
종속 꾸러미 설치 중:
 apr                                             x86_64                              1.7.0-11.el9                               appstream                              123 k
 apr-util                                        x86_64                              1.6.1-20.el9                               appstream                               95 k
 apr-util-bdb                                    x86_64                              1.6.1-20.el9                               appstream                               14 k
 centos-logos-httpd                              noarch                              90.4-1.el9                                 appstream                              252 k
 httpd-core                                      x86_64                              2.4.53-7.el9                               appstream                              1.5 M
 httpd-filesystem                                noarch                              2.4.53-7.el9                               appstream                               15 k
 httpd-tools                                     x86_64                              2.4.53-7.el9                               appstream                               85 k
 mailcap                                         noarch                              2.1.49-5.el9                               baseos                                  33 k
취약한 종속 꾸러미 설치 중:
 apr-util-openssl                                x86_64                              1.6.1-20.el9                               appstream                               16 k
 mod_http2                                       x86_64                              1.15.19-2.el9                              appstream                              150 k
 mod_lua                                         x86_64                              2.4.53-7.el9                               appstream                               63 k

연결 요약
=============================================================================================================================================================================
설치  12 꾸러미

총계 내려받기 크기: 2.4 M
설치된 크기 : 6.4 M
꾸러미 내려받기 중:
(1/12): apr-util-1.6.1-20.el9.x86_64.rpm                                                                                                     244 kB/s |  95 kB     00:00    
(2/12): apr-util-bdb-1.6.1-20.el9.x86_64.rpm                                                                                                 255 kB/s |  14 kB     00:00    
(3/12): mailcap-2.1.49-5.el9.noarch.rpm                                                                                                       71 kB/s |  33 kB     00:00    
(4/12): apr-util-openssl-1.6.1-20.el9.x86_64.rpm                                                                                             341 kB/s |  16 kB     00:00    
(5/12): apr-1.7.0-11.el9.x86_64.rpm                                                                                                          246 kB/s | 123 kB     00:00    
(6/12): httpd-2.4.53-7.el9.x86_64.rpm                                                                                                        601 kB/s |  50 kB     00:00    
(7/12): httpd-filesystem-2.4.53-7.el9.noarch.rpm                                                                                             339 kB/s |  15 kB     00:00    
(8/12): httpd-tools-2.4.53-7.el9.x86_64.rpm                                                                                                  1.7 MB/s |  85 kB     00:00    
(9/12): mod_http2-1.15.19-2.el9.x86_64.rpm                                                                                                   2.1 MB/s | 150 kB     00:00    
(10/12): mod_lua-2.4.53-7.el9.x86_64.rpm                                                                                                     1.0 MB/s |  63 kB     00:00    
(11/12): centos-logos-httpd-90.4-1.el9.noarch.rpm                                                                                            502 kB/s | 252 kB     00:00    
(12/12): httpd-core-2.4.53-7.el9.x86_64.rpm                                                                                                  2.4 MB/s | 1.5 MB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
합계                                                                                                                                         676 kB/s | 2.4 MB     00:03     
연결 확인 실행 중
연결 확인에 성공했습니다.
연결 시험 실행 중
연결 시험에 성공했습니다.
연결 실행 중
  준비 중           :                                                                                                                                                    1/1 
  설치 중           : apr-1.7.0-11.el9.x86_64                                                                                                                           1/12 
  설치 중           : apr-util-bdb-1.6.1-20.el9.x86_64                                                                                                                  2/12 
  설치 중           : apr-util-openssl-1.6.1-20.el9.x86_64                                                                                                              3/12 
  설치 중           : apr-util-1.6.1-20.el9.x86_64                                                                                                                      4/12 
  설치 중           : httpd-tools-2.4.53-7.el9.x86_64                                                                                                                   5/12 
  스크립트릿 실행 중: httpd-filesystem-2.4.53-7.el9.noarch                                                                                                              6/12 
  설치 중           : httpd-filesystem-2.4.53-7.el9.noarch                                                                                                              6/12 
  설치 중           : centos-logos-httpd-90.4-1.el9.noarch                                                                                                              7/12 
  설치 중           : mailcap-2.1.49-5.el9.noarch                                                                                                                       8/12 
  설치 중           : httpd-core-2.4.53-7.el9.x86_64                                                                                                                    9/12 
  설치 중           : mod_http2-1.15.19-2.el9.x86_64                                                                                                                   10/12 
  설치 중           : mod_lua-2.4.53-7.el9.x86_64                                                                                                                      11/12 
  설치 중           : httpd-2.4.53-7.el9.x86_64                                                                                                                        12/12 
  스크립트릿 실행 중: httpd-2.4.53-7.el9.x86_64                                                                                                                        12/12 
  확인 중           : mailcap-2.1.49-5.el9.noarch                                                                                                                       1/12 
  확인 중           : apr-1.7.0-11.el9.x86_64                                                                                                                           2/12 
  확인 중           : apr-util-1.6.1-20.el9.x86_64                                                                                                                      3/12 
  확인 중           : apr-util-bdb-1.6.1-20.el9.x86_64                                                                                                                  4/12 
  확인 중           : apr-util-openssl-1.6.1-20.el9.x86_64                                                                                                              5/12 
  확인 중           : centos-logos-httpd-90.4-1.el9.noarch                                                                                                              6/12 
  확인 중           : httpd-2.4.53-7.el9.x86_64                                                                                                                         7/12 
  확인 중           : httpd-core-2.4.53-7.el9.x86_64                                                                                                                    8/12 
  확인 중           : httpd-filesystem-2.4.53-7.el9.noarch                                                                                                              9/12 
  확인 중           : httpd-tools-2.4.53-7.el9.x86_64                                                                                                                  10/12 
  확인 중           : mod_http2-1.15.19-2.el9.x86_64                                                                                                                   11/12 
  확인 중           : mod_lua-2.4.53-7.el9.x86_64                                                                                                                      12/12 

설치되었습니다:
  apr-1.7.0-11.el9.x86_64                       apr-util-1.6.1-20.el9.x86_64          apr-util-bdb-1.6.1-20.el9.x86_64          apr-util-openssl-1.6.1-20.el9.x86_64         
  centos-logos-httpd-90.4-1.el9.noarch          httpd-2.4.53-7.el9.x86_64             httpd-core-2.4.53-7.el9.x86_64            httpd-filesystem-2.4.53-7.el9.noarch         
  httpd-tools-2.4.53-7.el9.x86_64               mailcap-2.1.49-5.el9.noarch           mod_http2-1.15.19-2.el9.x86_64            mod_lua-2.4.53-7.el9.x86_64                  

완료되었습니다!

# systemctl start httpd

# systemctl enable httpd

# systemctl status httpd
```

### 3. Firewall Disable
```
# firewall-cmd --add-service=http

# firewall-cmd --add-service=https

# firewall-cmd --runtime-to-permanent

# firewall-cmd --reload

or 

# systemctl stop firewalld && systemctl disable firewalld
```

### 4. SSL/TLS Configuration

##### 4.1. Create Certification (Optionnal)
```
[root@apache01 ~]# openssl genrsa -des3 -out private.key 2048
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:

[root@apache01 ~]# cp private.key private.key.dup

[root@apache01 ~]# openssl rsa -in private.key.dup -out private.key
Enter pass phrase for private.key.dup:
writing RSA key

[root@apache01 ~]# openssl req -new -key private.key -out  csr_temp.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:KR
State or Province Name (full name) []:Korea
Locality Name (eg, city) [Default City]:Seoul
Organization Name (eg, company) [Default Company Ltd]:Dev
Organizational Unit Name (eg, section) []:Dev
Common Name (eg, your name or your server's hostname) []:test.net
Email Address []:blueseam@gmail.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

[root@apache01 ~]# openssl x509 -req -days 365 -in csr_temp.csr -signkey private.key -out ssl.crt 

[root@apache01 ~]# cp private.key /etc/httpd/conf.d/

[root@apache01 ~]# cp ssl.crt /etc/httpd/conf.d/

```

##### 4.2. Configure httpd for SSL/TLS
```
[root@apache01 ~]# dnf install -y mod_ssl
마지막 메타자료 만료확인 2:08:02 이전인: 2022년 11월 17일 (목) 오전 11시 18분 18초.
종속성이 해결되었습니다.
=============================================================================================================================================================================
 꾸러미                                 구조                                  버전                                            레포지터리                                크기
=============================================================================================================================================================================
설치 중:
 mod_ssl                                x86_64                                1:2.4.53-7.el9                                  appstream                                112 k
종속 꾸러미 설치 중:
 sscg                                   x86_64                                3.0.0-5.el9                                     appstream                                 46 k

연결 요약
=============================================================================================================================================================================
설치  2 꾸러미

총계 내려받기 크기: 158 k
설치된 크기 : 366 k
꾸러미 내려받기 중:
(1/2): sscg-3.0.0-5.el9.x86_64.rpm                                                                                                           109 kB/s |  46 kB     00:00    
(2/2): mod_ssl-2.4.53-7.el9.x86_64.rpm                                                                                                       222 kB/s | 112 kB     00:00    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
합계                                                                                                                                          89 kB/s | 158 kB     00:01     
연결 확인 실행 중
연결 확인에 성공했습니다.
연결 시험 실행 중
연결 시험에 성공했습니다.
연결 실행 중
  준비 중           :                                                                                                                                                    1/1 
  설치 중           : sscg-3.0.0-5.el9.x86_64                                                                                                                            1/2 
  설치 중           : mod_ssl-1:2.4.53-7.el9.x86_64                                                                                                                      2/2 
  스크립트릿 실행 중: mod_ssl-1:2.4.53-7.el9.x86_64                                                                                                                      2/2 
  확인 중           : mod_ssl-1:2.4.53-7.el9.x86_64                                                                                                                      1/2 
  확인 중           : sscg-3.0.0-5.el9.x86_64                                                                                                                            2/2 

설치되었습니다:
  mod_ssl-1:2.4.53-7.el9.x86_64                                                            sscg-3.0.0-5.el9.x86_64                                                           

완료되었습니다!


[root@apache01 ~]# vi /etc/httpd/conf.d/ssl.conf
...
#SSLCertificateFile /etc/pki/tls/certs/localhost.crt
SSLCertificateFile /etc/httpd/conf.d/ssl.crt
...
#SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
SSLCertificateKeyFile /etc/httpd/conf.d/private.key
...

[root@apache01 ~]# systemctl restart httpd

```

### 5. HTTPD Configuration

##### 5.1. Redirection(HTTP > HTTPS) Configuration
```
[root@apache01 conf.d]# vi vhost.conf
...
<VirtualHost *:80>
 # ServerName *.serve.co.kr
 # ServerAlias www.serve.co.kr
 # DocumentRoot /var/www/html
 RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R,L]
</VirtualHost> 
...

[root@apache01 conf.d]# systemctl restart httpd
```

##### 5.2. Virtual Host Configuration
```
[root@apache01 conf.d]# vi ssl.conf
...
<VirtualHost *:443>
DocumentRoot /var/www/www
ServerName www.test.net
ErrorLog logs/www-error_log
CustomLog logs/www-access_log combined

SSLEngine on
SSLCipherSuite PROFILE=SYSTEM
SSLProxyCipherSuite PROFILE=SYSTEM
SSLCertificateFile /etc/httpd/conf.d/ssl.crt
SSLCertificateKeyFile /etc/httpd/conf.d/private.key
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0
</VirtualHost>

<VirtualHost *:443>
DocumentRoot /var/www/admin
ServerName admin.test.net
ErrorLog logs/www-error_log
CustomLog logs/www-access_log combined

SSLEngine on
SSLCipherSuite PROFILE=SYSTEM
SSLProxyCipherSuite PROFILE=SYSTEM
SSLCertificateFile /etc/httpd/conf.d/ssl.crt
SSLCertificateKeyFile /etc/httpd/conf.d/private.key
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0
</VirtualHost>

<VirtualHost *:443>
DocumentRoot /var/www/html
ServerName test.test.net
ServerAlias *.test.net
...

[root@apache01 conf.d]# systemctl restart httpd
```

##### 5.3. Reverse Proxy Configuration
```
[root@apache01 conf.d]# vi ssl.conf
...
<VirtualHost *:443>
DocumentRoot /var/www/html
ServerName apis.test.net
ErrorLog logs/www-error_log
CustomLog logs/www-access_log combined

ProxyRequests Off
ProxyPreserveHost On

<Proxy "balancer://mycluster">
BalancerMember "http://172.16.25.112:80"
BalancerMember "http://172.16.25.112:81"
BalancerMember "http://172.16.25.112:82"
</Proxy>

ProxyPass / balancer://mycluster/
ProxyPassReverse / balancer://mycluster/

# ProxyPass / http://172.16.25.112:80
# ProxyPassReverse / http://172.16.25.112:80

SSLEngine on
SSLCipherSuite PROFILE=SYSTEM
SSLProxyCipherSuite PROFILE=SYSTEM
SSLCertificateFile /etc/httpd/conf.d/ssl.crt
SSLCertificateKeyFile /etc/httpd/conf.d/private.key
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0
</VirtualHost>
...

[root@apache01 conf.d]# systemctl restart httpd
```

### 6. Other setting

##### 6.1. Cache Configuration
```
[root@apache01 conf.d]# ls-al /etc/httpd/modules/mod_expires.so

[root@apache01 conf.d]# vi ssl.conf
...
<VirtualHost *:443>
DocumentRoot /var/www/html
ServerName apis.test.net
ErrorLog logs/www-error_log
CustomLog logs/www-access_log combined

ProxyRequests Off
ProxyPreserveHost On

<Proxy "balancer://mycluster">
BalancerMember "http://172.16.25.112:80"
BalancerMember "http://172.16.25.112:81"
BalancerMember "http://172.16.25.112:82"
</Proxy>

ProxyPass / balancer://mycluster/
ProxyPassReverse / balancer://mycluster/

<IfModule mod_expires.c>
ExpiresActive On
ExpiresByType image/jpeg “acces plus 1 month”
ExpiresByType image/gif “acces plus 1 month”
ExpiresByType image/png “acces plus 1 month”
</IfModule>

SSLEngine on
SSLCipherSuite PROFILE=SYSTEM
SSLProxyCipherSuite PROFILE=SYSTEM
SSLCertificateFile /etc/httpd/conf.d/ssl.crt
SSLCertificateKeyFile /etc/httpd/conf.d/private.key
BrowserMatch "MSIE [2-5]" \
         nokeepalive ssl-unclean-shutdown \
         downgrade-1.0 force-response-1.0
</VirtualHost>
...

[root@apache01 conf.d]# systemctl restart httpd
```


