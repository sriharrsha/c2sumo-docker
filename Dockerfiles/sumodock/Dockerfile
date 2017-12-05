FROM library/ubuntu:14.04
RUN apt-get update

RUN apt-get install -y software-properties-common python-software-properties
RUN add-apt-repository ppa:sumo/stable
RUN apt-get update
RUN apt-get -f install
RUN apt-get install -y sumo sumo-tools sumo-doc

EXPOSE 9999
