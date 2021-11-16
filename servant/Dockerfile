FROM openjdk:11-jdk-slim AS builder
ARG VERSION="0_0_4"
ARG BASE_URL=https://bob.nem.ninja
WORKDIR /_work

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
  curl jq git autoconf make libtool unzip

RUN curl -L ${BASE_URL}/servant_${VERSION}.zip > servant_${VERSION}.zip \
  && mkdir -p /app && unzip servant_${VERSION}.zip -d /app

FROM openjdk:11-jre-slim
COPY --from=builder /app/ /app/
WORKDIR /app/servant
EXPOSE 7880
ENTRYPOINT [ "java" ]
CMD [ "-Xms256M", "-Xmx256M", "-cp", ".:jars/*", "org.nem.rewards.servant.NodeRewardsServant" ]
