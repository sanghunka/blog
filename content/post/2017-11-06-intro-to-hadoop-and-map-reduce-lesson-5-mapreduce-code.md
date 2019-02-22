---
title: "[Intro to Hadoop and MapReduce] Lesson 5 MapReduce Code"
metaAlignment: center
date: 2017-11-06 13:00:00+09:00
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
# 2. Quiz: Input Data

How to find total sales/store?

        KEY             VALUE
- [ ] time          store name
- [ ] cost          store name
- [x] store name    cost
- [ ] store name    product type


# 3. Quiz: Defensive Mapper Code

```py
# Your task is to make sure that this mapper code does not fail on corrupt data lines,
# but instead just ignores them and continues working
import sys

def mapper():
    # read standard input line by line
    for line in sys.stdin:
        # strip off extra whitespace, split on tab and put the data in an array
        data = line.strip().split("\t")

        # This is the place you need to do some defensive programming
        # what if there are not exactly 6 fields in that line?
        # YOUR CODE HERE
        
        # this next line is called 'multiple assignment' in Python
        # this is not really necessary, we could access the data
        # with data[2] and data[5], but we do this for conveniency
        # and to make the code easier to read
        date, time, store, item, cost, payment = data
        
        # Now print out the data that will be passed to the reducer
        print "{0}\t{1}".format(store, cost)
        
test_text = """2013-10-09\t13:22\tMiami\tBoots\t99.95\tVisa
2013-10-09\t13:22\tNew York\tDVD\t9.50\tMasterCard
2013-10-09 13:22:59 I/O Error
^d8x28orz28zoijzu1z1zp1OHH3du3ixwcz114<f
1\t2\t3"""

# This function allows you to test the mapper with the provided test string
def main():
	import StringIO
	sys.stdin = StringIO.StringIO(test_text)
	mapper()
	sys.stdin = sys.__stdin__
```    

```py
# my answer
if len(data) != 6:
    continue

# solution
if len(data) == 6:
    date, time, store, item, cost, payment = data
    print "{0}\t{1}".format(store, cost)
```


# 4. Quiz: Between

What happens between mapper and reducer?

- [ ] Bubble sort
- [x] Shuffle and sort
- [ ] Find and sort
- [ ] Quicksort


# 5. Quiz: Reducing

Keeping track

- [ ] Previous cost
- [x] Current cost
- [x] Total sales per store
- [x] Previous store
- [x] Current store


# 6. Quiz: Reducer Code

code: https://gist.github.com/chenghan/7456549

```py
import sys

salesTotal = 0
oldKey = None

for line in sys.stdin:
    data = line.strip().split("\t")
    if len(data) != 2:
        # Something has gone wrong. Skip this line.
        continue

    thisKey, thisSale = data
    if oldKey and oldKey != thisKey:
      print oldKey, "\t", salesTotal
      oldKey = thisKey
      salesTotal = 0

     oldKey = thisKey
     salesTotal += float(thisSale)
```

Are we done?

- [ ] Yes
- [x] No

> We are not done because we have not processed the last key. 아래 코드를 추가해줘야 함.

```py
if oldKey != None:
   print oldKey, "\t", salesTotal
```


# 7. Putting It All Together
# 8. Conclusion

See more in the free [Chapter 3 of Tom White’s essential text, Hadoop: The Definitive Guide][1]


[1]: http://go.cloudera.com/udacity-lesson-3

