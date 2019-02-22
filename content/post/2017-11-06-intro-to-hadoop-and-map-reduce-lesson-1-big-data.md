---
title: "[Intro to Hadoop and MapReduce] Lesson 1 Big data"
metaAlignment: center
date: 2017-11-06 09:00:00+09:00
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

# 1. Introduction

You can read more about [Big Data][1] in Wikipedia which is also a company that generates and processes huge amounts of data itself.

[MapReduce][2] and [Apache Hadoop][3] are the technologies we will be talking about more in this course.


# 2. Data Sources

According to IBM: "Every day, 2.5 billion gigabytes of high-velocity data are created in a variety of forms, such as social media posts, information gathered in sensors and medical devices, videos and transaction records"


# 3. Quiz: Big Data

What is BIG DATA?

- [ ] Order deatils for a store
- [x] All orders across 100s of stores
- [ ] A Persons's stock portpolio
- [x] All stock transactions for the NYSE


# 4. Definition of Big Data

A resonable definition of big data might be, It's data that's too big to be processed on a single machine.

Big Data is a loosely defined term used to describe data sets so large and complex that they become awkward to work with using standard statistical software. [(International Journal of Internet Science, 2012, 7 (1), 1–5)][4]


# 5. Quiz: Challenges

Challenges with big data

- [ ] Most data is worthless
- [ ] Data is created fast
- [ ] Data from different sources in various formats


# 6. The 3 Vs - Volume

The 3 V's were first defined in a research report by Douglas Laney in 2001 titled ["3D Data Management: Controlling Data Volume, Velocity and Variety".][5]

In 2012 [he updated][6] the definition as follows "Big data is high volume, high velocity, and/or high variety information assets that require new forms of processing to enable enhanced decision making, insight discovery and process optimization".


# 7. Quiz: Worthwhile Data

Data Worth Storing?

- [x] Transactions
- [x] Logs
- [x] Business
- [x] User
- [x] Sensor
- [x] Medical
- [x] Social


# 8. Variety

The problem is that to store data in systems like that(traditional database), the data needs to be able to fit in pre-defined tables. And a lot of data that we deal with these days, tends to be what we call unstructured or semi-sturctured data.


# 9. Data Formats

Nice thins about Hadoop is that it doesn't care what format your data comes in. Unlike a traditional database, you can store the data in its raw format and manipulate it and reformat it later.


# 10. Quiz: Using Variety

Variety

- [x] Current GPS
- [x] Current Plan
- [x] Traffic Data
- [x] Current Load
- [x] Fuel Efficiency


# 11. Velocity

TB/day


# 12. Quiz: Your Interests

What data intrests you?
> Survey question. no right.

- [ ] Science
- [ ] E-commerce
- [ ] Financial
- [ ] Medical
- [ ] Sports
- [ ] Social
- [ ] Utilities


# 13. Doug Intro




# 14. Doug Cutting: The Origins of Hadoop

**Doug Cutting**, Creator of Hadoop

Here are the papers Google published about their [distributed file system (GFS)][7] and their processing framework, [MapReduce][8].

# 15. Hadoop Logo Intro


# 16. Doug Cutting: The Name of Hadoop

Came from his son's toy.


# 17. Core Hadoop

https://youtu.be/alie2Kn-jRY

Cloudera provides free download of [Chapter 2 of Tom White’s essential text, Hadoop: The Definitive Guide.][9]


# 18. Hadoop Ecosystem

See more inforation about [Pig][10], [Hive][11], [HBase][12], [Impala][13], [Mahout][14], Sqoop, Flume, Hue, Oozie.

- CDH


# 19. Congratulations

See more in the free [Chapter 2 of Tom White’s essential text, Hadoop: The Definitive Guide][15]


[1]: http://en.wikipedia.org/wiki/Big_data
[2]: http://en.wikipedia.org/wiki/Mapreduce
[3]: http://hadoop.apache.org/
[4]: http://www.ijis.net/ijis7_1/ijis7_1_editorial.pdf
[5]: http://blogs.gartner.com/doug-laney/files/2012/01/ad949-3D-Data-Management-Controlling-Data-Volume-Velocity-and-Variety.pdf
[6]: http://en.wikipedia.org/wiki/Big_data#cite_note-23
[7]: http://static.googleusercontent.com/media/research.google.com/en/us/archive/gfs-sosp2003.pdf
[8]: http://static.googleusercontent.com/media/research.google.com/en/us/archive/mapreduce-osdi04.pdf
[9]: http://go.cloudera.com/udacity-lesson-1
[10]: http://www.cloudera.com/content/cloudera/en/resources/library/training/introduction-to-apache-pig.html
[11]: http://www.cloudera.com/content/cloudera/en/resources/library/training/introduction-to-apache-hive.html
[12]: http://www.cloudera.com/content/cloudera/en/resources/library/training/intorduction-hbase-todd-lipcon.html
[13]: http://www.cloudera.com/content/cloudera/en/resources/library/training/an-introduction-to-impala.html
[14]: http://mahout.apache.org/
[15]: http://go.cloudera.com/udacity-lesson-1