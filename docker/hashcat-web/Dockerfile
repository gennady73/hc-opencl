FROM docker.io/gennady73/hashcat

USER root
    
RUN yum --disableplugin=subscription-manager -y update \
    && yum --disableplugin=subscription-manager -y install python3 \
    && yum --disableplugin=subscription-manager -y install python3-pip \
    && yum --disableplugin=subscription-manager clean all \
    && rm -rf /var/cache/yum    

#RUN alternatives --set python /usr/bin/python3 \
#    && python --version
 
USER hashcat 
WORKDIR /hashcat/
EXPOSE 8080

ENTRYPOINT ["python3", "-m", "http.server", "8080"]
