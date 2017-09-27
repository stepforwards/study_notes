---
title: Lucene
tags: Lucene,linux,Java
grammar_cjkRuby: true
---

# 数据的分类
> 我们的生活中的数据总体分为两种：结构化数据和非结构化数据
> 结构化数据：之具体固定格式或长度的数据，如数据库，元数据等等
> 非结构化数据：指不定长或务固定格式的数据，如邮件，word文档等磁盘上的文件

## 结构化查询

> 因为数据库等这种结构化数据存储是有规律的，有行有列而且数据格式，数据长度都是固定的，所以使用sql查询，速度比较快

## 非结构化数据查询

> 非结构化数据查询方式有两张
> 1. 顺序扫描(Serial Scanning) 比如要查找内容包括某个字符串的文件，就是一个文档一个文档的坎，对于每一个文档，从头看到尾，如果此文档包含此字符串，则此文档为我们要找的文件，接下来查看下一个文件，知道扫描完成所有的文件，如利用windows的搜索也可以搜索全文内容，只是相当的缓慢。
> 2. 全文检索(Full-text Search)将非结构化数据中的一部分信息提取出来，重新组织，使其变得有一定结构，然后对此有一定结构的数据进行搜索，从而达到搜索较快的目的。这部分从非结构化数据中提取出来的然后重新组织的信息，我们称之为索引。

## 全文搜索

> 类似于字典的过程,字典的拼音表和部首检字表就相当于字典的索引,我
们通过索引查找到我们需要的内容 
这种先建立索引，再对索引进行搜索的过程就叫全文检索(Full-textSearch)。

# Lucene

> Lucene是apache下的一个开放源代码的全文检索引擎工具包。提供了完整的查询引擎和索引引擎，部分文本分析引擎。我们可以使用Lucene实现全文检索的功能

- [下载地址][1]
- 创建索引查询索引的流程

![enter description here][2]

## 创建索引

> 对文档索引的过程，将用户要搜索的文档内容进行索引，索引存储在索引库(index),创建关键索引的过程包括,几个部分, 
- 1.获取原始文档
- 2.创建文档对象进行分析
- 3.创建索引放入索引库

## 获取原始文档

> 原始文档是指要索引和搜索的内容。原始内容包括互联网上的网页、数据库中的数据、磁盘上的文件等。Lucene不提供信息采集的类库，需要自己编写一个爬虫程序实现信息采集，也可以通过一些开源软件实现信息采集,常用的java的信息采集类库有Nutch,jsoup,heritrix,我们目前研究的对象,是磁盘中的文件

## 创建文档对象
> 获取原始内容的目的是为了索引，在索引前需要将原始内容创建成文档Document对象,文档中包括一个一个的域Field域中存储内容.


![enter description here][3]

- 我们可以为磁盘中的每一个文件当成一个document对象,中包括一些Field如我们可以指定 
	- file_name文件名称
	- file_name文件名称
	- file_size文件大小
	- file_content文件内容
- 每个document可以有多个Field
- 不同的document可以有不同的field
- 同一个document可以定义相同的Field
- 每一个文档放入索引库的时候都会生成一个唯一的编号,叫做id,id和数据库不同的是id不是域

## 分析文档

> 将原始内容创建为包含域（Field）的文档（document），需要再对域中的内容进行分析，分析的过程是经过对原始文档提取单词、将字母转为小写、去除标点符号、去除停用词等过程生成最终的语汇单元，可以将语汇单元理解为一个一个的单词。

- 例如我们通过这段话 The Spring Framework provides acomprehensive programming and configuration model formodern Java-based enterprise applications经过分析就成为一个语汇单元 spring,framework,comprehensive,programming.......
- 每一个单词叫做一个Term,不同的域(如文件名,文件路径,文件内容)都含有同相同的单词(如spring)会被认为是不同的Term
- Term中包含两个部分 
	- 一部分是文档的域名
	- 一部分是单词的内容
- 每一个Term对象会被创建为索引保存到我们的索引库中
- 当我们进行搜索的时候,本质就是创建一个Term,包含域名和单词内容

![enter description here][4]

## 创建索引
![enter description here][5]

- 索引库由两个部分组成索引和文档对象
- 索引就是我们之前进行文件分析的时候的每个Term对象
- 文档对象就是我们之前创建的document对象
- 如果此时有多个文档对象有相同的Term对象,那么在索引库中只会有一个Term,在此Term中会有一个文档对应的编号,用来指定当前Term对应哪个文档,当进行查询的时候会将对应的多个文档一并返回
- 如果一个Term对应多个文档,那么这多个文档的先后顺序按照文档中的出现次数作为排序,次数越多排序越靠前

### 倒排索引

> 正常查看文档的方式应该先找到文档,再去查找文档中的内容,而我们上述的过程是先通过查找文档中的内容生成索引,再通过索引去找到对应的文档,这种方式叫做倒排索引.倒排索引结构也叫反向索引结构，包括索引和文档两部分，索引即词汇表，它的规模较小，而文档集合较大,所以通过倒排索引的方式速度较快

# 创建索引API

- 导包
	- lucene-core-4.10.3.jar
	- lucene-analyzers-common-4.10.3.jar
	- lucene-queryparser-4.10.3.jar
	- commons-io-2.2.jar
- 创建indexwriter对象
- 指定索引库的存放位置Directory对象
- 指定一个分析器，对文档内容进行分析
- 创建Document对象
- 创建Filed对象
- 创建Field对象，将对象添加到document对象中
- 使用indexwriter对象将document对象写入索引库，此过程进行索引创建。并将索引和document对象写入索引库`iw.addDocument(document);`
- 关闭IndexWriter对象 iw.close();




  [1]: http://lucene.apache.org
  [2]:https://www.github.com/xiesen310/notes_Images/raw/master/images/1506512560549.jpg
  [3]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506513156990.jpg
  [4]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506513464576.jpg
  [5]: https://www.github.com/xiesen310/notes_Images/raw/master/images/1506513492461.jpg