FROM arm32v7/node:slim

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#unzip, build-essential required for pigpio installation
#python-imaging for the display
#git for display driver

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    apt-utils \
    wget \
    build-essential \
    unzip \
    python-dev \
    python-imaging \
    git \
    net-tools:armhf && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /root/python
RUN mkdir -p /root/.node-red

#installing pigpio
WORKDIR /root
RUN wget abyz.co.uk/rpi/pigpio/pigpio.zip && \
    unzip pigpio.zip && \
    cd PIGPIO && \
    make && \
    make install && \
    cd .. && \
    rm -R PIGPIO && \
    rm pigpio.zip

# install RPI.GPIO python libs
RUN wget https://sourceforge.net/projects/raspberry-gpio-python/files/raspbian-jessie/python-rpi.gpio_0.6.3%7Ejessie-1_armhf.deb && \
    dpkg -i python-rpi.gpio_0.6.3~jessie-1_armhf.deb && \
    rm python-rpi.gpio_0.6.3~jessie-1_armhf.deb

#install node-RED
WORKDIR /root/.node-red
RUN npm install -g --unsafe-perm node-red

#make volumes accessible
VOLUME /root/.node-red
VOLUME /root/python

EXPOSE 1880

RUN npm install

CMD ["node-red-pi","-v","--max-old-space-size=512","flow.json"]
