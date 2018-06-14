# Using Ubuntu 16.04 image
FROM ubuntu:xenial

# Install PostgreSQL-9.5
RUN apt-get update
RUN apt-get install -y postgresql-9.5 postgresql-contrib-9.5

# Create user docker
USER postgres
RUN /etc/init.d/postgresql start &&\
 psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
 createdb -O docker docker_db


# Adjust PostgreSQL configuration so that remote connections to the
# database are possible.
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.5/main/pg_hba.conf

# And add ``listen_addresses`` to ``/etc/postgresql/9.5/main/postgresql.conf``
RUN echo "listen_addresses='*'" >> /etc/postgresql/9.5/main/postgresql.conf

# Expose the PostgreSQL port
EXPOSE 5432

# Add VOLUMEs to allow backup of config, logs and databases
VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

# Set the default command to run when starting the container
CMD ["/usr/lib/postgresql/9.5/bin/postgres", "-D", "/var/lib/postgresql/9.5/main", "-c", "config_file=/etc/postgresql/9.5/main/postgresql.conf"]
