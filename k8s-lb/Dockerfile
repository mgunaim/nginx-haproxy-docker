FROM nginx
COPY index.html /usr/share/nginx/html/index.html
RUN rm -f /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/nginx.conf