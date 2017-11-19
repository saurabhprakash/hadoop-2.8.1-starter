# hadoop-2.8.1-starter


java setup commands:

 * sudo apt-get update
 * sudo apt-get install default-jdk

hadoop setup commands:


```hadoop setup commands
> wget http://www-eu.apache.org/dist/hadoop/common/hadoop-2.8.1/hadoop-2.8.1.tar.gz
(other release page: http://hadoop.apache.org/releases.html)
> tar -xzvf hadoop-2.8.1.tar.gz
> sudo mv hadoop-2.8.1 /usr/local/hadoop

Configuring Hadoop's Java Home
> vim /usr/local/hadoop/etc/hadoop/hadoop-env.sh
Use Readlink to Set the Value Dynamically
# export JAVA_HOME=${JAVA_HOME}
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")

Running Hadoop
> /usr/local/hadoop/bin/hadoop
```
Reference link: https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-in-stand-alone-mode-on-ubuntu-16-04


For OSX
After setup files edited:

* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/hadoop-env.sh
```
Change the line
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
to
export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kd
```
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/core-site.xml
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
        <description>A base for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
        <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
    </property>
</configuration>
```
* cp /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml.template /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml
```
<configuration>
<property>
  <name>mapred.job.tracker</name>
  <value>localhost:9010</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
</property>
</configuration>
```
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/hdfs-site.xml
<configuration>
<property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
 </property>
 <property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
 </property>
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
 </property>
</configuration>
* sudo chown -R saurabh:staff /usr/local/hadoop_store
* hadoop namenode -format

* vi ~/.profile
```
	"~/.profile addition"
	alias hstart="/usr/local/Cellar/hadoop/2.8.2/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/2.8.2/sbin/start-yarn.sh"
	alias hstop="/usr/local/Cellar/hadoop/2.8.2/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/2.8.2/sbin/stop-dfs.sh"
```

*source ~/.profile
*hdfs namenode -format

Reference:
	https://medium.com/@luck/installing-hadoop-2-7-2-on-ubuntu-16-04-3a34837ad2db
	https://dtflaneur.wordpress.com/2015/10/02/installing-hadoop-on-mac-osx-el-capitan/
	http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_16_04_single_node_cluster.php

