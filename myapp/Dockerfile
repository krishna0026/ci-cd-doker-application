FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    apache2 \
    libapache2-mod-wsgi-py3 \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Create app directory
RUN mkdir -p /var/www/myapp
WORKDIR /var/www/myapp

# Copy application files
COPY app.py app.wsgi ./

# Install Flask
RUN pip3 install flask

# Apache config
RUN echo '<VirtualHost *:80>\n\
    ServerName localhost\n\
    WSGIScriptAlias / /var/www/myapp/app.wsgi\n\
    <Directory /var/www/myapp>\n\
        Require all granted\n\
    </Directory>\n\
</VirtualHost>' > /etc/apache2/sites-available/myapp.conf

# Enable site
RUN ln -s /etc/apache2/sites-available/myapp.conf /etc/apache2/sites-enabled/myapp.conf

# Expose port
EXPOSE 80

# Run Apache in foreground
CMD ["apachectl", "-D", "FOREGROUND"]
