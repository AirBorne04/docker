FROM arm32v7/debian:latest

ENV TZ=Europe/Berlin
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y mosquitto && rm -rf /var/lib/apt/lists/*

EXPOSE 1883

CMD mosquitto