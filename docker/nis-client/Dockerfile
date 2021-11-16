FROM openjdk:11-jre-slim-buster AS builder
ARG VERSION=""
ARG BASE_URL=https://bob.nem.ninja
ARG NODE_URL=http://hugealice.nem.ninja:7890
WORKDIR /_work

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
  curl jq
RUN if [ "$VERSION" = "" ]; then \
    VERSION=$(curl -Ls ${BASE_URL}/version.txt) \
  ; fi \
  ; curl -Ls ${BASE_URL}/nis-${VERSION}.tgz > nis-${VERSION}.tgz \
  && HASH=$(curl -Ls ${BASE_URL}/nis-${VERSION}.tgz.sig | grep txId | cut -d' ' -f2) \
  && SHA=$(curl -Ls ${NODE_URL}/transaction/get?hash=${HASH} | jq -r '.transaction.message.payload[10:74]') \
  && echo "${SHA} nis-${VERSION}.tgz" > checksum-${VERSION} \
  && sha256sum -c checksum-${VERSION} \
  && mkdir -p /app/nis && tar zxf nis-${VERSION}.tgz -C /app/nis --strip-components 1

FROM openjdk:11-jre-slim-buster
# FROM gcr.io/distroless/base
COPY --from=builder /app/ /app/
WORKDIR /app/nis/nis
EXPOSE 7778
EXPOSE 7890
# ENTRYPOINT [ "/app/jre/bin/java" ]
ENTRYPOINT [ "java" ]
CMD [ "-Xms4G", "-Xmx6G", "-cp", ".:./*:../libs/*", "org.nem.deploy.CommonStarter" ]
