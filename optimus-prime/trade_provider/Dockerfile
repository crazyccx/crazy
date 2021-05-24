FROM adoptopenjdk/openjdk14-openj9:jre-14.0.2_12_openj9-0.21.0 as builder
WORKDIR application
ARG JAR_FILE=target/application.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract
#COPY db/migration db/migration

FROM adoptopenjdk/openjdk14-openj9:jre-14.0.2_12_openj9-0.21.0
WORKDIR application
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/application/ ./
#COPY --from=builder application/db ./db
EXPOSE 8888
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher", "-Xshareclasses", "-Xquickstart", "-XX:+ShowCodeDetailsInExceptionMessages", "--logging.file.path=/application/logs"]
