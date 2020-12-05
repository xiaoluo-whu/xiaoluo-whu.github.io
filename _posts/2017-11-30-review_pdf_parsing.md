---
title: 'Pdf parsing and searching system'
date: 2017-11-30
permalink: /posts/2017/11/pdf_parsing_searching/
tags:
  - pdf parsing
  - Solr
  - search engine
  - Linux
  - cluster
---

This is a Pdf file parsing and searching system that I led seven members to develop.

<!--more-->

> ![](https://xiaoluo-whu.github.io/files/images/pdf_parsing.jpg)

In short, we want to build a web portal which makes it easy for users to search pdf files.
However, in order to make the search service user-friendly. We can not only search by pdf files' title. We need to go deeper, 
enabling searching by pdf files' content.

A pdf file usually contains not only plain text, but also table, graph, title, author name and other features.
To build such a system, we adopt third party frameworks, PDFPARSER and PDFBox, to parse pdf files, extracting a pdf's text, table and 
graph, title, author names and other features. Then, we save these fields into [MongoDB](https://www.mongodb.com/). 

We build an [Solr](https://lucene.apache.org/solr/) search engine and push those extracted pdf fields into the engine.
Then, we build a browser-server mode portal for users to search pdf files by various fields.

In total, we set up a distributed system consisting of five servers.

* server1: task generator. Monitoring incremental pdf files in MongoDB, assigning parsing tasks to "workers".
* server2: worker. Parsing pdf files and save extracted fields into MongoDB.
* server3: worker. Same as server2.
* server4: Solr search engine. 
* server5: backend of the portal website.