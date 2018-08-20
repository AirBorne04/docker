FROM resin/rpi-raspbian:stretch

LABEL author="Stefan Krüger"

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    apt-utils \
    wget \
    ca-certificates \
    gnupg

RUN wget https://homegear.eu/packages/Release.key && apt-key add Release.key && rm Release.key
RUN echo 'deb https://homegear.eu/packages/Raspbian/ stretch/' >> /etc/apt/sources.list.d/homegear.list

RUN apt-get update && apt-get install -y \
    homegear \
    homegear-nodes-core \
    homegear-homematicbidcos \
    homegear-philipshue

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/etc/homegear", "/var/lib/homegear", "/var/log/homegear"]

EXPOSE 2001 2002 2003

CMD service homegear start && tail -f /var/log/homegear/homegear.log
