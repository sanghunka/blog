---
title: "[Intro to Hadoop and MapReduce] Lesson 4 Problem set"
metaAlignment: center
date: 2017-11-06 12:00:00+09:00
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

Which of the following is true?

- [ ] HDFS uses a central SAN(storage area network) to hold its data
- [ ] HDFS stores a single copy of all data
- [x] HDFS replicates all data for reliability
- [x] To store 100TB of data in a Hadoop cluster you would need 300TB of raw disk space by default



# 2. Quiz: DataNode

Which of the following is true if one of the nodes running the DataNode daemon on the cluster fails?

- [ ] Data could be lost
- [x] Hadoop will automatically re-replicate any blocks which were stored on that node
- [ ] Hadoop will automatically e-mail the system administrator warning of the problem
- [ ] Hadoop will continue but from now on there will now only be two copies of some block


# 3. Quiz: NameNode

What precautions can you take to reduce the likelihood of problems related to NameNode failure?

- [x] Configure the NameNode to store its metadata in a second location using NFS
- [x] Configure a standby NameNode
- [ ] Run a NameNode daemon on every node in the cluster
- [x] Make sure the NameNode is running on high-end hardware


# 4. Quiz: MapReduce

If you run a MapReduce job and specify an output directory in HDFS which already exists, which of the following happens?

- [ ] The previous directory will be deleted
- [ ] The previous directory will be renamed with __old after its name
- [ ] The job will refuse to run
- [ ] The job will run and new files will be put in the existing directory


# 5. Quiz: Key

Think about the data set we used in Lesson 2. If we wanted to work out how many people had purchased goods using a particular credit card, what could we use as the key emitted by the Mappers?

- [ ] The store name
- [ ] The product description
- [x] The purchase method
- [ ] The purchase price


# 6. Quiz: Block Size

Why is Hadoop's block size set to 64MB by default, when most filesystem have block sizes of 16KB or less?

- [x] If Hadoop's block size was set to 16KB, there would be a huge number of blocks throughout the cluster, which causes the NameNode to manage an enormous amount of metatada
- [x] Since we need a Mapper for each block that we want to process, there would be a lot of Mappers, each processing a piece bit of data, which isn't efficient
- [ ] Because you can only store one block per node on the cluster
- [ ] Because a 16KB block is too small for multiple Mappers to process simultaneously