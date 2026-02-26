---
layout: post
title: Some tricks to backup large data (like dump with SQL)
tags: [data, backup, sql] 
---

We're sometimes bound to backup data for personal usage (e.g., photos) or in a professional context (e.g., in research). 
Data can be very large (gigabytes or terabytes), and I have faced many experiences where (1) data are located in a remote machine (2) the copy is not straightforward. 
I give three examples below, with some tricks (or not). 
If you want specifically a trick with mysqldump, it's at the end of the post. 


# Chess games

We performed large-scale analysis of chess games and gathered around 1 tera of chess engines' evaluations. If you're interested, [the article is available](https://hal.inria.fr/hal-01307091) while a snapshot of data is also available [here](http://chess.variability.io/). 
The solution to backup was quite brute force: buy a hard-disk with many teraoctets, dump the database into it, wait one week (true story). 
Fortunately the brute force copy was possible, that is, copying 1 tera was feasible. 

A not-so-funny experience followed: you want to check that your dump is correct, isn't it? 
What about opening a terabyte file with vim, emacs, more or less? Good luck ;) 

# Matrices

Another story occured when mining millions of tabular data out of Wikipedia dumps (a bit in the spirit of a [previous blog post](https://blog.mathieuacher.com/WikipediaMatrixChallenge/)). 
The size of files was OK (a few octets), but the number of files was not: millions of files! 
A mistake to never make is to copy into an USB hard disk files by files. Instead, produce an archive (a tar, a bzip, whatever) since only one file is then copied (it avoids infinite I/O calls), and the information is also compressed. The real story is that I recovered an existing hard disk containing a huge directory with millions of files, and then you're screwed: either you produce the archive, but I/O calls will happen within the hard disk, or you copy files by files (it can be looooog) and perform the compression on your local hard disk. 
I would have appreciated to have a single file, an archive ;)

# Configuration data 

This time, we had at our disposal a very large database of software configuration measurements (including executions' logs). 
The database was a few gigabytes and the hard disk was almost full. Urgent backup needed. 
The problem is that you don't have enough space to make a full dump. Without a dump, no free space. So what to do? 

A simple trick is to produce a dump of the last entries. 
I've found a way to do it with mysqldump:
```
mysqldump -u blabla -p --opt --where="1 limit 1000" Database Table | bzip2 > dump-configurations.sql.bz2
```

It performs a dump of the table `Table` in the database `Database`, only considering the first 1000 entries. 
It also produces a compressed file. 
This solution is useful per se, but not sufficient for my problem: I want the full dump. 
The idea is to split up the dump in different parts, save the intermediate dumps outside the machine, and iterates until covering all data.

The point is to understand what's behing `--where`. As nicely explained in this [StackOverflow comment](https://stackoverflow.com/questions/135835/limiting-the-number-of-records-from-mysqldump#comment7932450_135900)

```
The --where option is basically appended to a query of the form SELECT * from table WHERE, 
so in this case you get SELECT * from table WHERE 1 limit 1000000. 
Without the 1, you would have an invalid query. Specifying 1 for a where clause (since 1 is always true) simply selects all records.
```

We can edit the `--where` to insert a condition, order by with descending order strategy over a specific primary key. 

```
mysqldump -u blabla -p --opt --where="cid <= 15000 ORDER BY cid DESC limit 2000" Database Table | bzip2 > dump-configurations.sql.bz2
```
With this solution, I have been able to produce intermediate dumps of reasonable size in such a way I can move them to another disk and machine. 

It's not a big trick but I wanted to share it, just in case. And also illustrate the technicity of dealing with very large data (I'm sure you have similar stories to share). 



 






*(new!) I realized that some of my blog post entries are sometimes cited in academic works, so why not using the following bibtex entry?*

```bibtex
@misc{acher2019dumpingtrickssql,
  author = {Mathieu Acher},
  title = {Some tricks to backup large data (like dump with SQL)},
  year = {2019},
  month = {apr},
  howpublished = {\url{https://blog.mathieuacher.com/DumpingTricksSQL/}},
  note = {\url{https://blog.mathieuacher.com/DumpingTricksSQL/}}
}
```
