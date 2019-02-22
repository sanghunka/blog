---
title: "[Intro to Hadoop and MapReduce] Lesson 3 HDFS and MapReduce"
metaAlignment: center
date: 2017-11-06 11:00:00+09:00
categories:
- dev
tags:
- hadoop
- study
- udacity
- intro-to-hadoop-and-map-reduce
thumbnailImage: //tr4.cbsistatic.com/hub/i/r/2014/02/05/7edb6d5b-1151-4824-a8b7-6bd554f0ded5/resize/770x/293413c3bb9555b7d22aac146fa8b928/hadoop.logo.tr.jpg
thumbnailImagePosition: left
---

<!--more-->

# 1. Quiz: HDFS

Is there a problem?
> https://youtu.be/6F8-cCUbRU8

- [x] Network failure
- [x] Disk failure on DN(datanode)
- [ ] Not all DN used
- [ ] Block sizes differ
- [x] Disk failure on NN(namenode)


# 2. Quiz: Data Redundancy

Any problem now?(when NN failure)

- [x] Data inaccessible 
> when network failure on NN
- [x] Data lost forever
> when disk failure on NN
- [ ] No problem


# 3. NameNode Standby

The active namenode works before, but the standby can be configured to take over if the active one fails.


# 4. HDFS Demo

- Hadoop fs commands like unix commands
- You can read instructions on how to access and run the virtual machines [here][1]

```sh
hadoop fs -ls
hadoop fs -put purchases.txt
hadoop fs -ls
hadoop fs -tail purchases.txt
hadoop fs -mv purchases.txt newname.txt
hadoop fs -rm newname.txt
hadoop fs -mkdir myinput
hadoop fs -put purchases.txt myinput
hadoop fs -ls myinput
```


# 5. MapReduce


# 6. Real World Example


# 7. Quiz: Hashtables

Hashtables
> Key -> Value problems?

- [ ] It won't work
- [x] Run out of memory
- [x] Long time
- [ ] Wrong answer


# 8. Distributed Work


# 9. Summary of MapReduce

Note: Hadoop takes care of the Shuffle and Sort phase. You do not have to sort the keys in your reducer code, you get them in already sorted order.


# 10. Quiz: Sort Final Result

Final results in sorted order?

- [ ] Impossible
- [x] Only one reducer
- [x] Extra step


# 11. Quiz: Multiple Reducers

There are 4 intermediates: Apple, Banana, Carrot, Grape
Which keys go to the first reducer?

- [ ] Apple, Banana
- [ ] Apple, Carrot
- [ ] Carrot, Grape
- [ ] Apple, Grape
- [ ] Don't Know; 2 Each
- [x] Don't Know

> Even One reducer would get none.
> See [a nice overview of partitioning in Hadoop][2]

# 12. Daemons of MapReduce

- Job Tracker
- Task Trackers


# 13. Running a Job

RUNNING A MAPREDUCE JOB WITH THE VM ALIAS
hs {mapper script} {reducer script} {input_file} {output directory}

```sh
hadoop jar /usr/lib/hadoop-0.20-mapreduce/contrib/streaming/hadoop-streaming-2.0.0-mr1-cdh4.1.1.jar -mapper mapper.py -reducer reducer.py -file mapper.py -file reducer.py -input myinput -output joboutput

hadoop fs -get joboutput/part-00000 mylocalfile.txt
```


# 14. Simplifying Things
# 15. A Different Application
# 16. Other Problems
# 17. Virtual Machine Setup

You can read instructions on how to download and run the virtual machines[here][3].

Information on how to transfer files back and forth to the virtual machine can be found [here][4].

For step-by-step instructions for how to load data into HDFS, please re-watch [HDFS Demo][5]. For a reminder of how to run a mapreduce job, please re-watch [Simplifying Things][6].

# 18. Conclusion

See more in the free [Chapter 6 of Tom White’s essential text, Hadoop: The Definitive Guide][7]


[1]: https://docs.google.com/document/d/1v0zGBZ6EHap-Smsr3x3sGGpDW-54m82kDpPKC2M6uiY/edit?usp=sharing
[2]: http://developer.yahoo.com/hadoop/tutorial/module5.html#partitioning
[3]: https://docs.google.com/document/d/1v0zGBZ6EHap-Smsr3x3sGGpDW-54m82kDpPKC2M6uiY/pub
[4]: https://docs.google.com/a/knowlabs.com/document/d/1MZ_rNxJhR4HCU1qJ2-w7xlk2MTHVqa9lnl_uj-zRkzk/pub
[5]: https://classroom.udacity.com/courses/ud617/lessons/308873795/concepts/3095085570923
[6]: https://classroom.udacity.com/courses/ud617/lessons/308873795/concepts/3093825960923
[7]: http://go.cloudera.com/udacity-lesson-2