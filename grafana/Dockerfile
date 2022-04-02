# FROM grafana/grafana:5.0.0-beta1
FROM grafana/grafana AS base-image
ADD ./provisioning /etc/grafana/provisioning
ADD ./config.ini /etc/grafana/config.ini
ADD ./dashboards /var/lib/grafana/dashboards
# RUN curl -o /etc/nginx.conf https://prps-cf-strangler-resources.s3-us-gov-west-1.amazonaws.com/nginx.conf
# RUN cat /etc/nginx.conf
RUN grafana-cli plugins install grafana-simple-json-datasource
# RUN service grafana-server start

FROM base-image AS image-tools
USER root
RUN --mount=type=cache,id=image-tools-/var/cache/apk,target=/var/cache/apk \
    --mount=type=cache,id=image-tools-/var/lib/apk,target=/var/lib/apk
RUN apk update
RUN apk add git
RUN apk add openjdk11
RUN java --version \
    git --version
EXPOSE 8600
# RUN apk add make
# RUN wget -O /etc/apk/jdk  https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
# echo "https://apk.corretto.aws/" >> /etc/apk/repositories

#  wget -O /etc/jdk17.tar.gz https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.tar.gz
# tar -zxvf jdk17.tar.gz

FROM image-tools AS image-prod
RUN git clone https://github.com/DevSecOps-PRPS/sonarqubeapi.git /etc/sonarqubeapi
RUN nohup java -jar /etc/sonarqubeapi/sonarqubeapi/target/exposeapi-0.0.1-SNAPSHOT.jar &
