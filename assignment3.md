# Assignment 3

## Introduction
In this blogpost I present the experiences I have had with Spark and RDDs by doing Assignment 3, and this I do by explaining a bit about the theory behind it and answering the questions asked in the two notebooks in assignment 3A.

## Spark 
The idea behind Spark is to use a Resilient Distributed Dataset (RDD), which has partitions to divide the big data into smaller pieces, and distributes them across the nodes in a cluster. It uses parallel operators to manipulate the data in parallel, and will reconstruct intermediate results if there is a failure. 

## The contents of assignment 3A
In assignment 3A, I was acquainted with notebooks, initializing RDDs with unspecified and specified amounts of partitions using Spark, and what that means for the results. I saw how query processing is affected by the choice of operators, and how they use or don't use partitioners, and I learned that further ways to control the parallelism is with repartition and coalesce-operations.

## Questions (chronological order)
**Q: Explain why there are multiple result files.**  
We have the result files part-00000 and part-00001. There are two files due to there being two RDD partitions. 

**Q: why are the counts different?**   
The first collect() done on 'Macbeth' gets the result 30. The second gets the result 285. The difference between these is  that some
preprocessing was made for the second one - 

    val words = lines.flatMap(line => line.split(" "))  
                  .map(w => w.toLowerCase().replaceAll("(^[^a-z]+|[^a-z]+$)", ""))  
                  .filter(_ != "")  
                  .map(w => (w,1))  

                  .reduceByKey( _ + _ )


so all the Macbeth-variations are counted instead of ones that are exactly and only written as 'Macbeth'. 



**Q: Spot the differences between the results, and try to map what you see on Chapter 2 that we read for the course.**  
The results are 

![alt text](ass3_b_rddpairs.png "Results rddA and rddB")

rddPairGroup has 8 partitions, and rddPairGroup2 has 2 - because this one was initialized as 
`val rddPairsPart2 = rddPairs.partitionBy(new HashPartitioner(2))`. As the notebook explains, the default number of partitions will depend on the number of cores in the machine running docker - the computer I ran this on presumably had 8 cores.



**Q: do you understand why the partitioner is none? (No worries if not, you will find a clue below.)**  
When we do the partitionBy(new HashPartitioner(x)) (like we did with val rddPairsPart4 = rddPairs.partitionBy(new HashPartitioner(4))) 
we get the result:

>Number of partitions: 4
>res40: Option[org.apache.spark.Partitioner] = Some(org.apache.spark.HashPartitioner@4)>
>
>Some(org.apache.spark.HashPartitioner@4)

The none-partitioner means no partitioner was assigned.

**Q: Why are the results different for rddA and rddB? How is query processing affected by the partitioners?**  
rddA uses map(), which omits partitioners, and rddB uses mapValues(), an operation which guarantees that the output distribution will carry over an existing partitioner to its result. Query processing will be affected by partitioners if operations that carry those over are used.

**Q: Compare the two query plans for rddC and rddD. Can you explain why the second query plan has on less shuffle phase?**  
rddC is defined as a repartition with input 2 done on rddA, and rddD is defined as a coelesce operation done on rddB. coalesce() does not execute a full shuffle, and keeps the partitioner from rddB. repartition() does a full shuffle and creates 2 (=the number specified in the input) output partitions.

