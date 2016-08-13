##
# ubunto16.04 plus Oracle JDK 8 and Java Cryptography Extension
##
FROM          piegsaj/oracle-jre:1.8.0_72-b15
MAINTAINER    Jeffrey Hu <hsuanninghu@gmail.com>

ENV WILDFLY_VERSION 10.0.0.Final
ENV JBOSS_HOME      /opt/wildfly
ENV BUILD_PACKAGES  curl
ENV RUNTIME_PACKAGES pwgen

ADD run.sh /
ADD create_wildfly_admin_user.sh /

##
# JAVA Cryptography Extension from folder
##
ADD US_export_policy.jar /
ADD local_policy.jar /

##
# Download & Create Wildfly 10.0.0.Final
##
RUN apt-get update && apt-get install -y $RUNTIME_PACKAGES $BUILD_PACKAGES --no-install-recommends && apt-get clean -qq && \
    curl -Ls "http://download.jboss.org/wildfly/$WILDFLY_VERSION/wildfly-$WILDFLY_VERSION.tar.gz" \
    | tar -xzC /opt --no-same-owner && \
    apt-get remove -qq --purge -y $BUILD_PACKAGES $(apt-mark showauto) && rm -rf /var/lib/apt/lists/* && \

    ln -s /opt/wildfly-$WILDFLY_VERSION $JBOSS_HOME && \
    groupadd -r wildfly -g 433 && \
    useradd -u 431 -r -g wildfly -d $JBOSS_HOME -s /bin/false -c "WildFly user" wildfly && \
    chmod +x /create_wildfly_admin_user.sh /run.sh && \
    chown -R wildfly:wildfly /opt/wildfly*

##
# Copy JAVA Cryptography Extension to JAVA_HOME
##
COPY US_export_policy.jar /usr/lib/jvm/java-8-oracle/jre/lib/security/US_export_policy.jar
COPY local_policy.jar /usr/lib/jvm/java-8-oracle/jre/lib/security/local_policy.jar

##
# remove JCE
##
RUN rm /local_policy.jar
RUN rm /US_export_policy.jar

##
# My Source Code to Wildfly & DataSource
##
ADD WildflyTest.war /opt/wildfly/standalone/deployments
ADD standalone.xml /
ADD sqljdbc4-3.0.jar /
COPY standalone.xml /opt/wildfly/standalone/configuration/
COPY sqljdbc4-3.0.jar /opt/wildfly/standalone/deployments
RUN rm /standalone.xml
RUN rm /sqljdbc4-3.0.jar
EXPOSE 8080 9990

USER wildfly


CMD ["/run.sh"]
