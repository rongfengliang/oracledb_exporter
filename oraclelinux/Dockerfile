FROM golang:1.15 AS build
WORKDIR /go/src/oracledb_exporter
ENV  GO111MODULE=on
ENV  GOPROXY=https://goproxy.cn
COPY . .
RUN go build -v -ldflags "-X main.Version=${VERSION} -s -w"

FROM oraclelinux:7-slim
ARG release=19
ARG update=9
RUN  yum -y install oracle-release-el7 && \
     yum -y install oracle-instantclient${release}.${update}-basic && \
     rm -rf /var/cache/yum && \
     yum clean all
COPY --from=build /go/src/oracledb_exporter/oracledb_exporter /oracledb_exporter
ADD ./default-metrics.toml /default-metrics.toml
EXPOSE 9161
ENTRYPOINT ["/oracledb_exporter"]
