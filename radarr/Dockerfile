FROM debian:11 as base
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
        curl &&                          \
    apt-get clean &&                     \
    rm -rf /var/lib/apt/

FROM base as build
RUN curl -JL                                                                                        \
        'http://radarr.servarr.com/v1/update/master/updatefile?os=linux&runtime=netcore&arch=x64' | \
    tar -xz &&                                                                                      \
    mv Radarr/ /app/

FROM base
WORKDIR /app/
RUN apt-get update && apt-get install -y \
        libicu67                         \
        sqlite3 &&                       \
    apt-get clean &&                     \
    rm -rf /var/lib/apt/
COPY --from=build /app/ .
RUN ls
COPY docker-entrypoint.sh /usr/local/bin/
VOLUME /data/ /config/
RUN useradd -m app
EXPOSE 7878
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["./Radarr", "--nobrowser", "--data=/config"]
