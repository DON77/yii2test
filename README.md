<p align="center">
    <a href="https://github.com/yiisoft" target="_blank">
        <img src="https://avatars0.githubusercontent.com/u/993323" height="100px">
    </a>
    <h1 align="center">Yii 2 Advanced Project Template</h1>
    <br>
</p>

Yii 2 Advanced Project Template is a skeleton [Yii 2](http://www.yiiframework.com/) application best for
developing complex Web applications with multiple tiers.

The template includes three tiers: front end, back end, and console, each of which
is a separate Yii application.

The template is designed to work in a team development environment. It supports
deploying the application in different environments.

Documentation is at [docs/guide/README.md](docs/guide/README.md).

[![Latest Stable Version](https://poser.pugx.org/yiisoft/yii2-app-advanced/v/stable.png)](https://packagist.org/packages/yiisoft/yii2-app-advanced)
[![Total Downloads](https://poser.pugx.org/yiisoft/yii2-app-advanced/downloads.png)](https://packagist.org/packages/yiisoft/yii2-app-advanced)
[![Build Status](https://travis-ci.org/yiisoft/yii2-app-advanced.svg?branch=master)](https://travis-ci.org/yiisoft/yii2-app-advanced)

DIRECTORY STRUCTURE
-------------------

```
common
    config/              contains shared configurations
    mail/                contains view files for e-mails
    models/              contains model classes used in both backend and frontend
    tests/               contains tests for common classes    
console
    config/              contains console configurations
    controllers/         contains console controllers (commands)
    migrations/          contains database migrations
    models/              contains console-specific model classes
    runtime/             contains files generated during runtime
backend
    assets/              contains application assets such as JavaScript and CSS
    config/              contains backend configurations
    controllers/         contains Web controller classes
    models/              contains backend-specific model classes
    runtime/             contains files generated during runtime
    tests/               contains tests for backend application    
    views/               contains view files for the Web application
    web/                 contains the entry script and Web resources
frontend
    assets/              contains application assets such as JavaScript and CSS
    config/              contains frontend configurations
    controllers/         contains Web controller classes
    models/              contains frontend-specific model classes
    runtime/             contains files generated during runtime
    tests/               contains tests for frontend application
    views/               contains view files for the Web application
    web/                 contains the entry script and Web resources
    widgets/             contains frontend widgets
vendor/                  contains dependent 3rd-party packages
environments/            contains environment-based overrides
```
<p align="center">
    <h1 align="center">Installation Guide</h1>
    <br>
</p>
1) clon this repository
2) Install composer(If you have composer installed, skip this part)
3) update composer Command: composer update
4) Set document roots of your web server:

for frontend /path/to/yii-application/frontend/web/ and using the URL http://frontend.dev/
for backend /path/to/yii-application/backend/web/ and using the URL http://backend.dev/
For Apache it could be the following:

    <VirtualHost *:80>
        ServerName frontend.dev
        DocumentRoot "/path/to/yii-application/frontend/web/"
        
        <Directory "/path/to/yii-application/frontend/web/">
            # use mod_rewrite for pretty URL support
            RewriteEngine on
            # If a directory or a file exists, use the request directly
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteCond %{REQUEST_FILENAME} !-d
            # Otherwise forward the request to index.php
            RewriteRule . index.php

            # use index.php as index file
            DirectoryIndex index.php

            # ...other settings...
            # Apache 2.4
            Require all granted
            
            ## Apache 2.2
            # Order allow,deny
            # Allow from all
        </Directory>
    </VirtualHost>
    
    <VirtualHost *:80>
        ServerName backend.dev
        DocumentRoot "/path/to/yii-application/backend/web/"
        
        <Directory "/path/to/yii-application/backend/web/">
            # use mod_rewrite for pretty URL support
            RewriteEngine on
            # If a directory or a file exists, use the request directly
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteCond %{REQUEST_FILENAME} !-d
            # Otherwise forward the request to index.php
            RewriteRule . index.php

            # use index.php as index file
            DirectoryIndex index.php

            # ...other settings...
            # Apache 2.4
            Require all granted
            
            ## Apache 2.2
            # Order allow,deny
            # Allow from all
        </Directory>
    </VirtualHost>
For nginx:

    server {
        charset utf-8;
        client_max_body_size 128M;

        listen 80; ## listen for ipv4
        #listen [::]:80 default_server ipv6only=on; ## listen for ipv6

        server_name frontend.dev;
        root        /path/to/yii-application/frontend/web/;
        index       index.php;

        access_log  /path/to/yii-application/log/frontend-access.log;
        error_log   /path/to/yii-application/log/frontend-error.log;

        location / {
            # Redirect everything that isn't a real file to index.php
            try_files $uri $uri/ /index.php$is_args$args;
        }

        # uncomment to avoid processing of calls to non-existing static files by Yii
        #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
        #    try_files $uri =404;
        #}
        #error_page 404 /404.html;

        # deny accessing php files for the /assets directory
        location ~ ^/assets/.*\.php$ {
            deny all;
        }

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_pass 127.0.0.1:9000;
            #fastcgi_pass unix:/var/run/php5-fpm.sock;
            try_files $uri =404;
        }
    
        location ~* /\. {
            deny all;
        }
    }
     
    server {
        charset utf-8;
        client_max_body_size 128M;
    
        listen 80; ## listen for ipv4
        #listen [::]:80 default_server ipv6only=on; ## listen for ipv6
    
        server_name backend.dev;
        root        /path/to/yii-application/backend/web/;
        index       index.php;
    
        access_log  /path/to/yii-application/log/backend-access.log;
        error_log   /path/to/yii-application/log/backend-error.log;
    
        location / {
            # Redirect everything that isn't a real file to index.php
            try_files $uri $uri/ /index.php$is_args$args;
        }
    
        # uncomment to avoid processing of calls to non-existing static files by Yii
        #location ~ \.(js|css|png|jpg|gif|swf|ico|pdf|mov|fla|zip|rar)$ {
        #    try_files $uri =404;
        #}
        #error_page 404 /404.html;

        # deny accessing php files for the /assets directory
        location ~ ^/assets/.*\.php$ {
            deny all;
        }

        location ~ \.php$ {
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_pass 127.0.0.1:9000;
            #fastcgi_pass unix:/var/run/php5-fpm.sock;
            try_files $uri =404;
        }
    
        location ~* /\. {
            deny all;
        }
    }
5) Change the hosts file to point the domain to your server.

	Windows: c:\Windows\System32\Drivers\etc\hosts
	Linux: /etc/hosts
	Add the following lines:

	127.0.0.1   frontend.dev
	127.0.0.1   backend.dev
	
	
6)run migrations - php yii migrate/up --migrationPath=@vendor/dektrium/yii2-user/migrations

7) 
 7.1) edit your /common/config/main.php file, by setting  'enableConfirmation'=>true.
			'modules' => [
				'user' => [
					'class' => 'dektrium\user\Module',
					'enableConfirmation'=>false,
				],
			],
 7.2) edit your /backend/config/main.php file, by setting  'enableConfirmation'=>true.
			'modules' => [
				'user' => [
					'class' => 'dektrium\user\Module',
					'enableConfirmation'=>false,
				],
			],
 7.4) edit your /frontend/config/main.php file, by setting  'enableConfirmation'=>true.( if set to false, confirmation mail will not be send )
			'modules' => [
				'user' => [
					'class' => 'dektrium\user\Module',
					'enableConfirmation'=>false,
				],
			],
8) edit facebook auth configs in /frontend/config/main.php file
			'authClientCollection' => [
				'class'   => \yii\authclient\Collection::className(),
				'clients' => [
					'facebook' => [
						'class'        => 'dektrium\user\clients\Facebook',
						'clientId'     => 'APP_ID',
						'clientSecret' => 'APP_SECRET',
					],
				],
			],
			
	note that you should also edit your redirectional urls in facebook developer dashboard.
	
9)configure your mailing system. At this point all mails go to  /frontend/runtime/mail folder.
