# Load-balancer-WebApp
This is a simple web app with AWS classic load balancer and Amazon Linux EC2


* Create a EC2 instance with Amazon Linux 

*Paste the given script given below in the User Data which is present in Advanced Section*

```#!/bin/bash

# Variable Declaration
#PACKAGE="httpd wget unzip"
#SVC="httpd"
URL='https://www.tooplate.com/zip-templates/2098_health.zip'
ART_NAME='2098_health'
TEMPDIR="/tmp/webfiles"

yum --help &> /dev/null

if [ $? -eq 0 ]
then
   # Set Variables for CentOS
   PACKAGE="httpd wget unzip"
   SVC="httpd"

   echo "Running Setup on CentOS"
   # Installing Dependencies
   echo "########################################"
   echo "Installing packages."
   echo "########################################"
   sudo yum install $PACKAGE -y > /dev/null
   echo

   # Start & Enable Service
   echo "########################################"
   echo "Start & Enable HTTPD Service"
   echo "########################################"
   sudo systemctl start $SVC
   sudo systemctl enable $SVC
   echo

   # Creating Temp Directory
   echo "########################################"
   echo "Starting Artifact Deployment"
   echo "########################################"
   mkdir -p $TEMPDIR
   cd $TEMPDIR
   echo

   wget $URL > /dev/null
   unzip $ART_NAME.zip > /dev/null
   sudo cp -r $ART_NAME/* /var/www/html/
   echo

   # Bounce Service
   echo "########################################"
   echo "Restarting HTTPD service"
   echo "########################################"
   systemctl restart $SVC
   echo

   # Clean Up
   echo "########################################"
   echo "Removing Temporary Files"
   echo "########################################"
   rm -rf $TEMPDIR
   echo

   sudo systemctl status $SVC
   ls /var/www/html/

else
    # Set Variables for Ubuntu
   PACKAGE="apache2 wget unzip"
   SVC="apache2"

   echo "Running Setup on CentOS"
   # Installing Dependencies
   echo "########################################"
   echo "Installing packages."
   echo "########################################"
   sudo apt update
   sudo apt install $PACKAGE -y > /dev/null
   echo

   # Start & Enable Service
   echo "########################################"
   echo "Start & Enable HTTPD Service"
   echo "########################################"
   sudo systemctl start $SVC
   sudo systemctl enable $SVC
   echo

   # Creating Temp Directory
   echo "########################################"
   echo "Starting Artifact Deployment"
   echo "########################################"
   mkdir -p $TEMPDIR
   cd $TEMPDIR
   echo

   wget $URL > /dev/null
   unzip $ART_NAME.zip > /dev/null
   sudo cp -r $ART_NAME/* /var/www/html/
   echo

   # Bounce Service
   echo "########################################"
   echo "Restarting HTTPD service"
   echo "########################################"
   systemctl restart $SVC
   echo

   # Clean Up
   echo "########################################"
   echo "Removing Temporary Files"
   echo "########################################"
   rm -rf $TEMPDIR
   echo

   sudo systemctl status $SVC
   ls /var/www/html/
fi 
```

* Launch the instance now


* You can create custom AMI for this project 

* Then launch that AMI instance 

<img width="1552" alt="image" src="https://user-images.githubusercontent.com/107933310/236505352-e856a4f1-5321-41cc-9497-fbce58362aee.png">

* Thus two instances will be visible in EC2

<img width="1552" alt="image" src="https://user-images.githubusercontent.com/107933310/236505430-23d17080-1b14-461e-b96d-0035d7b0b1eb.png">


***TO create a load balancer***

* First create a target group and do the necessary settings.

* Similarly create the load balancer and launch it .

<img width="1552" alt="Screenshot 2023-05-05 at 9 12 52 PM" src="https://user-images.githubusercontent.com/107933310/236505192-718c567c-99e8-46c8-a6a7-0a32bf1bad67.png">

* Change the security group inbound rules for AMI and EC2 image by allowing LB tcp connection.

*In target group check the healthy status for AMI anf EC2

<img width="1552" alt="image" src="https://user-images.githubusercontent.com/107933310/236512321-121d0bed-74d7-4ecc-9f46-bc7acc87b8c7.png">



Final Output:(Load Balancer DNS)

<img width="1552" alt="image" src="https://user-images.githubusercontent.com/107933310/236512462-7d9af91a-b90a-41ca-882f-a7e58a802fa8.png">









