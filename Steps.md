![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/118a9ed1-9490-4290-b198-518cbafc8acf)# Setting up a Single Node Cluster on Centos 7

## Installing Java Development Kit 8u33, current user name is Centos is hadoop
-Pre-configuration Setup
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
1)Open the browser and go to the url https://hadoop.apache.org/releases.html.
Scroll down and click on Apache release archive to see past historical versions of Hadoop to download. Since our objective is to install an older Hadoop version 2.10.2.
You will be directed to a file listing, for Hadoop 2.10.2 it is located in the stable2/ folder. 
Then right click on Hadoop-2.10.2.tar.gz  and select copy link address to get the url path. 

2) In your home directory download the hadoop file 2.20.2, from the url u get above. Once completed there should be a hadoop-2.10.2.tar.gz in your home directory
```
wget https://archive.apache.org/dist/hadoop/common/stable2/hadoop-2.10.2.tar.gz
```
2) Extract the file of hadoop. This will unzip your files
```
tar -xzvf ~/hadoop-2.10.2.tar.gz
```
3) configure the bashprofile to add in the environmental path. in your home directory
```
gedit ~/.bash_profile
```
4)copy below files and paste in your .bash_profile file. then save your file
```
export JAVA_HOME=/home/hadoop/jdk1.8.0_333 
export JRE_HOME=/home/hadoop/jdk1.8.0_333/jre 
export HADOOP_HOME=/home/hadoop/hadoop-2.10.2 
export HADOOP_INSTALL=$HADOOP_HOME 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export HADOOP_COMMON_HOME=$HADOOP_HOME 
export HADOOP_HDFS_HOME=$HADOOP_HOME 
export HADOOP_YARN_HOME=$HADOOP_HOME 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
PATH=$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$JRE_HOME/bin:$PATH 
export PATH
```

5)Type below to see the output of your jave. (e.g /home/hadoop/jdk1.8.0_333)
```
$JAVA_HOME
```

6) Type below to see the output of your hadoop home directory (e.g. /home/hadoop/hadoop-2.10.2/)
```
$HADOOP_HOME 
```

7) Set the hadoop configuration files location type
```
gedit /home/hadoop/hadoop-2.10.2/etc/hadoop/hadoop-env.sh
```
8) And remove all lines and type below 2 in lines into the hadoop-env.sh. Then save it
```
export JAVA_HOME=/home/hadoop/jdk1.8.0_333 
export HADOOP_OPTS=-Djava.net.preferIPv4Stack=true

```
9) Type below to see both the java version and hadoop version
```
java -version 
hadoop version
```
##Modifying your hostname

1)Edit your network file
```
sudo gedit /etc/sysconfig/network
```

2) Enter below into the network file. then save your network file
```
HOSTNAME = hadoop618c
```

3) Restart the systemd
```
sudo systemctl restart systemd-hostnamed
```

4) Check if your hostname has been changed. Type below
```
hostname
```

##Configure the password-less connection between nodes 

1) Make sure if you are in the home directory and then to generate the rsa private and public key type below
```
ssh-keygen -t rsa
```

2) Copy the contents of the id_rsa.pub to authorized_keys file
```
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

3) Change user access rights to the authorized keys
```
chmod 600 ~/.ssh/authorized_keys
```

##Configuration of Hadoop
All files are found in local directory under ~/hadoop2.10.2/etc/Hadoop/
###Configuring core-site.xml

1) To amend core-site.xml
```
gedit ~/hadoop-2.10.2/etc/hadoop/core-site.xml
```
2) Then type in below between the configuration tags
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/eaa2d14b-b384-4004-8979-959a39dfc667)

###Configuring hdfs-site.xml
1) To amend hdfs-site.xml
```
gedit ~/hadoop-2.10.2/etc/hadoop/hdfs-site.xml
```

2) Then type in below between the configuration tags
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/e453dd5e-8b2d-41e1-8607-874f1d059684)

###Configuring mapred-site.xml
1) To amend mapred-site.xml
```
gedit ~/hadoop-2.10.2/etc/hadoop/mapred-site.xml
```
2) Then type in below between the configuration tags
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/0f91c8ad-a138-4667-af0f-5d4244ebf0e4)

###Configuring yarn-site.xml
1) To amend yarn-site.xml
```
gedit ~/hadoop-2.10.2/etc/hadoop/yarn-site.xml
```
2) Then type in below between the configuration tags
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/36717a87-8c7e-48d9-9422-bff73f69e295)

##Formatting Hadoop Namenode
1) Create a Hadoop Distributed File System by formatting
```
hdfs namenode -format
```
2) Check the namespace is created after the HDFS formatting
```
 ls –lah /home/hadoop/hadoop-2.10.2/dfs/name/current
