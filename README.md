unitedasian/docker-grav
===========

A Docker image that can be used to run a [grav](http://getgrav.org/) website inside a container via nginx and php-fpm.

Usage
-----

Create your [grav](http://getgrav.org/) website as per the instructions from the [Grav documentation](http://learn.getgrav.org/).

### Using the original image

```
docker run -d -p 8000:80 -v `pwd`:/usr/share/nginx/html unitedasian/docker-nginx-php-fpm-grav
```

To customize PHP, PHP-FPM settings and nginx configuration, create the relevant configuration files in your host and mount them on the container:

```
docker run -d -p 8000:80 \
	-v `pwd`/php.ini:/etc/php5/fpm/conf.d/local.ini \
	-v `pwd`/php-fpm.conf:/etc/php5/fpm/pool.d/ww.conf \
	-v `pwd`/nginx.conf:/etc/nginx/nginx.conf \
	-v `pwd`:/usr/share/nginx/html \
	unitedasian/grav
```

### Using your own image

Create a Dockerfile with the following contents:

```
FROM unitedasian/grav

COPY . /usr/share/nginx/html

RUN chown -R www-data:www-data /usr/share/nginx/html && \
	chmod -R 775 /usr/share/nginx/html && \
	chmod -R +s /usr/share/nginx/html && \
	umask 0002

# Customize PHP settings (optional)
# COPY php.ini /etc/php5/fpm/conf.d/local.ini


# Customize PHP-FPM settings (optional)
# COPY php-fpm.conf /etc/php5/fpm/pool.d/www.conf

# Customize nginx configuration (optional)
# COPY nginx.conf /etc/nginx/nginx.conf

CMD ["/entrypoint.sh"]

```

Note that this image does NOT include a Grav installation. It assumes you already have a working Grav site which will be mounted on the container.
