# @see: https://github.com/llaumgui/docker-images
FROM httpd:2.4-alpine

# Enable some modules.
# RUN sed -i 's@^#LoadModule deflate_module modules/mod_deflate.so@LoadModule deflate_module modules/mod_deflate.so@g' /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule expires_module modules/mod_expires.so@LoadModule expires_module modules/mod_expires.so@g' /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule ssl_module modules/mod_ssl.so@LoadModule ssl_module modules/mod_ssl.so@g'                 /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule rewrite_module modules/mod_rewrite.so@LoadModule rewrite_module modules/mod_rewrite.so@g' /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule proxy_module modules/mod_proxy.so@LoadModule proxy_module modules/mod_proxy.so@g'         /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule proxy_http_module modules/mod_proxy_http.so@LoadModule proxy_http_module modules/mod_proxy_http.so@g'             /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so@LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so@g'             /usr/local/apache2/conf/httpd.conf && \
#     sed -i 's@^#LoadModule socache_shmcb_module modules/mod_socache_shmcb.so@LoadModule socache_shmcb_module modules/mod_socache_shmcb.so@g' /usr/local/apache2/conf/httpd.conf
RUN sed -i 's@^#LoadModule rewrite_module modules/mod_rewrite.so@LoadModule rewrite_module modules/mod_rewrite.so@g'              /usr/local/apache2/conf/httpd.conf && \
    sed -i 's@^#LoadModule proxy_module modules/mod_proxy.so@LoadModule proxy_module modules/mod_proxy.so@g'         /usr/local/apache2/conf/httpd.conf && \
    sed -i 's@^#LoadModule proxy_http_module modules/mod_proxy_http.so@LoadModule proxy_http_module modules/mod_proxy_http.so@g'             /usr/local/apache2/conf/httpd.conf && \
    sed -i 's@^#LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so@LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so@g'  /usr/local/apache2/conf/httpd.conf

# Use 443
# RUN sed -i 's@Listen 80@Listen 80\nListen 443@g' /usr/local/apache2/conf/httpd.conf

# Custom configuration.
RUN echo 'IncludeOptional conf/conf.d/*.conf' >> /usr/local/apache2/conf/httpd.conf

# EXPOSE 443
WORKDIR /var/www
