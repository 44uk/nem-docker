FROM maven:3-openjdk-11 AS builder
ARG VERSION=""
ARG REPO_URL=https://github.com/NemProject/nis-client.git
WORKDIR /_work

RUN apt-get update -y && apt-get upgrade -y && apt-get install -y \
  git

RUN git clone "$REPO_URL" nis-client && cd nis-client \
  && if [ "$VERSION" = "" ]; then \
    VERSION=$(git describe --tags `git rev-list --tags --max-count=1`) \
  ; fi \
  && echo "$VERSION" \
  && git checkout -b "$VERSION" \
  && sed -ie 's/git@github.com:/https:\/\/github.com\//g' .gitmodules \
  && git submodule update --init --recursive \
  && git submodule foreach './setup_java9.sh'

RUN cd nis-client \
  && mvn clean package -DskipTests=true \
  && rm -rf package/libs/*.jar package/nis/*.jar \
  && cp -r nis/target/libs/*.jar package/libs \
  && rm package/libs/nem-* \
  && cp -r nis/target/libs/nem-*.jar package/nis \
  && cp -r nis/target/nem-*.jar package/nis \
  && mkdir -p /app && mv package /app/nis-client

# docker run --entrypoint /bin/bash -it --rm -v $(pwd):/package maven:3-openjdk-11
# jdeps --multi-release 9 --module-path . --print-module-deps --ignore-missing-deps ./**/*.jar

# docker run -it --entrypoint /bin/bash --rm nis-client

# docker run --rm -v $(pwd):/package maven:3-openjdk-11 jdeps  --module-path .  --print-module-deps --ignore-missing-deps /package/**/*.jar
# RUN JDEPS=$(jdeps --multi-release 9 --module-path . --print-module-deps --ignore-missing-deps /app/nis-client/**/*.jar) \
#   && jlink --compress=2 --strip-debug --no-header-files --no-man-pages --add-modules "${JDEPS}" --output custom-jre \
#   && mv custom-jre /app/jre

# FROM gcr.io/distroless/base
# FROM maven:3-openjdk-11
FROM openjdk:11-jre-slim
COPY --from=builder /app/ /app/
WORKDIR /app/nis-client/nis
EXPOSE 7778
EXPOSE 7890
# ENTRYPOINT [ "/app/jre/bin/java" ]
ENTRYPOINT [ "java" ]
CMD [ "-Xms4G", "-Xmx6G", "-cp", ".:./*:../libs/*", "org.nem.deploy.CommonStarter" ]
