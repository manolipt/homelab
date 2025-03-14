FROM nextcloud:latest

RUN apt-get update && apt-get install -y \
  curl sudo && \
  rm -rf /var/lib/apt/lists/*

COPY apache.conf /etc/apache2/conf-available/custom-nextcloud.conf
RUN a2enconf custom-nextcloud

COPY post-installation /docker-entrypoint-hooks.d/post-installation
RUN chmod +x /docker-entrypoint-hooks.d/post-installation

CMD ["apache2-foreground"]