```
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/e31da718-8dbf-4933-952e-caeb16304613)


3) Start Hadoop
```
start-dfs.sh
```
4) Start Yarn
```
start-yarn.sh
```

5) Check to see the java processing system
```
jps
```
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/c3654520-3f4a-4b2d-ad2c-7505c1d8bcca)



##Setting up the word count application
1) Getting a sample text from gutenberg
```
wget www.gutenberg.org/files/100/100-0.txt
```
2) Rename the file from 100-0.txt to shakespeare2.txt
```
mv 100-0.txt shakespeare2.txt
```
3) Create a directory in the Hadoop directory File System. This is where the input of your files will be.
```
hdfs dfs -mkdir /user/hadoop/input
```

4) 	Copying the file from local directory to the Hadoop distributed file system. This will be the location where the text document will be used for the wordcount
```
hdfs dfs -put ~/Documents/shakespeare2.txt /user/hadoop/input
```
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/df6ad222-4c85-4d16-9447-8b7367f2b483)

5) Run the jar application to do the wordcount of shakespeare2.txt this will send out to the output folder:
```
hadoop jar /home/hadoop/hadoop-2.10.2/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.10.2.jar wordcount /user/hadoop/input/shakespeare2.txt output
```
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/bb6884a1-fdfc-426e-b3e6-a8aab41d08a8)

6) Below is the output from the Hadoop cluster http://hadoop618c:8088/cluster/apps. This shows the application has running successfully.
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/ecfe2064-4c58-4af7-94a7-2efd37ba9f10)

7) When I check the output folder of /user/Hadoop/output and view the file part-r-00000
```
hdfs dfs -cat /user/hadoop/output/part-r-00000
```
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/cd4dca77-240c-47f3-a3a5-1bf89604a67f)

#Reflection on setting up a single node Hadoop cluster

It was a fascinating experience to set up a Single Node Hadoop system and learn about distributed computing and big data processing. Even though the main goal was to learn more about Hadoop's complexities, the process also revealed several difficulties and new perspectives that improved my comprehension of the system's design, problems, and possible solutions.
##Recognizing the Need
Knowing why Hadoop was necessary in the first place was essential before beginning any setup. In today's digital age, data is growing at an exponential rate, making traditional data processing methods inadequate. Hadoop offered a scalable solution with its distributed processing and storage capabilities. One way to get a peek into this world and create the groundwork for larger distributed systems would be to set up a Single Node Hadoop.
##Setup and Configuration at First
First, the Hadoop distribution was downloaded and the environment variables were set up. Although the first stages appeared simple, there were several subtleties that emerged during the configuration stage. Java and other requirements needed to be installed and set correctly, and this was crucial. Because skipping a stage could cause more serious problems down the line, this method required extreme attention to detail.
##Encountered Problems and Challenges
 ** Version Compatibility:** Ensuring version compatibility between Hadoop and other components, such as Java, was one of the initial issues faced. It is crucial to carefully check compatibility matrix because an incompatible version may cause unforeseen issues. What I did was to go back to lecture slides and install the Java JDK accordingly and follow the steps accordingly to update the appropriate JDK

 ** Configuration Issues: ** It is crucial to use proper parameters especially during the configuration of the xml files i.e. core-site.xml, hdfs-site.xml, mapred-site.xml, yarn-site.xml Misconfigurations, including wrong parameter values or file paths, frequently resulted in failures that needed a lot of troubleshooting. 
This section, was a bit difficult as the slides and the videos was based on a multi-node cluster, and uses master and slave in the configuration, as my hostname was hadoop618c, I had to troubleshoot and test accordingly after the implementation
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/8b876c70-9355-47ef-bf73-f3b17daea104)
![image](https://github.com/HassanHosain/BigData-hadoopSingleNode/assets/77096784/57dccebf-6c7a-4a46-83f2-4999821d215c)


** Data Node and Name Node Understanding Issues:** While in the lectures we were guided into constructing the multi-node cluster, when creating a single node cluster there was some confusion that arises as certain steps are not required like setting up a static ip for multiple nodes.

##Learning and Insights:
**Importance of Documentation:** As the setup process progressed, it became clear how important comprehensive documentation was. Keeping thorough documentation of setups, modifications, and troubleshooting procedures aided in the prompt resolution of problems and proved to be an invaluable resource for subsequent undertakings.

**Appreciation for Distributed Systems:** While configuring a Single Node Hadoop revealed details about the architecture of the system, it also brought to light the difficulties that come with distributed systems. Larger clusters present greater issues for fault tolerance, performance optimization, and data management across several nodes.

**Community Support:** Making use of community resources, documentation, and online forums was really helpful. Interacting with the Hadoop community helped solve complex issues and promoted group problem-solving and learning. Some of the resources that help me was https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html
and https://www.tecmint.com/install-hadoop-single-node-on-centos-7/. This is in addition to the videos and slides from the lecture Videos of Dr Foo.

##In summary
Beyond only technical expertise, setting up a Single Node Hadoop system was a satisfying experience. It provided a comprehensive grasp of data management issues, distributed computing concepts, and system optimizations. Even though there were many difficulties along the way.

This is a good experience for me as with the ever-growing amounts of data handled by companies /organizations, what I learn is that it is very beneficial to create multi parallel processing to assist more efficient process of data.
What I would do different is that I would learn to use java to create a java application and convert it into a jar file to process a file, instead of using the sample wordcount application. Albeit even without the application it has indeed help me appreciate the conveniences and efficiency of multi parallel processing.

