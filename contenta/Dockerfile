FROM nginx:alpine

ADD cnginx.conf /etc/nginx/nginx.conf
ADD cnginx.default /etc/nginx/conf.d/default.conf

RUN ln -sf /dev/stdout /var/log/nginx/access.log \ && ln -sf /dev/stderr /var/log/nginx/error.log
CMD ["nginx-debug", "-g", "daemon off;"]
