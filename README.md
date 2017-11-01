export JAVA_HOME=/usr/java/default
export PATH=${JAVA_HOME}/bin:${PATH}
export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar

hadoop com.sun.tools.javac.Main WordCount.java
jar cf wc.jar WordCount*.class


start-dfs.sh && start-yarn.sh 

hadoop fs -mkdir -p  hdfs://localhost:9000/user/<user>/data/wordcount

hadoop dfs -copyFromLocal  input  hdfs://localhost:9000/user/<user>/data/wordcount

hadoop jar wc.jar WordCount hdfs://localhost:9000/user/<user>/data/wordcount/input hdfs://localhost:9000/user/<user>/data/wordcount/output

hadoop fs -cat /user/<user>/datat/wordcount/output/part-r-00000

stop-yarn.sh && stop-dfs.sh

