# NodeDome

Used for demo.

## 环境准备
-------------
	
 1. nodejs

  - 下载并安装[nodejs](https://nodejs.org/en/)
 
	安装nodejs时会一并安装npm，使用以下命令确定是否安装成功

	```
	>node -v
	>npm -v
	```

## 新建项目（基础篇）
-------------
 1.  使用express生成项目
	```
	>npm i -g express-generator 
	>express -e ejs demo
	>cd demo
	```

  
 2.  修改package.json, 添加mongo和monk模块，并修改 "start":"node ./bin/www"
 

	```
	{
  "name": "NodeDemo",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "nodemon ./bin/www"
  },
  "dependencies": {
    "body-parser": "~1.13.2",
    "cookie-parser": "~1.3.5",
    "debug": "~2.2.0",
    "ejs": "~2.3.3",
    "express": "~4.13.1",
    "morgan": "~1.6.1",
    "serve-favicon": "~2.3.0",
    "mongodb": "*",
    "monk": "*",
    "nodemon": "*"
  }
}

	```
**安装依赖包：**
	>npm install


 4. 运行项目
 
	```
	>cd <project-folder>
	>npm start
	```
 5. 打开浏览，启动：

    >http://localhost:3000

## 新建项目（进阶篇）
-------------
1.  从界面开始，先修改views/index.ejs
	```
	//删除<p>Welcome to <%= title %></p>	
	//添加以下代码，表单提交，实现增加一条记录的功能
		<form action='/add' method="post">
			<label for="title">title:</label>
			<input type="text" name="title">
			<label for="url">url:</label>
			<input type="text" name="url">
			<input type="submit">
		</form>
	```

 
2.  添加路由，连接数据库，修改routes/index.js，添加数据库连接命令， 和/add路由， 然后浏览器端打开页面进行测试。
 

	```
	
	var monk = require('monk');
   var db = monk('localhost:27017/demo');
   
	router.post('/add', function(req, res){
	var newItem = req.body;
	var collection = db.get("bookmarks");
	collection.insert(newItem, function(err){
		if(err){
			res.send(err);
		}else{
			res.redirect('/');
		}
	});	
});

	```

4. 目前并没有什么变化，接下来添加显示列表，修改views/index.ejs，添加以下代码
 
	```
		<ul>
			<% items.forEach(function(item){%>
			<li>
				<a href="<%= item.url %>" rel="<%= item._id %>"> 
				<%= item.title %></a>
			</li>
			<%})%>
		</ul>
	```
5. 修改routes/index.js, 打开浏览器测试下

  	```
router.get('/', function(req, res, next) {
	var collection = db.get("bookmarks");
	collection.find({},{}, function(err, result){
		if(err){
			res.send(err);
		}else{
			res.render('index', 
				{title: 'Bookmarks List', items: result});
		}
	});
	```

## 进阶（rest篇&&Debug）
-------------
1.  直接演示， 使用postman测试接口

	```
	post:
	http://localhost:3000/rest/add 
	delete:
	http://localhost:3000/rest/delete/:id 
	get: 
	http://localhost:3000/rest/list
	
	```

 

4. Debug, 安装 node-inspector
 
	```
	>npm install -g node-inspector
	>node-inspector
	```
5. 修改package.json

  	```
"start": "nodemon --debug ./bin/www"
	```

4. 启动项目，debug
	```
	http://127.0.0.1:8080/?ws=127.0.0.1:8080&port=5858 
	```

## 参考资料
-------------

1. [nodejs](https://nodejs.org/en/)
2. [npm](https://www.npmjs.com/)
3. [Express](http://www.expressjs.com.cn/4x/api.html#res)