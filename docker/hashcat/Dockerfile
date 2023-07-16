FROM docker.io/gennady73/oneapi-opencl-ubi8.x86_64

RUN groupadd oneapi \
    && useradd -ms /bin/bash hashcat -g oneapi \
    && useradd -ms /bin/bash myuser \
    && passwd -d myuser

RUN mkdir /hashcat \
    && mkdir /hashcat/tmp \
    && chown -R hashcat:oneapi /hashcat \
    && chmod -R 2775 /hashcat

COPY --chown=hashcat:oneapi ./hashcat-6.2.6 /hashcat

USER hashcat