FROM debian as base
ENV DEBIAN_FRONTEND=noninteractive

FROM base as build
RUN apt-get update && apt-get install -y \
        curl &&                          \
    apt-get clean &&                     \
    rm -rf /var/lib/apt/
RUN curl -JLo radarr.tar.gz                                                                          \
        'http://radarr.servarr.com/v1/update/master/updatefile?os=linux&runtime=netcore&arch=x64' && \
    tar -xf radarr.tar.gz &&                                                                         \
    mv Radarr/ /app/ &&                                                                              \
    rm radarr.tar.gz

FROM base
WORKDIR /app/
RUN apt-get update && apt-get install -y \
        curl                             \
        libicu67                         \
        sqlite3 &&                       \
    apt-get clean &&                     \
    rm -rf /var/lib/apt/
COPY --from=build /app/ .
RUN ls
COPY docker-entrypoint.sh /usr/local/bin/
VOLUME /data/ /config/
RUN useradd -m app
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["./Radarr", "--nobrowser", "--data=/config"]
