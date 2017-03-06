# Jenkins Demo
-----
#### Simple PHP Webserver setup on AWS
* Launch instance on AWS.
* Assign to security groups - ports 22 and 80 at least.
* update server
  * ```sudo apt-get update```
* install nginx
  * ```sudo apt-get install nginx```
* install mysql-server (why not?)
  * ```sudo apt-get install mysql-server```
  * ```sudo mysql_secure_installation```
* install php-fpm php-mysql
  * ```sudo apt-get install php-fpm php-mysql```
* configure php to work with nginx
  * ```sudo nano /etc/php/7.0/fpm/php.ini```
    * find cgi.fix_pathinfo and set to 0
* restart php processor
  * ```sudo systemctl restart php7.0-fpm```
* configure nginx to use php processor
  * edit config to match server block below
  * ```sudo nano /etc/nginx/sites-available/default```
  ```
  server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;

        index index.php index.html index.htm index.nginx-debian.html;

        server_name xxx.xxx.xxx.xxx;# your IP or server address

        location / {
            try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
            deny all;
        }

}```
  * check with ```sudo nginx -t```
  * restart nginx ```sudo systemctl reload nginx```

