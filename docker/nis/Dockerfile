FROM debian:bookworm-slim AS builder

ARG VERSION="0.6.102"
ARG BASE_URL=https://github.com/NemProject/nem/releases/download

WORKDIR /_workspace

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
  curl git jq

RUN curl -sL ${BASE_URL}/v${VERSION}/nis-${VERSION}.tgz > nis-${VERSION}.tgz \
  && mkdir -p /app/nem \
  && tar zxf nis-${VERSION}.tgz -C /app/nem --strip-components 1 \
  && sed -i.bak 's|nem.folder = %h/nem|nem.folder = /data/nem|' /app/nem/nis/config.properties

FROM gcr.io/distroless/java:11

COPY --from=builder /app/ /app/
WORKDIR /app/nem/nis

EXPOSE 7778
EXPOSE 7890

ENTRYPOINT [ "java", "-cp", ".:./*:../libs/*", "org.nem.deploy.CommonStarter" ]
CMD [ "-Xms4G", "-Xmx6G" ]
