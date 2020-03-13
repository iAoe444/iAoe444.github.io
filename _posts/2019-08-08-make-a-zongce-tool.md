---
title: 综测分计算工具
categories:
- project
tags:
- python
- ocr
---

> 由于往年计算综测分是件繁琐而又复杂的事情，所以写了个综测分计算工具来帮助解决这种问题，后来逐渐诞生出三个版本，一个是本地版本`local_version`，一个是web版本`web_version`，还有一个是数据库版本`web_db_version`，使用的技术主要为`flask`快速搭建后台+`baiduAI`文字识别，以及`layui`做响应式布局
>
> 书写于2019/08/08 18:17

## OCR识别成绩和科目

> 这里识别的是奕报告的成绩截图，将里面的成绩和科目提取出来，这样就可以快速计算基础分了
>
> [百度智能云-文字识别](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)
>
> [百度智能云-Python文字识别SDK文档](https://ai.baidu.com/docs#/OCR-Python-SDK/top)
>
> [zongce/local_version](https://github.com/iAoe444/zongce/tree/master/local_version)

1. 这里识别的是奕报告的成绩截图，利用文字基础识别的`分块识别`的特点，将里面的成绩和科目提取出来，这样就可以快速计算基础分了

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sgq0445jj30ef07y0t3.jpg)

2. 这里由于做的是一个小工具，使用的是`python`语言，所以用的是里面提供的python的sdk，比较方便一点，当然你也可以通过提供里面提供的API来进行识别，不过就是没有sdk方便就是了[百度智能云-Python文字识别SDK文档](https://ai.baidu.com/docs#/OCR-Python-SDK/top)

3. 所以首先安装`OCR Python SDK`

   ```bash
   pip install baidu-aip
   ```

4. 快速创建一个通用识别文字的应用，这里还有很多参数和方法可以使用，具体看文档

   ```python 
   from aip import AipOcr
   
   """ 你的 APPID AK SK """
   APP_ID = '你的 App ID'
   API_KEY = '你的 Api Key'
   SECRET_KEY = '你的 Secret Key'
   
   """ 读取图片 """
   def get_file_content(filePath):
       with open(filePath, 'rb') as fp:
           return fp.read()
   
   image = get_file_content('example.jpg')
   client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
   """ 带参数调用通用文字识别 """
   client.basicGeneral(image)
   ```

   注意这里的`APP_ID`和`APP_KEY`以及`SECRET_KEY`需要自己去百度智能云的官网创建一个应用后获取[百度智能云-文字识别](https://console.bce.baidu.com/ai/#/ai/ocr/overview/index)

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sge8tpxoj31280jy3zy.jpg)

5. 当你使用上面的代码来识别奕报告的截图的时候，会发现识别的结果具体如下

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sguub94hj30qf05mwg1.jpg)

   发现是个字典类型，并且每个文字块都被分开了，成绩就在`综合成绩:96`这块里面，而科目信息就在成绩的前面，所以需要做的事情也就是使用`正则表达式`查找包含综合成绩的信息，并将成绩提取出来，而后取前一个信息作为科目

   ```python
   """ 获取成绩信息 """
   def get_grade_info(wordsResult):
       grades = {}
       for i in range(len(wordsResult)):
           # 获取wordsResult里面的"综合成绩:100"的结果
           words = wordsResult[i]['words']
           # 正则表达式匹配成绩
           matchObj = re.match(r'综合成绩:(.*)',words)
           # 如果words里面存在"综合成绩:100"的情况
           if(matchObj):
               grade = matchObj.group(1)
               # 科目是成绩前一个的信息
               subject = wordsResult[i-1]['words']
               grades[subject] = grade
       return grades
   
   # 通用文字识别
   wordsResult = client.basicGeneral(image)['words_result']
   # 获取成绩字典
   grades = get_grade_info(wordsResult)
   ```

6. 但是实际上在提取文字信息的时候会发现存在一些问题，比如出现成绩和科目混在一块的问题，具体还需要做些算法进行补偿

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sh3a8akmj30vp05g40m.jpg)

7. 通过优化这样就可以写成一个本地的版本[zongce/local_version](https://github.com/iAoe444/zongce/tree/master/local_version)，这里将图片放在image目录下，然后调用`ocr.py`就可以实现批量识别了

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sh9yxzh5j30ds022t8j.jpg)

## Web端的开发

### Flask快速搭建后台

