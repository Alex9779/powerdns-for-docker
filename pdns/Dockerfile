FROM python:stretch
LABEL maintainer="JSenecal@connectitnet.com"

RUN apt-get update && apt-get install -y --no-install-recommends \
  curl \
  gnupg

COPY src/ /

ENV DEBIAN_FRONTEND=noninteractive
RUN curl https://repo.powerdns.com/FD380FBB-pub.asc | apt-key add - \
  && curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
  && apt-get update && apt-get install -y --no-install-recommends \
  pdns-server \
  pdns-backend-mysql \
  pdns-backend-pgsql \
  pdns-backend-sqlite3 \
  pdns-tools \
  postgresql-client \
  mysql-client \
  && apt-get purge -y --auto-remove curl gnupg \
  && rm -rf /var/lib/apt/lists/*

RUN pip3 install envtpl \
  && rm -rf ~/.cache/*

ENV VERSION=4.1 \
  PDNS_setuid=pdns \
  PDNS_setgid=pdns \
  PDNS_guardian=yes \
  PDNS_webserver=yes \
  PDNS_webserver_address=0.0.0.0 \
  PDNS_webserver_port=8081 
  

EXPOSE 53 53/udp 8081

COPY pdns.conf.jinja2 /etc/powerdns/
COPY docker-cmd.sh /

CMD [ "bash", "/docker-cmd.sh" ]
