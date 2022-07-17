# Hadoop-Assignment
Steps to install Hadoop:
    1. Make sure java is installed. 
java -version

If java is not installed, then type in the following commands:
sudo apt-get install update
sudo apt-get update
sudo apt-get install default-jdk 
Make sure now java is installed. 
java -version

![alt text]()



    2. Install ssh server
sudo apt-get install ssh-server
Generate public/private RSA key pair. 
ssh-keygen -t rsa -P “” 
When prompted for the file name to save the key, press Enter (leave it blank).

Type the following commands:
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
ssh localhost
exit


    3. Install Hadoop by navigating to the following link and downloading the tar.gz file for Hadoop version 3.3.0 (or a later version if you wish). (478 MB)
https://hadoop.apache.org/release/3.3.0.html

    4. Once downloaded, open the terminal and cd to the directory where it is downloaded (assume the desktop for example) and extract it as follows:
cd Desktop
sudo tar -xvzf hadoop-3.3.0.tar.gz
You can now check that there is an extracted file named hadoop-3.3.0 by typing the command “ls” or by visually inspecting the files. 
    5. Now, we move the extracted file to the location /usr/local/hadoop
sudo mv hadoop-3.3.0 /usr/local/hadoop
    6. Let’s configure the hadoop system. 
Type the following command:
sudo gedit ~/.bashrc 
At the end of the file, add the following lines: (Note: Replace the java version with the version number you already have. You can navigate to the directory /usr/lib/jvm and check the file name java-xx-openjdk-amd64)
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/native"










    7. Save the file and close it. 
    8. Now from the terminal, type the following command:
source ~/.bashrc 
    9. We start configuring Hadoop by opening hadoop-env.sh as follows:
sudo gedit /usr/local/hadoop/etc/hadoop/hadoop-env.sh
Search for the line starting with export JAVA_HOME= and replace it with the following line. 
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
Save the file by clicking on “Save” or (Ctrl+S)










    10.  Open core-site.xml as follows:
sudo gedit /usr/local/hadoop/etc/hadoop/core-site.xml
    Add the following lines between the tags <configuration> and </configuration> and  save it (Ctrl+S).
<property>
	<name>fs.default.name</name>
	<value>hdfs://localhost:9000</value>
</property>








    11.  Open hdfs-site.xml as follows:
sudo gedit /usr/local/hadoop/etc/hadoop/hdfs-site.xml
    Add the following lines between the tags <configuration> and </configuration> and save it (Ctrl+S).
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
<property>
 <name>dfs.namenode.name.dir</name>
 <value>file:/usr/local/hadoop_space/hdfs/namenode</value>
</property>
<property>
 <name>dfs.datanode.data.dir</name>
 <value>file:/usr/local/hadoop_space/hdfs/datanode</value>
</property>











    12.  Open yarn-site.xml as follows:
sudo gedit /usr/local/hadoop/etc/hadoop/yarn-site.xml
    Add the following lines between the tags <configuration> and </configuration> and save it (Ctrl+S)
 <property>
   <name>yarn.nodemanager.aux-services</name>
   <value>mapreduce_shuffle</value>
 </property>
 <property>
   <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
   <value>org.apache.hadoop.mapred.ShuffleHandler</value>
 </property>



    13.  Open mapred-site.xml as follows:
sudo gedit /usr/local/hadoop/etc/hadoop/mapred-site.xml
    Add the following lines between the tags <configuration> and </configuration> and save it (Ctrl+S)
 <property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
 </property>
 <property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
 </property>
 <property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
 </property>
 <property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
 </property>















    14. Now, run the following commands on the terminal to create a directory for hadoop space, name node and data node. 

    • sudo mkdir -p /usr/local/hadoop_space
    • sudo mkdir -p /usr/local/hadoop_space/hdfs/namenode
    • sudo mkdir -p /usr/local/hadoop_space/hdfs/datanode
Now we have successfully installed Hadoop. 
    15. Format the namenode as follows: 
hdfs namenode -format

This step should end by shutting down the namenode as follows: 






    16. Before starting the Hadoop Distributed File System (hdfs), we need to make sure that the rcmd type is “ssh” not “rsh” when we type the following command 
    • pdsh -q -w localhost

    17. If the rcmd type is “rsh” as in the above figure, type the following commands:
export PDSH_RCMD_TYPE=ssh
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
Run Step 16 again to check that the rcmd type is now ssh.
If not, skip that step. 

    18. Start the HDFS System using the command. 
start-dfs.sh
    19. Start the YARN using the command
start-yarn.sh

    20. Type the following command. You should see an output similar to the one in the following figure.
jps


Make sure these nodes are listed: (ResourceManager, NameNode, NodeManager, SecondaryNameNode, Jps and DataNode).


    21. Go to localhost:9870 from the browser. You should expect the following



Steps to run WordCount Program on Hadoop:
    1. Make sure Hadoop and Java are installed properly
hadoop version
javac -version

    2. Create a directory on the Desktop named Lab and inside it create two folders; one called “Input” and the other called “tutorial_classes”. 
[You can do this step using GUI normally or through terminal commands] 
cd Desktop
mkdir Lab
mkdir Lab/Input
mkdir Lab/tutorial_classes

    3. Add the file attached with this document “WordCount.java” in the directory Lab

    4. Add the file attached with this document “input.txt” in the directory Lab/Input. 

    5. Type the following command to export the hadoop classpath into bash.
export HADOOP_CLASSPATH=$(hadoop classpath)
Make sure it is now exported. 
echo $HADOOP_CLASSPATH
    6. It is time to create these directories on HDFS rather than locally. Type the following commands.
hadoop fs -mkdir /WordCountTutorial 
hadoop fs -mkdir /WordCountTutorial/Input
hadoop fs -put Lab/Input/input.txt /WordCountTutorial/Input
    7. Go to localhost:9870 from the browser, Open “Utilities → Browse File System” and you should see the directories and files we placed in the file system. 
    8. Then, back to local machine where we will compile the WordCount.java file. Assuming we are currently in the Desktop directory.
cd Lab
javac -classpath $HADOOP_CLASSPATH -d tutorial_classes WordCount.javaPut the output files in one jar file (There is a do
t at the end)
jar -cvf WordCount.jar -C tutorial_classes .

    9. Now, we run the jar file on Hadoop.
hadoop jar WordCount.jar WordCount /WordCountTutorial/Input /WordCountTutorial/Output

    10.  Output the result:
hadoop dfs -cat /WordCountTutorial/Output/*


























Requirement:
Vodafone Egypt is launching a marketing campaign in Ramadan to promote their sales and increase their profit from selling the prepaid recharge cards. These cards are worth 5, 10, 15, 50, and 100 EGP. 

The data science team at Vodafone are analyzing the customers’ data which include the customer personal information, the prepaid card they purchased, the timestamp they registered the prepaid amount on their Vodafone accounts, among other information. 

The details of the customers are omitted, and you are only provided with a file “in.csv” which includes two columns. 
    1. Customer ID. (Each ID maps to a certain customer, whose data is hidden for confidentiality). 
    2. Prepaid Card Amount. 

Your task is to generate a report using MapReduce (similar to the WordCount program) showing the total amount of prepaid cards for each customer that they have purchased.  For example, if a customer with ID 300 purchased 5 cards with 10, 15, 15, 10, 100, then the report should include that customer ID 300 bought cards with a total amount of 150. 







Disclaimer: Thanks to Vodafone DS team who provided us with this real customer data. 
