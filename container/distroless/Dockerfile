# https://github.com/GoogleContainerTools/distroless

FROM docker.io/library/python:3.9-slim as python-base
FROM gcr.io/distroless/python3-debian11:nonroot

ARG ARCH=x86_64-linux-gnu
ARG USER="app"
# ARG HOME="/home/${USER}"

COPY --from=python-base /usr/local/lib/ /usr/local/lib/
COPY --from=python-base /usr/local/bin/python /usr/local/bin/python
COPY --from=python-base /etc/ld.so.cache /etc/ld.so.cache

COPY --from=python-base /lib/${ARCH}/libz.so.1 /lib/${ARCH}/

COPY --from=python-base /bin/echo /bin/echo
COPY --from=python-base /bin/rm /bin/rm
COPY --from=python-base /bin/sh /bin/sh
# COPY --from=python-base /bin/mkdir /bin/mkdir

RUN set -xe && \
    echo ":${USER}:x:1000:${USER}" >> /etc/group && \
    echo ":${USER}:x:1001:" >> /etc/group && \
    echo ":${USER}:x:1000:1001::/home/:${USER}:" >> /etc/passwd

RUN set -xe && \
    # mkdir ${HOME} && \
    python --version && \
    rm -f /bin/sh /bin/echo /bin/rm

USER ${USER}
# WORKDIR ${HOME}

# standardise on locale, don't generate .pyc, enable tracebacks on seg faults
# ENV LANG C.UTF-8
# ENV LC_ALL C.UTF-8
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONFAULTHANDLER 1

ENTRYPOINT ["/usr/local/bin/python"]
