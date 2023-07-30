FROM docker.io/gennady73/hashcat

USER root
RUN yum --disableplugin=subscription-manager -y update \
    && yum --disableplugin=subscription-manager -y install wget yum-utils make gcc openssl-devel bzip2-devel libffi-devel zlib-devel \
    && yum --disableplugin=subscription-manager clean all \
    && rm -rf /var/cache/yum    

# Get Python 3.10 source code
COPY python3.10-packages/Python-3.10.12 /Python-3.10.12

WORKDIR /Python-3.10.12/

# Install alternative version of Python beside distro supplied
# https://superuser.com/questions/686980/how-to-install-alternative-version-of-python-beside-distro-supplied
# Prepare the source code with the required values before compiling:
#
RUN ./configure --prefix=/usr --enable-optimizations --with-system-ffi --with-computed-gotos --enable-loadable-sqlite-extensions \
    && make -j ${nproc} \
    && make install
    #altinstall

# Python 3.10 on RHEL 8
# https://bugzilla.redhat.com/show_bug.cgi?id=1779294
# https://www.declarativesystems.com/2019/09/18/rhel8-no-python.html
# https://developers.redhat.com/blog/2019/05/07/what-no-python-in-red-hat-enterprise-linux-8
#
RUN if alternatives --display python | grep -q 'no-python'; then \
  alternatives --remove python /usr/libexec/no-python; \
fi \
&& alternatives --install /usr/bin/unversioned-python \
                       python \
                       /usr/bin/python3 \
                       300 \
             --slave   /usr/bin/python \
                       unversioned-python \
                       /usr/bin/python3 \
             --slave   /usr/share/man/man1/python.1.gz \
                       unversioned-python-man \
                       /usr/share/man/man1/python3.1.gz \
&& update-alternatives --list

# Cleanup 
RUN cd / && rm -rf /Python-3.10.12

# Adapt containers to run on Red Hat OpenShift Container Platform
# https://gitlab.com/gitlab-org/charts/gitlab/-/issues/1069 
# https://developers.redhat.com/blog/2020/10/26/adapting-docker-and-kubernetes-containers-to-run-on-red-hat-openshift-container-platform# 
#
RUN chgrp -R 0 /hashcat \
 && chown hashcat:0 /hashcat \
 && chmod g+rws /hashcat


USER hashcat 
WORKDIR /hashcat/
EXPOSE 8080

ENTRYPOINT ["python3", "-m", "http.server", "8080"]