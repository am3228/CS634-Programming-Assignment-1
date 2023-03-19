#### Angela Murphy
#### NJIT CS 643

# CS 643 Programming Assignment 1

You have to build an image recognition pipeline in AWS, using two EC2 instances, S3, SQS, and Rekognition. The assignment must be done in Java on Amazon Linux VMs.

## Setting up your AWS

First you would need to either create your own account or use the account that the TA has set up for us. Once this is completed then you would log into the AWS console.

### IAM SETUP

First: Go to IAM and click on access management then policies.
Second: Find and add the following three policies:  AmazonRekognitionFullAccess, AmazonS3FullAccess and AmazonSQSFullAccess.

### EC2 SETUP

First: Find and click on EC2 within the services section. Then click on launch instance and then click on launch instance again.
Then for application and OS Images choose Amazon Linux.
Instance type: t2.micro FREE TIER
Network settings - create security group. Create three for SSH HTTP and HTTPS all with a dropdown of MY IP.
Summary: Number of instances should be set to 2.
Last: Launch Instance

Once launching a window will pop up stating to create a key. Create this key as CS643 and then download key pair. Launch instances and wait until they both state running.

To connect to your instances you would need to submit the following into your terminal but not until you set up your configurations. You would need to do this for both separate instances.

$ ssh -i "<your pem folder path" ec2-user@<YOUR_INSTANCE_PUBLIC_DNS>

### Setting up Configurations

Navagate back into IAM --> Access Management --> Users

Then you would click into your user and generate an access key. Save the access key file onto your computer.

The credentials file should already exist on both of your EC2 instances, so in order for us to set these credentials we can do the following in the terminal.

aws configure

It will then prompt you to put in your credentials as long as you have aws cli downloaded.

Once a session ends - you will need to ssh into each instance and enter these credentials before moving forward.

### Installing Java on both instances

SSH into both instances with the commands above under EC2 setup and download java on each one with the following commands.

$ sudo yum install java-1.8.0-devel
$ sudo /usr/sbin/alternatives --config java

### Installing/Updating Java and Maven locally on your terminal

sudo apt install openjdk-11-jre-headless
sudo apt install openjdk-11-jdk-headless
sudo apt install maven

### Cloning Git Repository and Saving Jar Files

Use the following commands in your terminal 

$ git clone <your github repository https link>
$ cd cs643-program-1/CarText
$ mvn clean install package
$ cd ../CarRecognize
$ mvn clean install package
$ cd ..

### Running on each EC2

We would then run the following commands:

First for EC2-B

scp -i \Users\am322\IdeaProjects\CarRecognize\CarText\target\CarText-1.0-SNAPSHOT.jar ec2-user@<YOUR_PUBLIC_DNS_FOR_EC2_B>:~/carText.jar

Then for EC2-A

scp -i \Users\am322\IdeaProjects\CarRecognize\CarText\target\CarText-1.0-SNAPSHOT.jar ec2-user@<YOUR_PUBLIC_DNS_FOR_EC2_A>:~/carText.jar

Then we would need to SSH into EC2-B and then enter the following commands

$ cd ~
$ java -jar ~/CarText.jar

Things should start to run but will not reflect fully until we run EC2-A

SSH into EC2-A and enter the following commands

$ cd ~
$ java -jar ~/CarRecognize.jar

Both programs will be running at the same time now. Once both finish running a file named output.txt will be found on EC2-B in the home directory.
