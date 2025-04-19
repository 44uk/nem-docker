FROM debian:bookworm-slim AS builder

ARG VERSION="0_0_4"
ARG BASE_URL=https://bob.nem.ninja

WORKDIR /_workspace

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
  autoconf curl git jq make libtool unzip

RUN curl -sL ${BASE_URL}/servant_${VERSION}.zip > servant_${VERSION}.zip \
  && mkdir -p /app \
  && unzip servant_${VERSION}.zip -d /app

# FROM openjdk:11-jre-slim-bullseye
FROM gcr.io/distroless/java:11

COPY --from=builder /app/ /app/
WORKDIR /app/servant

EXPOSE 7880

ENTRYPOINT [ "java", "-cp", ".:jars/*", "org.nem.rewards.servant.NodeRewardsServant" ]
CMD [ "-Xms256M", "-Xmx256M" ]
