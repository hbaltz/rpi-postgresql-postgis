FROM tobi312/rpi-postgresql:9.4

# Pour le apt-get
ENV http_proxy http://10.0.4.2:3128
ENV https_proxy https://10.0.4.2:3128

ENV POSTGRES_USER docker
ENV POSTGRES_PASSWORD docker
ENV POSTGRES_DB docker

ENV POSTGIS_MAJOR 2.1
ENV POSTGIS_VERSION 2.1.*

RUN apt-get update \
      && apt-get install -y --no-install-recommends \
           postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR=$POSTGIS_VERSION \
           postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts=$POSTGIS_VERSION \
           postgis=$POSTGIS_VERSION \
      && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /docker-entrypoint-initdb.d
COPY ./initdb-postgis.sh /docker-entrypoint-initdb.d/postgis.sh
RUN chmod +x /docker-entrypoint-initdb.d/postgis.sh

RUN sed -e '90d' -i /usr/share/postgresql/9.4/pg_hba.conf.sample
RUN sed -e '91d' -i /usr/share/postgresql/9.4/pg_hba.conf.sample

RUN echo "host all all 0.0.0.0/0 trust" >> '/usr/share/postgresql/9.4/pg_hba.conf.sample'
RUN echo "local all all trust" >> '/usr/share/postgresql/9.4/pg_hba.conf.sample'

RUN sed -e "s/[#]\?listen_addresses = .*/listen_addresses = '*'/g" -i '/usr/share/postgresql/postgresql.conf.sample'
