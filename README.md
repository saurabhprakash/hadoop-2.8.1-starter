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


