FROM library/ubuntu:14.04
RUN apt-get update

#RUN apt-get install -y wget
#RUN wget https://bootstrap.pypa.io/get-pip.py
#RUN python get-pip.py



RUN apt-get install -y python-dev libxml2-dev libxslt-dev
RUN apt-get install -y php5-mcrypt python-pip
RUN apt-get update && \
    apt-get install -y python python-dev python-pip python-virtualenv && \
    rm -rf /var/lib/apt/lists/*

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

RUN apt-get update

RUN apt-get install -y apache2



RUN apt-get install -y software-properties-common python-software-properties
RUN add-apt-repository ppa:sumo/stable
RUN apt-get update
RUN apt-get -f install
RUN apt-get install -y sumo sumo-tools sumo-doc

RUN pip install cherrypy

RUN apt-get install -y git
RUN echo 'new version with ENV variable code'
RUN git clone https://github.com/sriharrsha/c2sumo.git
RUN mv c2sumo/src/C3STEM/ /opt/

RUN mkdir -p /opt/C3STEM/Middleware/session
RUN mkdir -p /opt/C3STEM/Middleware/log/apache
RUN chown -hR www-data /opt/C3STEM/Middleware/session
RUN chown -hR www-data /opt/C3STEM/Middleware/log
RUN chown -hR www-data /opt/C3STEM/Middleware/static/Data

RUN ls /opt/C3STEM/webserver-config/etc/apache2/sites-available/
RUN cp -r /opt/C3STEM/webserver-config/etc/apache2/sites-available/* ./etc/apache2/sites-available/
RUN cp -r /opt/C3STEM/webserver-config/etc/apache2/* ./etc/apache2/

RUN apt-get install -y libapache2-mod-wsgi

RUN pip install pymongo
RUN pip install docker

RUN apt-get install -y python-bcrypt
RUN apt-get install -y python-cffi

RUN pip install --upgrade setuptools
RUN apt-get install -y python-dev
RUN pip install --upgrade pip enum34
RUN pip install paramiko
RUN pip install gviz_api
RUN apt-get install -y subversion
RUN /bin/bash -c "cd /opt \
		&& svn co https://github.com/google/google-visualization-python \
		&& python /opt/google-visualization-python/trunk/setup.py install"


RUN echo 'editing application.conf'
RUN echo '[DB]\n\
ip = mongodb://mongodb:27017\n\
\n\
[SIMULATION]\n\
# simulation update rate\n\
UPDATE_RATE = 2' > /opt/C3STEM/Middleware/config/application.conf

RUN apt-get install openssl
RUN /bin/bash -c "mkdir ~/.ssh \
		&& ssh-keygen -t rsa -f ~/.ssh/id_rsa -C "sri.harsha.seethalam@vanderbilt.edu" \
		&& openssl rsa -in ~/.ssh/id_rsa -outform pem > ~/.ssh/id_rsa.pem \
		&& chown -hR www-data ~/.ssh/id_rsa.pem \
		&& cp ~/.ssh/id_rsa.pem /opt/C3STEM/Middleware/"

RUN echo 'import pymongo\n\
import os\n\
from pymongo import MongoClient\n\
from bson.objectid import ObjectId\n\
\n\
connection = MongoClient(os.getenv("MONGO_URI"))\n\
db = connection.c3stem_database\n\
ip = "127.0.0.1"\n\
db.virtualmachine.insert({\n\
    "_id": ip,\n\
    "flavor": "m1.medium",\n\
    "private_IP": ip,\n\
    "public_IP": ip,\n\
    "key_nam": "id_rsa",\n\
    "type": "TRANSIENT",\n\
    "status": "AVAIL",\n\
    "user": "ALL",\n\
    "mode": "GROUP"});' >/opt/C3STEM/DB/cloud.py

RUN echo 'Now you should be able to login to http://localhost username and passwords are the ones in the file /opt/C3STEM/DB/users.csv'

CMD service apache2 restart
EXPOSE 80

ADD dbinit.sh /
RUN chmod +x ./dbinit.sh

#DATABASE Initialization and Starting Apache


