FROM python:3.8.3-alpine
LABEL maintainer "rlex"

ENV PYTHONUNBUFFERED 1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV PIP_NO_CACHE_DIR=1
RUN apk add --no-cache -U \
  libxslt-dev \
  libxml2-dev \
  build-base
RUN addgroup -g 1000 freetakserver \
  && adduser -Ss /bin/false -u 1000 -G freetakserver -h /home/freetakserver freetakserver \
  && mkdir -m 777 /data \
  && chown freetakserver:freetakserver /data /home/freetakserver
ARG FREETAKSERVER_VERSION=0.8.19
RUN pip install FreeTAKServer==${FREETAKSERVER_VERSION}

#FIXME: Waiting for upstream fix to run FTS from /data, but for now, this ugly kludge should work
ENV PYTHONPATH /usr/local/lib/python3.8/site-packages/FreeTAKServer/controllers
#FIXME: another bug, FreeTAKServerDataPackageFolder creates in site-packages
RUN mkdir -m 777 /data/fts_datapackages && \
  ln -s /data/fts_datapackages /usr/local/lib/python3.8/site-packages/FreeTAKServer/controllers/FreeTAKServerDataPackageFolder
EXPOSE 8087 8080
VOLUME ["/data"]
USER freetakserver

WORKDIR /data
ENTRYPOINT ["python", "-m", "FreeTAKServer.controllers.Orchestrator"]