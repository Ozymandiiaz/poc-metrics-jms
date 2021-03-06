# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run:
#      $ sudo docker build -t 1213-osb-compact-domain .
#

# Pull base image
# ---------------
FROM jeqo/soa:12.2.1.2-quickstart

# Maintainer
# ----------
MAINTAINER ToBeDefined

# Copy scripts
#-------------
COPY container-scripts/createDomain.sh container-scripts/startDomain.sh container-scripts/create-osb-domain.py /u01/oracle/

USER root

RUN chmod +xr /u01/oracle/createDomain.sh && \
    chmod +xr /u01/oracle/startDomain.sh && \
    chown oracle:oracle -R /u01/oracle/*.sh

USER oracle

ENV DOMAIN_NAME="${DOMAIN_NAME:-base_domain}" \
    DOMAIN_HOME=/u01/oracle/user_projects/domains/${DOMAIN_NAME:-base_domain} \
    ADMIN_PORT="${ADMIN_PORT:-7001}" \
    PATH="$PATH:${DOMAIN_HOME}"

RUN /u01/oracle/createDomain.sh

WORKDIR ${DOMAIN_HOME}

COPY config.yml jmx_prometheus_javaagent-0.9.jar ${DOMAIN_HOME}/

ENV JAVA_OPTIONS="-javaagent:$DOMAIN_HOME/jmx_prometheus_javaagent-0.9.jar=5555:$DOMAIN_HOME/config.yml"

EXPOSE 5555

# Define default command to start script.
CMD ["/u01/oracle/startDomain.sh"]
