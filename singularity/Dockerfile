FROM ubuntu:18.04

ENV TERM=xterm \
    UID=1000 GID=1000

RUN apt-get -q update \
 && apt-get -q -y install --no-install-recommends ca-certificates adduser curl git \
      build-essential libseccomp-dev pkg-config cryptsetup \
 && apt-get clean && rm -rf /var/lib/apt/lists/* \
 && addgroup --gid ${GID} bargees \
 && adduser --home /home/bargee --shell /bin/bash --gecos '' --ingroup bargees --uid ${UID} --disabled-password bargee

USER bargee

ENV GOVERSION=1.18.1 \
    GOROOT=/home/bargee/.go \
    GOPATH=/home/bargee/go

ENV PATH=${PATH}:${GOROOT}/bin:${GOPATH}/bin

RUN mkdir -p ${GOROOT} \
 && curl https://dl.google.com/go/go${GOVERSION}.linux-amd64.tar.gz \
      | tar xvzf - -C ${GOROOT} --strip-components=1

VOLUME ${GOPATH}

WORKDIR ${GOPATH}/src/github.com/sylabs/singularity

CMD [ "sh", "-c", "./mconfig -p /usr --sysconfdir=/etc --localstatedir=/var && make -C builddir" ]
