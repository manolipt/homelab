FROM nextcloud:latest

COPY apache.conf /etc/apache2/conf-available/custom-nextcloud.conf
RUN a2enconf custom-nextcloud

COPY post-installation.sh /docker-entrypoint-hooks.d/post-installation/post-installation.sh
RUN chmod +x /docker-entrypoint-hooks.d/post-installation/post-installation.sh

CMD ["apache2-foreground"]
