FROM registry.access.redhat.com/ubi8/ubi:8.0

MAINTAINER Red Hat Training <training@redhat.com>

LABEL io.openshift.expose-services="8080:http"

# DocumentRoot for Apache
ENV DOCROOT=/var/www/html 


RUN   yum install -y --nodocs --disableplugin=subscription-manager httpd && \
      yum clean all --disableplugin=subscription-manager -y && \
      echo "Hello from the httpd-parent container!" > ${DOCROOT}/index.html

# Allows child images to inject their own content into DocumentRoot
ONBUILD COPY src/ ${DOCROOT}/

EXPOSE 8080

# This stuff is needed to ensure a clean start
RUN rm -rf /run/httpd && mkdir /run/httpd &&
	chgrp -R 0 /var/log/httpd /var/run/httpd &&
	chmod -R g=u /var/log/httpd /var/run/httpd &&
	sed -i "s/Listen 80/Listen 8080/g" /etc/httpd/conf/httpd.conf

# Run as the root user
USER 1001

# Launch httpd
CMD /usr/sbin/httpd -DFOREGROUND
