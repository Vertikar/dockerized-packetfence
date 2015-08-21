FROM ubuntu:12.04

# Set non-interactive session
RUN export DEBIAN_FRONTEND=noninteractive
# Prepare apt
RUN echo "deb http://fr.archive.ubuntu.com/ubuntu precise main multiverse" >> /etc/apt/sources.list
RUN echo "deb http://inverse.ca/downloads/PacketFence/ubuntu precise precise" > /etc/apt/sources.list.d/packetfence.list
RUN apt-key adv --keyserver keys.gnupg.net --recv-key 0x810273C4
RUN apt-get update -y
# Provision needed utilities: expect interpreter, supervisor
RUN apt-get install -y expect supervisor
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/supervisord.conf

# Install packetfence
RUN apt-get install -y packetfence

## clean apt
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["/usr/bin/supervisord"]
