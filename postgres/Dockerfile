FROM postgres:10.0-alpine
LABEL maintainer="Sheroz Khaydarov http://sheroz.com"

USER postgres

# RUN chmod 0700 /var/lib/postgresql/data &&\
RUN initdb /var/lib/postgresql/data \
&&  echo "local all  all   trust" >> /var/lib/postgresql/data/pg_hba.conf \
&&  echo "host all  all    0.0.0.0/0  trust" >> /var/lib/postgresql/data/pg_hba.conf \
&&  echo "host all  all    127.0.0.1/32  trust" >> /var/lib/postgresql/data/pg_hba.conf \
&&  echo "listen_addresses='*'" >> /var/lib/postgresql/data/postgresql.conf \
&&  pg_ctl start \
&&  psql -U postgres -tc "SELECT 1 FROM pg_database WHERE datname = 'main'" | grep -q 1 || psql -U postgres -c "CREATE DATABASE athena_dev" \
&&  psql --command "ALTER USER postgres WITH ENCRYPTED PASSWORD 'postgres';"

EXPOSE 5432
