

Blog Repository
=========== 

* [Assignment 1A](https://github.com/rubigdata/bigdata-blog-2019-tendr-un/blob/master/Assignment1A.md).

* [Assignment 2](https://github.com/rubigdata/bigdata-blog-2019-tendr-un/blob/master/assignment2.md).

As I believe you need certain permissions to view these files, Assignment 2 is also posted here in its entirety.
##
Assignment 2:

# Hadoop: HDFS and Map Reduce

The tutorial
============
Before running Map-Reduce, the environment for HDFS had to be prepared.

* The command '*bin/hdfs namenode -format*' will start the NameNode, then format it and shut it down. 
* '*sbin/start-dfs.sh*' and '*sbin/stop-dfs.sh*' start and stop the HDFS.
* '*bin/hdfs dfs*' will preceed filesystem commands when in Hadoop. '*bin/hdfs dfs -mkdir /user*' creates a directory called user.
* '*bin/hdfs dfs -put etc/hadoop input*' copies the contents in 'etc/hadoop' to the destination 'input'
* '*bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar grep input output 'dfs[a-z.]+'*' finds and puts the entries from the jar file that match the string into the output file

Counting the number of words using MapReduce
============================================

An example code for MapReduce was taken from [Hadoop's MapReduce tutorial](https://hadoop.apache.org/docs/r2.9.2/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html#Source_Code). This implementation is called WordCount, and counts the number of times each word in the given input data shows up.

The mapping function in WordCount is map(Object key, Text value, Context context), and this  function processes each line in the document one at a time, and then makes a key-value pair for each word and the amount of times it encounters that word. A sample of the output from 100.txt/Complete Shakespeare:
 

> wolvish	2
>
> wolvish-ravening	1
>
> woman	162
>
> woman!	18
>
> woman'd.	1
>
> woman's	65


By using the terminal to count all the occurances of 'Romeo' and 'Juliet' (in different keys) in the output file given by WordCount, I find 313 instances of Romeo, and 206 of Juliet. 
