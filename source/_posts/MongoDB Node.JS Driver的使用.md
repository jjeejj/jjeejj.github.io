---
title: MongoDB Node.JS Driver 的使用
date: 2016-10-24 17:23:58
tags: [node,mongodb,数据库]
description: MongoDB 官方为 Node.js 提供的mongodb数据库驱动。在mogodb-core核心上为最终使用的用户提供了一个高级API。
---

--------------------------

## 安装

通过npm命令行的方式开始使用***Node.js 2.2 driver***，并把该依赖安装到项目中。

### MongoDB Driver

在用 ***npm init*** 初始化你创建的项目后，你可以用以下命令安装 ***MongoDB Driver*** 和它的***依赖***

	npm install mongodb --save

>这句命令会下载 ***MongoDB Driver***，并且把该依赖的键值对添加到 ***package.json*** 文件中

## 快速开始

这个说明将向你展示在一个简单的应用中如何设置并使用 ***Node.js*** 和 ***MongoDB***。包括怎么设置驱动和执行简单的 ***CRUD*** 操作。

### 创建 ***package.json*** 文件 

首先，创建一个应用文件夹

	mkdir myproject
	cd myproject
	
输入以下命令，回答在初始化你的项目结构是出现的问题

	npm init

然后，安装驱动依赖
	
	npm install mongodb --save
	
>你将看到 ***NPM*** 下载一些文件，一旦下载完成，你可以看到所有的下载包在 ***node_modules*** 文件夹下

### 启动一个 MongoDB Server

如何启动自行Google

### 连接 MongoDB Server

创建一个 ***app.js*** 文件，添加以下代码，试图用 ***MongoDB driver*** 进行一个基础的 ***CRUD*** 操作。
	
	var MongoClient = require('mongodb').MongoClient
	  , assert = require('assert');

	// Connection URL
	var url = 'mongodb://localhost:27017/myproject';

	// Use connect method to connect to the server
	MongoClient.connect(url, function(err, db) {
	  assert.equal(null, err);
	  console.log("Connected successfully to server");

	  db.close();
	});

利用下面的命令运行你的app
	
	node app.js
	
>你将在控制台看到 ***Connected successfully to server*** 信息

### 插入文档

向 ***app.js*** 中添加以下方法。利用 ***insertMany*** 方法向 *** document *** 集合中添加3条记录

	var insertDocuments = function(db, callback) {
	  // Get the documents collection
	  var collection = db.collection('documents');
	  // Insert some documents
	  collection.insertMany([
		{a : 1}, {a : 2}, {a : 3}
	  ], function(err, result) {
		assert.equal(err, null);
		assert.equal(3, result.result.n);
		assert.equal(3, result.ops.length);
		console.log("Inserted 3 documents into the collection");
		callback(result);
	  });
	}

**insert**  命令返回一个对象，包含以下的字段
* **result**  包含来自 ****MongoDB*** 插入结果文档
* **ops**  包含插入的文档，和增加的 ***_id*** 字段
* **connection**  插入操作的连接

添加以下的代码调用 ***insertDocuments*** 方法向

	var MongoClient = require('mongodb').MongoClient
	  , assert = require('assert');

	// Connection URL
	var url = 'mongodb://localhost:27017/myproject';
	// Use connect method to connect to the server
	MongoClient.connect(url, function(err, db) {
	  assert.equal(null, err);
	  console.log("Connected successfully to server");

	  insertDocuments(db, function() {
		db.close();
	  });
	});
	
运行更新后的***app.js***文件

	node app.js

上述操作讲打印出以下信息

	Connected successfully to server
	Inserted 3 documents into the collection


## 更多文档说明，请查看[官方文档说明](http://mongodb.github.io/node-mongodb-native/2.2/quick-start/)


-------------------------------------------------------

	

