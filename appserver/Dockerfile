FROM centos:8

LABEL maintainer "Thiago Mota <tgmspawn@gmail.com>"

RUN yum -y update \
    && yum -y install \
        dmidecode \
        fontconfig \
        glib2 \
        glibc \
        libstdc++ \
        libuuid \
        wget \
    && rm -rf /var/cache/yum/* \
    && yum clean all

RUN DUMB_INIT_SHA256="37f2c1f0372a45554f1b89924fbb134fc24c3756efaedf11e07f599494e0eff9" \
    && wget -O /usr/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.2/dumb-init_1.2.2_amd64 \
    && echo "37f2c1f0372a45554f1b89924fbb134fc24c3756efaedf11e07f599494e0eff9 */usr/bin/dumb-init" | sha256sum -c - \
    && chmod +x /usr/bin/dumb-init

COPY /build /build
# COPY ./downloads/tttp120.rpo /totvs/protheus/apo/tttp120.rpo

ADD ./downloads/APPSERVER_BUILD.TAR.GZ /totvs/protheus/bin/appserver
ADD ./downloads/SMARTCLIENT_WEBAPP.TAR.GZ /totvs/protheus/bin/appserver

RUN /build/setup.sh

EXPOSE 1234 80 81

WORKDIR /totvs/protheus/bin/appserver

ENTRYPOINT [ "/usr/bin/dumb-init", "--" ]

CMD [ "/usr/local/bin/my-init.sh" ]
