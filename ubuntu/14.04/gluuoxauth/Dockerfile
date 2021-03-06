FROM gluubase

MAINTAINER Shouro <shouro@gluu.org>

RUN groupadd -r tomcat && useradd -r -g tomcat tomcat

RUN apt-get update && apt-get install -y \
    openjdk-7-jre-headless \
    wget \
    unzip \
    apache2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Tomcat
RUN wget -q http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.65/bin/apache-tomcat-7.0.65.tar.gz -O /tmp/tomcat.tar.gz \
    && tar -xzf /tmp/tomcat.tar.gz -C /opt \
    && mv /opt/apache-tomcat-7.0.65 /opt/tomcat \
    && rm -f /tmp/tomcat.tar.gz

# Install oxAuth
RUN wget -q http://ox.gluu.org/maven/org/xdi/oxauth-server/2.4.1.Final/oxauth-server-2.4.1.Final.war -O /tmp/oxauth.war \
    && unzip -qq /tmp/oxauth.war -d /opt/tomcat/webapps/oxauth \
    && rm -f /tmp/oxauth.war

# Install Jython
RUN wget -q http://central.maven.org/maven2/org/python/jython-standalone/2.7.0/jython-standalone-2.7.0.jar -O /tmp/jython-standalone-2.7.0.jar \
    && unzip -q /tmp/jython-standalone-2.7.0.jar -d /opt/jython \
    && ln -sf /opt/jython/Lib /opt/tomcat/webapps/oxauth/WEB-INF/lib/Lib \
    && rm -f /tmp/jython-standalone-2.7.0.jar

# Directory for certificates
RUN mkdir -p /etc/certs

# Directory for scripts, e.g. duo_web.py
RUN mkdir -p /opt/tomcat/conf/python

# Directory for custom oxAuth context
RUN mkdir -p /opt/tomcat/conf/Catalina/localhost

# Ports required by tomcat
EXPOSE 8009
EXPOSE 8005

# Ports required by Apache
EXPOSE 80
EXPOSE 443
