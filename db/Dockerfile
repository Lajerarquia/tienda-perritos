FROM mysql:8

ENV MYSQL_ROOT_PASSWORD=admin123
ENV MYSQL_DATABASE=tienda_perritos
ENV MYSQL_USER=tienda_user
ENV MYSQL_PASSWORD=tienda_pass

COPY init.sql /docker-entrypoint-initdb.d/

EXPOSE 3306
