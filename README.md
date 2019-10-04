# WordPress Heroku

Este proyecto es una plantilla para instalar y ejecutar [WordPress](http://wordpress.org/) en [Heroku](http://www.heroku.com/). El repositorio viene incluido con:
* [PostgreSQL for WordPress](http://wordpress.org/extend/plugins/postgresql-for-wordpress/)
* [Amazon S3 and Cloudfront](https://wordpress.org/plugins/amazon-s3-and-cloudfront/)
* [WP Sendgrid](https://wordpress.org/plugins/wp-sendgrid/)
* [Wordpress HTTPS](https://wordpress.org/plugins/wordpress-https/)

## Instalación

Clonar el repositorio de Github

    $ git clone git://github.com/QuidamAshura/academia.git

Con la [Heroku gem](http://devcenter.heroku.com/articles/heroku-command), cree su aplicación

    $ cd heroku-wordpress
    $ heroku create
    Creating strange-bird-1234... done, stack is cedar
    http://strange-bird-1234.herokuapp.com/ | git@heroku.com:strange-bird-1234.git
    Git remote heroku added

Agregue una base de datos a su aplicación

    $ heroku addons:create heroku-postgresql
    Creating HEROKU_POSTGRESQL_INSTANCE... done, (free)
    Adding HEROKU_POSTGRESQL_INSTANCE to strange-bird-1234... done
    Setting DATABASE_URL and restarting strange-bird-1234... done, v3
    Database has been created and is available
     ! This database is empty. If upgrading, you can transfer
     ! data from another database with pgbackups:restore
    Use `heroku addons:docs heroku-postgresql` to view documentation.

Promocione la base de datos (reemplace HEROKU_POSTGRESQL_INSTANCE con el nombre del resultado anterior)

    $ heroku pg:promote HEROKU_POSTGRESQL_INSTANCE
    Promoting HEROKU_POSTGRESQL_INSTANCE to DATABASE_URL... done
    Ensuring an alternate alias for existing DATABASE... done, HEROKU_POSTGRESQL_COLOR
    Promoting HEROKU_POSTGRESQL_INSTANCE to DATABASE_URL on strange-bird-1234... done

Agregue la capacidad de enviar correos electrónicos (es decir, restablecimiento de contraseña, etc.)

    $ heroku addons:create sendgrid:starter
    Creating SENDGRID_INSTANCE... done, (free)
    Adding SENDGRID_INSTANCE to strange-bird-1234... done
    Setting SENDGRID_PASSWORD, SENDGRID_USERNAME and restarting strange-bird-1234... done, v7
    Use `heroku addons:docs sendgrid` to view documentation.

Cree una nueva sucursal para cualquier configuración / cambios de configuración necesarios

    $ git checkout -b production

Almacene claves y sales únicas en las variables de entorno de Heroku. Wordpress puede proporcionar valores aleatorios [aquí](https://api.wordpress.org/secret-key/1.1/salt/).

    heroku config:set AUTH_KEY='put your unique phrase here' \
      SECURE_AUTH_KEY='put your unique phrase here' \
      LOGGED_IN_KEY='put your unique phrase here' \
      NONCE_KEY='put your unique phrase here' \
      AUTH_SALT='put your unique phrase here' \
      SECURE_AUTH_SALT='put your unique phrase here' \
      LOGGED_IN_SALT='put your unique phrase here' \
      NONCE_SALT='put your unique phrase here'

Deploy en Heroku

    $ git push heroku production:master
    -----> Deleting 0 files matching .slugignore patterns.
    -----> PHP app detected

     !     WARNING: No composer.json found.
           Using index.php to declare PHP applications is considered legacy
           functionality and may lead to unexpected behavior.

    -----> No runtime requirements in composer.json, defaulting to PHP 5.6.2.
    -----> Installing system packages...
           - PHP 5.6.2
           - Apache 2.4.10
           - Nginx 1.6.0
    -----> Installing PHP extensions...
           - zend-opcache (automatic; bundled, using 'ext-zend-opcache.ini')
    -----> Installing dependencies...
           Composer version 1.0-dev (ffffab37a294f3383c812d0329623f0a4ba45387) 2014-11-05 06:04:18
           Loading composer repositories with package information
           Installing dependencies
           Nothing to install or update
           Generating optimized autoload files
    -----> Preparing runtime environment...
           NOTICE: No Procfile, defaulting to 'web: vendor/bin/heroku-php-apache2'
    -----> Discovering process types
           Procfile declares types -> web

    -----> Compressing... done, 78.5MB
    -----> Launcing... done, v5
           http://strange-bird-1234.herokuapp.com deployed to Heroku

    To git@heroku:strange-bird-1234.git
      * [new branch]    production -> master

Después de la implementación, WordPress tiene algunos pasos más para configurar y ¡listo!

## Uso

Debido a que un archivo no se puede escribir en el sistema de archivos de Heroku, la actualización e instalación de complementos o temas debe hacerse localmente y luego enviarse a Heroku.

## Actualización

Actualizar su versión de WordPress es solo una cuestión de combinar las actualizaciones en
La rama creada a partir de la instalación.

    $ git pull # Get the latest

Usando el mismo nombre de sucursal de nuestra instalación:

    $ git checkout production
    $ git merge master # Merge latest
    $ git push heroku production:master

WordPress necesita actualizar la base de datos. Después de presionar, navegue a:
    http://your-app-url.herokuapp.com/wp-admin

WordPress le pedirá que actualice la base de datos. Después de eso serás bueno
ir.

## Implementación y optimización

Si tiene archivos que desea rastrear en su repositorio, pero no necesita implementación (por ejemplo, * .md, * .pdf, * .zip). Luego agregue la ruta o la coincidencia del archivo de Linux al archivo `.slugignore` y no se implementarán.

Ejemplos:
```
path/to/ignore/
bin/
*.md
*.pdf
*.zip
```
