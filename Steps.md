# Setting up a Single Node Cluster on Centos 7

## Installing Java Development Kit 8u33, current user name is Centos is hadoop

1) There are many ways to install Java in Centos 7, this is via the url browser and downloading the file
Go to https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html and click "Java Archive".
Find Java SE 8. and select Jave SE 8 (8u211 and later), find jdk-8u333-linux-x64.tar.gz and click download

2) In the Centos 7 VM go to your home directory>> downloads and find jdk-8u333-linux-x64.tar.gz that you have downloaded
```
cd ~/Downloads
```
3) Then move the file to your home directory
```
mv ~/Downloads/jdk-8u333-linux-x64.tar.gz ~
```
4) After which you unzip the file
```
tar -xzf ~/jdk-8u333-linux-x64.tar.gz
```
5) Install the unzipper tar file
```
sudo alternatives --install /usr/bin/java java /home/hadoop/jdk1.8.0_333
```
6) Run sudo alternatives and select the jdk1.8.0_333
```
sudo alternatives -config java
```
7) Check the java version
```
java -version
```

## Installing hadoop 2.10.2

1) In your home directory download the hadoop file 2.20.2
```
wget https://archive.apache.org/dist/hadoop/common/stable2/hadoop-2.10.2.tar.gz
```
2) Extract the file of hadoop
```
tar -xzvf ~/hadoop-2.10.2.tar.gz
```
3) configure the bashprofile to add in the environmental path
```
hadoop
```
