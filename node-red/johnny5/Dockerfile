FROM arm32v7/node:slim

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#unzip, build-essential required for pigpio installation
#sudo required for raspi-io installation
#kmod needed for modprobe commands for johnny5 command in node-RED
#python-imaging for the display
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    apt-utils \
    wget \
    unzip \
    build-essential \
    sudo \
    kmod \
    python-dev \
    python-imaging \
    git

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /root/python
RUN mkdir -p /root/.node-red

#installing pigpio, required for raspi-io
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

#install LCD driver
WORKDIR /root/python
RUN git clone https://github.com/adafruit/Adafruit_Nokia_LCD.git && \
    cd Adafruit_Nokia_LCD && \
    python setup.py install
    #cd .. && \
    #rm -R Adafruit_Nokia_LCD

#install node-RED
WORKDIR /root/.node-red
RUN npm install -g --unsafe-perm node-red

#install johny5 and dependencies for use with node-RED
RUN npm install -g johnny-five --unsafe-perm
RUN npm install -g raspi-io --unsafe-perm
RUN npm install -g node-red-contrib-gpio --unsafe-perm

#install node module for gpio access
#RUN npm install -g onoff --unsafe-perm

VOLUME /root/.node-red
VOLUME /root/python

EXPOSE 1880

RUN npm install

CMD ["node-red-pi","-v","--max-old-space-size=512","flow.json"]
