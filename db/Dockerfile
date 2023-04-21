FROM mysql:latest
# Commenting out run and copy to reduce redundancy.
#RUN mkdir /data
COPY ./products.csv /products.csv
COPY ./users.csv /users.csv
COPY ./seedjob.sql /docker-entrypoint-initdb.d/seedjob.sql
#COPY ./setup.sql /docker-entrypoint-initdb.d/setup.sql

ENTRYPOINT ["/entrypoint.sh"]
CMD ["mysqld", "--secure-file-priv=/"]
#CMD ["--secure-file-priv=/"]
