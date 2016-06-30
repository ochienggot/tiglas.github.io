---
layout: post
title: "Data analysis in Awk"
modified:
categories: articles
excerpt:
tags: []
image:
  feature:
date: 2016-06-18T20:37:17+02:00
---
I recently encountered a problem that required writing a script to parse and analyze log data and output aggregated statistics.
The parsing goals included opening a file containing syslog data, finding all lines containing a specific pattern and extracting values corresponding to specific entries from the lines. The result of the analysis was supposed to be the totals for each of the values.

My gut reaction was that this is the kind of problem in which Awk excels: finding patterns in text and analyzing file contents. While working out the solution, I came to reflect on the powerful features of Awk such as associative arrays: a data structure that I had previously used many times without giving much thought. Associative arrays in Awk are like normal arrays, except that they can be indexed with strings.

For illustration purposes, assume we have a syslog file with lines that look like this

```
application=http original=632 terminal=1607
```

We want to add all the "original" and "terminal" values corresponding to each application. Awk arrays allow us to neatly express this as

```
original[$1] += $2
terminal[$1] += $3
```

It does the proper initialization when ```$1``` is not present in the ```original``` and ```terminal``` arrays. Accessing array elements is achieved through a simple loop

```
for (val in array)
```

The complete solution is in [this Github repository](https://github.com/Tiglas/syslog-analysis). For comparison, I implemented the solution in Python as well.
