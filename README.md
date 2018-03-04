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
----------------------------------------------------
### Running wordcount example
* Inside samples directory:
	* Step1: Jar creation from WordCount.java
	``` 
		export JAVA_HOME=$(/usr/libexec/java_home)
		export PATH=${JAVA_HOME}/bin:${PATH}
		export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
		hadoop com.sun.tools.javac.Main WordCount.java
		jar cf wc.jar WordCount*.class
	```
* Put input files on location:
```
	$ hdfs dfs -put file01 /wordcount_input
	for checking files and content: 
	hadoop fs -ls /
	hadoop fs -cat /wordcount_input/
```
* For running: Inside samples directory:
	```hadoop jar wc.jar WordCount /wordcount_input /wordcount_output```
* Checking the contents of output: ```hadoop fs -cat /wordcount_output/part-r-00000```

Reference:
* https://medium.com/@luck/installing-hadoop-2-7-2-on-ubuntu-16-04-3a34837ad2db
* https://dtflaneur.wordpress.com/2015/10/02/installing-hadoop-on-mac-osx-el-capitan/
* http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_16_04_single_node_cluster.php
* https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Example:_WordCount_v2.0


#############################################################################################

For running on mac: https://amodernstory.com/2014/09/23/installing-hadoop-on-mac-osx-yosemite/

INSTALLING HADOOP ON MAC PART 1

A step by step guide to get your running with Hadoop today! In Hadoop on Mac part 2 we actually walk through the creation and compilation process of Java Hadoop Wordcount from beginning to end and automating it with .pom files. hadooplogo


Install HomeBrew
Install Hadoop
Configuring Hadoop
SSH Localhost
Starting and Stopping Hadoop
Good to know

Additional Resources
Github Wordcount example.
Install HomeBrew
Download it from the website at http://brew.sh/ or simply paste the script inside the terminal
```
	$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	Install Hadoop
	$ brew install hadoop
	Hadoop will be installed in the following directory
	/usr/local/Cellar/hadoop
```
Configuring Hadoop
```
Edit hadoop-env.sh
The file can be located at /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/hadoop-env.sh
where 2.6.0 is the hadoop version.

Find the line with

export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
and change it to

export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
Edit Core-site.xml
The file can be located at /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/core-site.xml .

<configuration>  
<property>
     <name>hadoop.tmp.dir</name>
     <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
     <description>A base for other temporary directories.</description>
  </property>
  <property>
     <name>fs.default.name</name>                                     
     <value>hdfs://localhost:9000</value>                             
  </property>
</configuration>    
Edit mapred-site.xml
The file can be located at /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/mapred-site.xml and by default will be blank.

<configuration>
<property>
<name>mapred.job.tracker</name>
<value>localhost:9010</value>
</property>
</configuration>
Edit hdfs-site.xml
The file can be located at /usr/local/Cellar/hadoop/2.6.0/libexec/etc/hadoop/hdfs-site.xml .

<configuration>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
</configuration>
To simplify life edit your ~/.profile using vim or your favorite editor and add the following two commands. By default ~/.profile might not exist.

alias hstart="/usr/local/Cellar/hadoop/2.6.0/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/start-yarn.sh"
alias hstop="/usr/local/Cellar/hadoop/2.6.0/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/stop-dfs.sh"
and execute
```

$ source ~/.profile
in the terminal to update.

Before we can run Hadoop we first need to format the HDFS using

```
$ hdfs namenode -format
```

SSH Localhost
Nothing needs to be done here if you have already generated ssh keys. To verify just check for the existance of ~/.ssh/id_rsa and the ~/.ssh/id_rsa.pub files. If not the keys can be generated using

```
$ ssh-keygen -t rsa
Enable Remote Login
“System Preferences” -> “Sharing”. Check “Remote Login”
Authorize SSH Keys
To allow your system to accept login, we have to make it aware of the keys that will be used

$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
Let’s try to login.

$ ssh localhost
Last login: Fri Mar  6 20:30:53 2015
$ exit
Running Hadoop
Now we can run Hadoop just by typing

$ hstart
and stopping using

$ hstop
```

