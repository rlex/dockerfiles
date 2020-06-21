FROM python:3.8.3-alpine
LABEL maintainer "rlex"

ENV PYTHONUNBUFFERED 1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1
ENV PIP_NO_CACHE_DIR=1
ARG FREETAKSERVER_VERSION=0.8.19.6.3
RUN apk add --no-cache -U libxslt libxml2 && \
  apk add libxslt-dev libxml2-dev build-base && \
  pip install FreeTAKServer==${FREETAKSERVER_VERSION} && \
  apk del libxslt-dev libxml2-dev build-base
RUN addgroup -g 1000 freetakserver \
  && adduser -Ss /bin/false -u 1000 -G freetakserver -h /home/freetakserver freetakserver \
  && mkdir -m 777 /data \
  && chown freetakserver:freetakserver /data /home/freetakserver

#FIXME: bug, FreeTAKServerDataPackageFolder gets created in site-packages
RUN mkdir -m 777 /data/fts_datapackages && \
  mkdir -m 777 /data/logs && \
  ln -s /data/fts_datapackages /usr/local/lib/python3.8/site-packages/FreeTAKServer/controllers/FreeTAKServerDataPackageFolder
EXPOSE 8087 8080
VOLUME ["/data"]
USER freetakserver
WORKDIR /data
ENTRYPOINT ["python", "-m", "FreeTAKServer.controllers.Orchestrator"]