> 这里使用Python的后台框架flask来搭建后台，方便快捷
>
> [web_version iAoe444/zongce](https://github.com/iAoe444/zongce/tree/master/web_version)

1. 首先安装flask

   ```bash
   pip install flask
   ```

2. 这里先贴出flask的后台代码，是不是难以相信这么少的代码就可以做一个后台

   ```python 
   # -*- coding:UTF-8 -*-
   from flask import render_template,Flask,request
   from getGrades import get_grades
   import os
   import json
   import random
   
   app = Flask(__name__)
   
   # 根路由
   @app.route('/', methods=['GET', 'POST'])
   def index():
       # 根路由返回static下的index.html目录
       return app.send_static_file('index.html')
   
   # 通过getgrades路由实现图片的上传和结果的返回
   @app.route('/getgrades', methods=['POST'])
   def getGrades():
       if 'file' in request.files:
           # 获取图片
           file = request.files['file']
           # 设置图片名
           fileName = str(random.randint(1000,9999))+file.filename
           # 保存图片
           file.save(fileName)
           # 识别成绩和科目
           grades = get_grades(fileName)
           # 删除图片
           os.remove(fileName)
           # 返回json数据
           return json.dumps(grades,ensure_ascii=False)
   
   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5000)
   ```

   其中已经将上一步的成绩识别写成了一个包`getGrades`，方便调用，这里仅返回科目和成绩，返回前端的窗口显示，这里有两个路由，根路由负责前端页面，发送我们事先写好的`index.html`页面，另一个是负责图片的上传和返回成绩的识别结果

3. 之后就是前端页面`index.html`的书写，由于有个大神做了一个简单的js版本的本地html页面，这里直接修改代码实现图片的上传和成绩结果的渲染，这里使用jQuery来实现

   ```html
   <!--成绩的输入和渲染的地方-->
   <div>
       无线传感器网络技术
   </div>
   <input type="number" id="无线传感器网络技术">
   
   <!--文件上传的地方-->
   <input type="file" id="image"/>
   <input type="submit" id="upload"/>
   
   <script type="text/javascript">
       // 成绩渲染函数，遍历json数据
       function setGrade(json) {
           for (var key in json) {
               $('#' + key).val(json[key]);
           }
       }
       // 点击上传按钮
       $('#upload').click(function (){
           // 获取file里面的图片文件
           var file = document.getElementById("image").files[0]; // js获取文件对象
           var data = new FormData();
           data.append("file", file);
           //调用接口
           $.ajax({
               url: "/getgrades",
               data: data,
               type: "Post",
               dataType: "json",
               cache: false,//上传文件无需缓存
               processData: false,//用于对data参数进行序列化处理 这里必须false
               contentType: false, //必须
               success: function (result) {
                   //渲染成绩
                   setGrade(result);
               },
           })
       });
   </script>
   ```

4. 当然基础分的计算和具体优化的操作可以通过js来实现，这里就不贴出来了，由于在写代码的过程中没使用git，所以初代的版本没有图片贴出，接下来是直接进行美化

### layui美化项目

> layui是一个前端的响应式框架，界面挺好看的所以使用了
>
> [layui - 经典模块化前端 UI 框架](https://www.layui.com/)

1. 这里分别使用了它的`表单`和`导航`，`弹出层`以及`文件上传`，如果你使用的仅仅是它的样式的话，那么直接使用导入它包里面的`layui.css`就可以了

   ```html
   <link rel="stylesheet" href="layui.css" media="all">
   ```

   其他的从它的实例代码里面搬运就行了，这里只用到样式的是[表单 - 在线演示 - layui](https://www.layui.com/demo/form.html)和[导航菜单/面包屑 - 在线演示 - layui](https://www.layui.com/demo/nav.html)

2. 但是当我再用到像[layer弹出层 - 在线演示 - layui](https://www.layui.com/demo/layer.html)和[文件/图片上传 - 在线演示 - layui](https://www.layui.com/demo/upload.html)需要使用到js的样式时，就要将整个layui整个包下载下来放在项目里，也就600多kb而已

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sig1o0j9j307y05kweg.jpg)

   上面的图片是layui包的路径，在调用的时候要调用css和js

   ```html
   <link rel="stylesheet" href="static/layui/css/layui.css" media="all">
   <script src="static/layui/layui.js" charset="utf-8"></script>
   ```

3. 接下来就是实现弹出层和文件上传的功能了，需要改写之前的代码

   ```javascript
   layui.use('layer', function () {
       var $ = layui.jquery, layer = layui.layer;
       // 刚打开页面弹出信息
       layer.open({
           offset: 'auto',
           title: '注意',
           content: "如果使用奕报告截图识别，请注意核对成绩"
       })
   });
   // 点击上传图片的时候自动上传文件
   layui.use('upload', function () {
       var upload = layui.upload;
   
       //执行实例
       var uploadInst = upload.render({
           elem: '#upload' //绑定元素
           , url: '/getgrades' //上传接口
           , done: function (res) {
               if (JSON.stringify(res) == "{}") {
                   layer.msg('请上传正确的奕报告截图');
               } else {
                   setGrade(res);
                   layer.msg('识别成功,注意核对成绩');
               }
           }
           , error: function () {
               layer.msg('上传出现错误');
           }
       });
   });
   ```

4. 到这里就可以展示一下改造后的项目了[web_version iAoe444/zongce](https://github.com/iAoe444/zongce/tree/master/web_version)

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5silem6vwj31200le3zk.jpg)

   ![](https://assest.iaoe.xyz/img/006bBmqIgy1g5sim0cj00j30cd0kkmxn.jpg)

   

![](https://assest.iaoe.xyz/img/006bBmqIgy1g5simlgw65j30cl0nimxw.jpg)

### 项目的部署

> ❗待完成

## 动态页面的开发

> 由于这里的绩点和科目都是写死的，不方便后期的修改，那么写一个连接数据库的动态页面就很方便了，这里使用mysql做数据库，使用pymyql进行数据库操作
>
> [web_db_version iAoe444/zongce](https://github.com/iAoe444/zongce/tree/master/web_db_version)

1. 数据库建表

   ```sql
   DROP TABLE IF EXISTS `tb_credit`;
   CREATE TABLE `tb_credit` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `subject` varchar(255) DEFAULT NULL COMMENT '科目',
     `credit` float(3,1) DEFAULT '4.0' COMMENT '学分',
     `term` int(2) DEFAULT '1' COMMENT '学期（1代表第一学期，2代表第二学期）',
     `enable_status` int(2) DEFAULT '1' COMMENT '1代表显示，0代表不显示',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=14 DEFAULT CHARSET=utf8;
   ```

2. 安装`pymysql`包

   ```bash
   pip install pymysql
   ```

3. 快速入门`pymysql`

   ```python
   import pymysql
   
   db = pymysql.connect("localhost","root","123456","zongce")
   cursor = db.cursor()
   
   """获取科目和对应的学分"""
   def getCredits():
       # 执行的sql语句
       sql = "SELECT * FROM tb_credit where enable_status=1"
       # 使用 execute()  方法执行 SQL 查询 
       cursor.execute(sql)
       # 使用 fetchall() 方法获取所有数据.
       results = cursor.fetchall()
       # 获取科目，学分和学期
       for row in results:
           subject = row[1]
           credit = row[2]
           term = row[3]
   
   db.close()
   ```

4. 制作一个类实现类似spring里的service层功能

   ```python
   import pymysql
   
   class DAO:
       # 初始化
       def __init__(self):
           # 创建连接
           self.conn = pymysql.Connect(
               host='localhost',
               port=3306,
               user='root',
               password='123456',
               db='zongce',
               charset='utf8',
               cursorclass=pymysql.cursors.DictCursor  # 以字典的形式返回数据
           )
           # 获取游标
           self.cursor = self.conn.cursor()
   
       """获取科目和对应的学分"""
       def getCredits(self):
           credits = []
           sql = "SELECT subject,credit,term FROM tb_credit where enable_status=1"
           self.cursor.execute(sql)
           for row in self.cursor.fetchall():
               credits.append(row)
           return credits
   
       """补偿科目信息"""
       def completeSubject(self, need_completed_subject):
           sql = "SELECT subject FROM tb_credit where subject like '%" + \
               need_completed_subject + "%'"
           self.cursor.execute(sql)
           return self.cursor.fetchone()
   
       # 关闭连接
       def close(self):
           self.cursor.close()
           self.conn.close()
   ```

5. 之后调用这个就行了，接着是将数据渲染到html页面上

   ```python
   @app.route('/', methods=['GET', 'POST'])
   def index():
       # 实例化类
       dao = DAO()
       # 调用方法
       credits = dao.getCredits()
       # 传递credits到模板上
       return render_template('index.html',credits=credits)
   ```

6. 接着是模板html的部分代码，注意要放在`templates`文件夹下

   ```html
   <div class="layui-form">
       <fieldset class="layui-elem-field layui-field-title" style="margin-top: 20px;">
           <legend>第一学期成绩</legend>
       </fieldset>
       {% for item in credits %}
       {% if item['term']==1 %}
       <div class="layui-form-item">
           <label class="layui-form-label">{{ item['subject'] }}<div name="credit">绩点：{{ item['credit'] }}</div>
           </label>
           <div class="layui-input-block">
               <input type="number" id="{{ item['subject'] }}" name="grade" autocomplete="off" placeholder="请输入成绩"
                      class="layui-input">
           </div>
       </div>
       {% endif %}
       {% endfor %}
   </div>
   ```

7. 接下来就是其他优化了，至此三个版本就介绍完毕了，完成这样一个小项目也算是对之前的学习的内容的总结，也算是小有成就感成就感