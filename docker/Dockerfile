ARG BASE_IMAGE
FROM ${BASE_IMAGE}
ENV HOME /home/omar
USER root
ADD run-httpd.sh $HOME/run-httpd.sh
RUN mkdir -p $HOME
RUN yum --disablerepo=centos* -y install hostname
RUN yum --disablerepo=centos* -y install httpd
RUN yum --disablerepo=centos* -y install gettext
RUN yum -y install nss_wrapper
RUN yum -y install mod_ssl
RUN yum clean all
RUN setcap cap_net_bind_service+ep /usr/sbin/httpd
RUN setcap cap_net_bind_service+ep /usr/sbin/apachectl
RUN mkdir /etc/ssl/server-certs
RUN mkdir /etc/httpd/crl
RUN chmod -v +x $HOME/run-httpd.sh
RUN chmod 774 /usr/share/httpd
RUN chmod 774 /var/run/httpd
RUN chown 1001:0 /run/httpd
RUN chown 1001:0 /var/log/httpd
RUN chmod 774 /var/log/httpd
RUN chmod g+s /var/run/httpd
RUN ln -sf /proc/self/fd/1 /var/log/httpd/access_log
RUN ln -sf /proc/self/fd/1 /var/log/httpd/error_log
RUN ln -sf /proc/self/fd/1 /var/log/httpd/ssl_access_log
RUN ln -sf /proc/self/fd/1 /var/log/httpd/ssl_request_log
RUN ln -sf /proc/self/fd/1 /var/log/httpd/ssl_error_log
RUN find /usr/share/httpd -type d -exec chmod g+xws {} \;
RUN find /usr/share/httpd -type f -exec chmod g+rw {} \;
RUN find /etc/httpd -type d -exec chmod g+rw {} \;
RUN find /etc/pki -type d -exec chmod g+xws {} \;
RUN find /etc/pki -type f -exec chmod g+rw {} \;
RUN chown -R 1001:0 /usr/share/httpd
RUN chown -R 1001:0 /etc/httpd
RUN chown -R 1001:0 /etc/pki
RUN chown -R 1001:0 /etc/ssl
RUN find $HOME -type d -exec chmod g+xws {} \;
RUN find $HOME -type f -exec chmod g+rw {} \;
RUN chown -R 1001:0 $HOME
RUN chmod 777 $HOME/*.sh
RUN chmod 777 $HOME
RUN chown -R 1001:0 $HOME
EXPOSE 80 443
WORKDIR $HOME
USER 1001
CMD $HOME/run-httpd.sh
