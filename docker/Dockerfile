FROM nginx:1.21.1

WORKDIR /usr/share/nginx/html
COPY index.html index.html

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]