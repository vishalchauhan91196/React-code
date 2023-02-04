#=======================================================================================================

Appspec.yml file structure:


--------------------
appspec.yml
--------------------

version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/server
hooks:
  AfterInstall:
    - location: afterinstall.sh
      timeout: 300
  ApplicationStart:
    - location: applicationstart.sh
      timeout: 300

--------------------
afterinstall.sh
--------------------

#!/bin/bash

cd /home/ec2-user/server
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash -
yum -y install nodejs npm


#Remove Unused Code

rm -rf node_modules
rm -rf build

#Install node_modules & Make Build and install PM2

npm -f install
npm run build
npm install -g pm2


--------------------
applicationstart.sh
--------------------

#!/bin/bash

cd /home/ec2-user/server
sudo pm2 delete Frontend
sudo pm2 start server.js --name Frontend

#=======================================================================================================

