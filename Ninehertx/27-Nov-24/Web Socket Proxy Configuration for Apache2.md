
```bash
<VirtualHost *:80>
        RewriteEngine On
        RewriteCond %{REQUEST_METHOD} ^TRACE
        RewriteRule .* - [F]
        ServerName demo.dev9server.com
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/dev9server.com/public_html/
<Directory /var/www/dev9server.com/public_html/>
       Options FollowSymLinks
       AllowOverride All
       Require all granted
</Directory>
    RewriteCond %{SERVER_NAME} =demo.dev9server.com
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
    ProxyPass /rc-parts-dev-socket ws://localhost:3385
    ProxyPassReverse /rc-parts-dev-socket ws://localhost:3385
</VirtualHost>
	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
	
<VirtualHost *:443>
ServerName  demo.dev9server.com
DocumentRoot /var/www/dev9server.com/public_html/
SSLEngine on
SSLCertificateFile /opt/ssl/ssl-2023/demo_dev9server_com.crt
SSLCertificateKeyFile  /opt/ssl/ssl-2023/demo.dev9server.com_key.txt
SSLCertificateChainFile  /opt/ssl/ssl-2023/demo_dev9server_com.ca-bundle
<Directory /var/www/dev9server.com/public_html/>
    Options FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
    ProxyPass /rc-parts-dev-socket wss://localhost:3385
    ProxyPassReverse /rc-parts-dev-socket wss://localhost:3385
</VirtualHost>
```



