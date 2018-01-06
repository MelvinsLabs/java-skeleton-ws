# Base Image is Debian SID with CURL
FROM buildpack-deps:sid-curl

# Install Utilities
RUN apt-get update \
    && apt-get install -y \
               procps

WORKDIR /opt/

# Install Oracle Java Development Kit (JDK)
RUN curl -fSL -b "oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u151-b12/e758a0de34e24606bca991d704f6dcbf/jdk-8u151-linux-x64.tar.gz -o java.tar.gz \
    && tar -xvzf java.tar.gz \
    && rm -rf java.tar.gz

ENV JAVA_HOME=/opt/jdk1.8.0_151
ENV PATH=$PATH:$JAVA_HOME/bin

# Install Apache Tomcat
RUN curl -fSL http://apache.claz.org/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz -o tomcat.tar.gz \
    && tar -xvzf tomcat.tar.gz \
    && rm -rf tomcat.tar.gz

ENV CATALINA_BASE=/opt/apache-tomcat-8.5.24
ENV PATH=$PATH:$CATALINA_BASE/bin

# Copy Tomcat AddOns
COPY tomcat/server.xml $CATALINA_BASE/conf/
COPY tomcat/*.jar $CATALINA_BASE/lib/
COPY tomcat/log4j2.xml $CATALINA_BASE/lib/

# Copy WS JAR
COPY ${service.artifactId}-${service.version}.war $CATALINA_BASE/webapps/${service.name}.war

# Expose Ports
# 8181 : WS Endpoint
EXPOSE 8181

## Start Tomcat When Container Is Ran
CMD ["catalina.sh", "run"]