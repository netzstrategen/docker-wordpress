FROM wordpress:5.1.1-php7.1-apache

ENV NODE_VERSION=8

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
  chmod +x wp-cli.phar && \
  mv wp-cli.phar /usr/local/bin/wp

RUN curl -O https://raw.githubusercontent.com/wp-cli/wp-cli/v1.5.1/utils/wp-completion.bash && \
  mv wp-completion.bash /etc/

RUN set -eux; \
  { \
    echo 'source /etc/wp-completion.bash'; \
  } > /var/www/.bashrc

RUN apt-get update && apt -y install \
  gpg \
  less \
  vim \
  mysql-client \
  && \
  curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - && apt-get install -y nodejs && \
  npm -g install gulp

RUN set -eux; \
  { \
    echo '<VirtualHost *:80>'; \
    echo ' RewriteEngine On'; \
    echo ' RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-f'; \
    echo ' RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-d'; \
    echo ' RewriteCond %{DOCUMENT_ROOT}%{REQUEST_FILENAME} !-l'; \
    echo ' RewriteRule ^/(wp-content/uploads/.*)$ %{REQUEST_SCHEME}://%{ENV:UPLOADS_PROXY_DOMAIN}/$1 [QSA,L,R=302]'; \
    echo '</VirtualHost>'; \
  } > /etc/apache2/conf-enabled/custom.conf

# Assume the id of the host user is 1000.
RUN usermod -u 1000 www-data && chown -R www-data:www-data /var/www
