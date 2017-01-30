# Hadoop Installation
Hadoop Installation has some prerequisites -
1. Java
2. OpenSSH Server

Once you met these prereqs, follow the steps below:

1. Download Hadoop Binary using the following command:
wget http://apache.mirrors.tds.net/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz
2. Once done, use the following command to extract the binary:
tar -zxvf hadoop-2.7.3.tar.gz 
3. Now create your environment variables for Hadoop by adding the following strings at the end of the profile:
# -- HADOOP ENVIRONMENT VARIABLES START -- #
export JAVA_HOME=/usr/lib/jvm/java-8-oracle ##This is the JAVA Home. Could change if you installed JVM elsewhere
export HADOOP_HOME=/home/user/hadoop-2.7.3 ##This is the path to your uncompressed download
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
# -- HADOOP ENVIRONMENT VARIABLES END -- #
and then close the terminal and reopen it.
4. Now let's configure the Hadoop installation by modifying/creating the following files under /home/user/hadoop-2.7.3/etc/hadoop:
a. hadoop-env.sh - add the absolute reference to JAVA_HOME by updating {$JAVA_HOME} to /usr/lib/jvm/java-8-oracle
b. core-site.xml - add the following configuration within <configuration>	</configuration> tags:
<property>
	<name>fs.default.name</name>
	<value>hdfs://localhost:9000</value>
</property>
c. yarn-site.xml - add the following configuration within <configuration> </configuration> tags:
<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
</property>
<property>
		<name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
d. mapred-site.xml - add the following configuration within <configuration> </configuration> tags:
<property>
	<name>mapreduce.framework.name</name>
	<value>yarn</value>
</property>
5. There's further configuration to be done. But before that, let's create folders:
cd ~
sudo mkdir -p hadoop_tmp/hdfs/namenode
sudo mkdir -p hadoop_tmp/hdfs/datanode
6. Now let's fix the hdfs-site.xml. Add the following between <configuration> </configuration> tags:
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
	<property>
		<name>dfs.namenode.name.dir</name>
		<value>file:/home/user/hadoop_tmp/hdfs/namenode</value>
	</property>
	<property>
		<name>dfs.datanode.data.dir</name>
		<value>file:/home/user/hadoop_tmp/hdfs/datanode</value>
	</property>
7.Now let's fix some permissions:
sudo chown -R user:user /home/user/hadoop-2.7.3/etc/hadoop
sudo chown -R user:user /home/user/hadoop_tmp
8. Format the HDFS Namenode:
hdfs namenode -format
9. Now that's the end of the configuration. Let's get started.
start-all.sh
10. Here's URL: http://localhost:50070 to look at your stuff
