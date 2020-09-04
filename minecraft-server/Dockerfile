FROM openjdk:14-jdk-alpine as jdk
RUN apk add --no-cache git

FROM jdk as builder
ENV REV=1.16.2
ADD https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar ./BuildTools.jar
RUN java -jar BuildTools.jar --rev ${REV}

FROM jdk
ENV REV=1.16.2
ENV EULA=true
COPY --from=builder /spigot-1.16.2.jar .
WORKDIR /server
VOLUME /server
EXPOSE 25565
EXPOSE 8123
ENV JAVA_OPTS -Xms512m -Xmx1800m -XX:+UseConcMarkSweepGC
ENTRYPOINT ln -sf "../spigot-${REV}.jar" "spigot.jar" && \
  echo "eula=${EULA}" > eula.txt && \
  java -jar "spigot.jar" nogui