Download Examples
To run examples, Hadoop needs to be started.

Hadoop Examples 1.2.1 (Old) 
Hadoop Examples 2.6.0 (Current)

Test them out using:
```
$ hadoop jar  pi 10 100
Good to know
We can access the Hadoop web interface by connecting to

Resource Manager: http://localhost:50070
JobTracker: http://localhost:8088
Specific Node Information: http://localhost:8042
```

Command
```
$ jps 
7379 DataNode
7459 SecondaryNameNode
7316 NameNode
7636 NodeManager
7562 ResourceManager
7676 Jps

$ yarn  // For resource management more information than the web interface. 
$ mapred  // Detailed information about jobs
This we can use to access the HDFS filesystem, for any resulting output files.
```

HDFS viewer

Errors
```
To resolve ‘WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform… using builtin-java classes where applicable’ (Stackoverflow.com)

Connection Refused after installing Hadoop
$ hdfs dfs -ls
15/03/06 20:13:54 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
ls: Call From spaceship.local/192.168.1.65 to localhost:9000 failed on connection exception: java.net.ConnectException: Connection refused; For more details see:   http://wiki.apache.org/hadoop/ConnectionRefused
The start-up scripts such as start-all.sh do not provide you with specifics about why the startups failed. Some of the time it won’t even notify you that a startup failed… To troubleshoot the service that isn’t functioning execute it manually.

$ hdfs namenode
15/03/06 20:18:31 WARN namenode.FSNamesystem: Encountered exception loading fsimage
org.apache.hadoop.hdfs.server.common.InconsistentFSStateException: Directory /usr/local/Cellar/hadoop/hdfs/tmp/dfs/name is in an inconsistent state: storage directory does not exist or is not accessible.
15/03/06 20:18:31 FATAL namenode.NameNode: Failed to start namenode.
and the problem is…

$ hadoop namenode -format
To verify the problem is fixed run

$ hstart
$ hdfs dfs -ls /
If ‘hdfs dfs -ls’ gives you a error

ls: `.': No such file or directory
then we need to create the default directory structure Hadoop expects (ie. /user/whoami_output/)

$ whoami
 spaceship
$ hdfs dfs -mkdir -p /user/spaceship 
 15/03/06 20:31:19 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
$ hdfs dfs -ls
 15/03/06 20:31:23 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
$ hdfs dfs -put book.txt
 15/03/06 20:32:29 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
$ hdfs dfs -ls 
 15/03/06 20:32:50 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
 Found 1 items
 -rw-r--r--   1 marekbejda supergroup      29578 2015-03-06 20:32 book.txt
JPS and Nothing Works…
Seems like certain builds of Java 1.8 (i.e.. 1.8_40) are missing a critical package that breaks Yarn. Check your logs at

$ jps
 5935 Jps
$ vim /usr/local/Cellar/hadoop/2.6.0/libexec/logs/yarn-*
 2015-03-07 16:21:32,934 FATAL org.apache.hadoop.hdfs.server.datanode.DataNode: Exception in secureMain java.lang.NoClassDefFoundError: sun/management/ExtendedPlatformComponent
..
 2015-03-07 16:21:32,937 INFO org.apache.hadoop.util.ExitUtil: Exiting with status 1
 2015-03-07 16:21:32,939 INFO org.apache.hadoop.hdfs.server.datanode.DataNode: SHUTDOWN_MSG:
http://mail.openjdk.java.net/pipermail/core-libs-dev/2014-November/029818.html

Either downgrade to Java 1.7 or I’m currently running 1.8.0_20

$ java -version
 java version "1.8.0_20"
 Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
 Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
I’ve Done Everything!! SSH still asks for a password!!!! OMGG!!!!
So I’ve ran across this problem today, all of a sudden ssh localhost started requesting a password. I’ve generated new keys and searched all day for an answer, thanks to this Apple thread.

$ chmod go-w ~/        
$ chmod 700 ~/.ssh       
$ chmod 600 ~/.ssh/authorized_keys    
```
