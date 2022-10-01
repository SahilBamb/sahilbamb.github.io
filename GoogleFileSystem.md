# Works Cited

### Original Paper: The Google File System Sanjay Ghemawat, Howard Gobioff, and Shun-Tak Leung

Link: https://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf

Original paper published at Google that theorized the concept and was implemented to great success at Google.  All the other resources will explain this paper in great depth. 

### Google File System - Paper that Inspired Hadoop Youtube Video by Defog Tech
Link: https://www.youtube.com/watch?v=eRgFNW4QFDc

A short summary of the paper and connections to later implementation in the well known Apache Hadoop distributed file system. 

Google File System is essentially a distributed file system. In any given cluster, there can be 100s or 1000s of commodity servers. But together this cluster allows a number of clients to read or write a file. 

It is a file system but distributed among hundreds or thousands of servers. 
Some design considerations were: 
1. Commodity Hardware
2. Handling Large Files
3. Two Operations: Read and Append
4. Chunk Servers
5. One Master for All Servers

### MIT 6.824 Distributed Systems (Spring 2020) Lecture 3 - GFS
Link: https://www.youtube.com/watch?v=EpIgvowZr00

An amazing lecture from an MIT course on distributed systems that describes the GFS or Google File System from a background approach. 

**Why is it Hard?**
We want high **performance** so we leverage 100s to 1000s of machines by using many servers in parallel (sharding). This number of systems will inevitably lead to **faults** due to the cheap nature of hardware and sheer number of servers. Dealing with these faults demand automatically **tolerant** systems. We can deal with automatically tolerant systems through **replication**. Replication produces **inconsistencies** between data where the same replicated file is saved differently in different locations. Fixing this consistency issue demands **low performance**. 

As you can see in order to achieve the desired performance, we end up resulting in low performance leading to a circular issue. 

Strong consistency means that it feels like the user is just accessing their files from one place, when in reality it is stored on numerous devices geographically distinct from one another. It also allows for cheap hardware to replace one large, expensive drive. 


### The Last Publicly Announced Change of GFS in 2010: Removing Map Reduce

Discusses updating GFS in 2010 to Colossus to deal with changes in the digital handheld era. Specifically, adapting to the mobile landscapes need to keep up with the real-time world. Map Reduce, the underlying algorithm underneath GFS and motivation for the paper is inherently batch oriented. 

Goal is to update the search index continuously, within seconds of content changing, without rebuilding the entire index from scratch using MapReduce.

When a page is crawled, changes are made incrementally to the web map store din Big Table using something like a database trigger

Triggers tend to:
* Destroy Performance
* Update anomalies in code
* Checking logic is duplicated
* Not scalable
* Application logic moves into triggers (no divide)

Google may have made a specialized database that solves these issues with triggers. Which makes sense as hundreds of thousands of pages are being crawled simultaneously.

Interesting Quote about Google's motivations: "_We're in business of making searches useful, we're not in the business of selling infrastructure_."
### Data Intensive Storage Services for Cloud Environments by Kyriazis, Dimosthenis

Link to E-Book (relevant pages only): https://books.google.com/books?id=mNCeBQAAQBAJ&pg=PA13#v=onepage&q&f=false

This book offers a great, brief overview of different distributed systems from Google File System to Hadoop File System. 

Google File System: GFS
GFS is a file system designed by Google to support its unique applications. Not for general purpose of file system but for Google's search enging data collecting mechanisms. Thus the most used operations are appending with files rarely being modified. Also due to this business need, there is a preference for high throughput rather than low latency. 

Hadoop File System: HDFS
HDFS originally attempted to mimic GFS as it stores large files across multiple machines and is designed to handle those large files. It has since branched considerably and tends to different business needs than GFS. It is also important to note that GFS is proprietary with most of its intricacy not publicly available while HDFS is an open source project. 

### Case Study - GFS: Evolution on Fast-Forward

Link: https://queue.acm.org/detail.cfm?id=1594206

This article is a short summary of the paper and relevant background, and then goes into an extremely insightful interview with Sean Quinlan, GFS tech leader for years and currently a principal engineer at Google. It explains many of the decisions made in the design of GFS outlined in the paper. 

Building Google Search did not originally involve building a new file system as the focus was on indexing and crawling the vast web. But the limitations of existing storage software in accomplishing these web crawling needs made it clear the need for a more efficient system: GFS. 

The demands were that the storage network be inexpensive (Google was a young startup company) as well as constant monitoring fault tolerance, error detection and automatic recovery. 

The biggest limitations were the throughput requirements. As Google needed to crawl the entire web, it meant multi-gigabyte files and data sets of terabytes of information and millions of objects. This meant traditional I/O had to be completely overhauled to account for the acual needs of the system rather than general flexibility. For example, rare were scraped websites "edited" with they almost always being appended. 

There was also the need for scale. While this was important back then, the engineers at Google would have no idea the real level of scale that would be demanded twenty years later. 

Some interesting notes from the interview:
* Master was made for feasibility as it needed to be used by Google as soon as possible. Ultimately some of these same engineers rolled back this decision in BigTable years later. 
* Designing GFS, the Google engineers had no idea the level of scale they would be dealing with. Going from terabytes to petabytes then to tens of petabytes meant linear increases in many checking and duplication operations. 
* MapReduce was made obsolete because of the single master, as with thousands of clients talking to master (with master only being able to do a few thousand operations a second) means less than one operation a second for each client. An application like MapReduce may have a thousand tasks each wanting to open a number of files. This causes a unsurmountable issue. 
* One strategy to deal with scale to do with the use of multiple "cells" across the network, functioning essentially as related but distinct file systems. With each cell, ultimately, having its own master. 


### # Google File System Eval: Part I
A great simplified down version of GFS for non-technical readers. 

Link: https://storagemojo.com/google-file-system-eval-part-i/
