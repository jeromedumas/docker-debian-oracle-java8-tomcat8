FROM docker-debian-oracle-java8

MAINTAINER Jerome Dumas

ENV TOMCAT_MAJOR=8 \
    TOMCAT_VERSION=8.0.39 \
    TOMCAT_HOME=/opt/tomcat \
    CATALINA_HOME=/opt/tomcat \
    CATALINA_OUT=/dev/null

# Get  install Tomcat
RUN apt-get update -q && \
    apt-get install -q -y --no-install-recommends curl && \
    curl -jksSL -o /tmp/apache-tomcat.tar.gz http://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR}/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz && \
    gunzip /tmp/apache-tomcat.tar.gz && \
    tar -C /opt -xf /tmp/apache-tomcat.tar && \
    ln -s /opt/apache-tomcat-${TOMCAT_VERSION} ${TOMCAT_HOME} && \
    #rm -rf ${TOMCAT_HOME}/webapps/* && \
    apt-get remove --purge -y curl && \
    apt-get autoremove --purge -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add log configuration
COPY logging.properties ${TOMCAT_HOME}/conf/logging.properties

# Add server.xml configuration
COPY server.xml ${TOMCAT_HOME}/conf/server.xml

# Add admin/admin user
ADD tomcat-users.xml ${TOMCAT_HOME}/conf/

# Get and install PSI Probe
#RUN curl -fksSL https://github.com/psi-probe/psi-probe/releases/download/2.4.0.SP1/probe.war -o /tmp/probe.war && \
#    mv /tmp/probe.war ${TOMCAT_HOME}/webapps/probe.war

VOLUME "/logs"
VOLUME "/opt/tomcat/webapps"

EXPOSE 8080
EXPOSE 8009

# Launch Tomcat
#CMD ["${TOMCAT_HOME}/bin/catalina.sh", "run"]
