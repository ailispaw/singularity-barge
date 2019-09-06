FROM ailispaw/ubuntu-essential:14.04-nodoc

ENV TERM=xterm \
    UID=1000 GID=1000

RUN apt-get -q update \
 && apt-get -q -y install --no-install-recommends ca-certificates adduser curl git \
      build-essential libssl-dev uuid-dev libseccomp-dev pkg-config cryptsetup \
 && apt-get clean && rm -rf /var/lib/apt/lists/* \
 && addgroup --gid ${GID} bargees \
 && adduser --home /home/bargee --shell /bin/bash --gecos '' --ingroup bargees --uid ${UID} --disabled-password bargee

USER bargee

ENV GOVERSION=1.11.4 \
    GOROOT=/home/bargee/.go \
    GOPATH=/home/bargee/go

ENV PATH=${PATH}:${GOROOT}/bin:${GOPATH}/bin

RUN mkdir -p ${GOROOT} \
 && curl https://dl.google.com/go/go${GOVERSION}.linux-amd64.tar.gz \
      | tar xvzf - -C ${GOROOT} --strip-components=1

VOLUME ${GOPATH}/src/github.com/sylabs/singularity

WORKDIR ${GOPATH}/src/github.com/sylabs/singularity

CMD [ "sh", "-c", "./mconfig -p /usr --sysconfdir=/etc --localstatedir=/var && make -C builddir" ]
