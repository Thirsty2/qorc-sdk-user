ARG PARENT_IMAGE="qorc-sdk:latest"
FROM ${PARENT_IMAGE} as base

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/Los_Angeles

RUN apt-get update && apt-get install -y \
    git \
    libusb-1.0-0 \
    python3-pip \
    usbutils \
    udev && \
    rm -rf /var/lib/apt/lists/*

FROM base as release-candidate

ARG USER=ic
ARG UID=1000
ARG GID=1000
RUN addgroup --gid ${GID} ${USER}
RUN adduser --uid ${UID} --gid ${GID} --gecos "" --disabled-password --shell /bin/bash ${USER} 
RUN usermod -a -G plugdev ${USER} && usermod -a -G dialout ${USER}
COPY 98-quickfeather.rules /etc/udev/rules.d/98-quickfeather.rules
USER ${USER}
WORKDIR /home/${USER}

SHELL [ "/bin/bash" ]
COPY conda-user.sh /scripts/conda-user.sh

ENTRYPOINT [ "/scripts/conda-user.sh" ]

FROM release-candidate as qorc-sdk-user

# Below are some example commands to build and run a docker container.
# From within the dockerfiles directory in a terminal window type:
#
# docker build . -t qorc-sdk-user
# 
# In a directory containing a qorc-sdk project:
# docker run -it --rm -v $(pwd):/home/ic/work qorc-sdk-user bash
#
# The above will execute bash within the running container.  Any
# tools called thru bash will run within the container.
#
# You can run the container with an optional command, which it 
# passes to bash.  For instance to run 'ls' in the container:
#
# docker run --rm -v $(pwd):/home/ic/work qorc-sdk-user ls
#
# To work with the hardware, you may need something like the following:
# docker run --privileged -it --rm --device-cgroup-rule "c 166:* rwm" --device-cgroup-rule "c 189:* rwm" -v /dev/bus:/dev/bus:ro -v /dev/serial:/dev/serial:ro -v $(pwd):/home/ic/work qorc-sdk-user bash
# 
# I would like to get away from requiring --privileged.  Still working on it.