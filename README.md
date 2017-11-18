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
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/core-site.xml
* cp /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml.template /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/mapred-site.xml
* vi /usr/local/Cellar/hadoop/2.8.2/libexec/etc/hadoop/hdfs-site.xml
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

