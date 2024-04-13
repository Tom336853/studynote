[python flask框架详解_尘世风的博客-CSDN博客](https://blog.csdn.net/shifengboy/article/details/114274271)

## 1.url_for函数跳转

url_for()函数对于动态构建特定函数的URL非常有用。该函数接受函数的名称作为第一个参数，以及一个或多个关键字参数，每个参数对应于URL的变量部分

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
"""
@author:chenshifeng
@file:flask_demo.py
@time:2021/03/01
"""
from flask import Flask, redirect, url_for

app = Flask(__name__)


@app.route('/admin')
def hello_admin():
    return 'Hello Admin'


@app.route('/guest/<guest>')
def hello_guest(guest):
    return 'Hello %s as Guest' % guest


@app.route('/user/<name>')
def hello_user(name):
    if name == 'admin':
        return redirect(url_for('hello_admin'))
    else:
        return redirect(url_for('hello_guest', guest=name))


if __name__ == '__main__':
    app.run(debug=True)
```

redirect函数用于重定向，实现机制很简单，就是向客户端（浏览器）发送一个重定向的HTTP报文，浏览器会去访问报文中指定的url。

## 2.get/post请求

login.html

```
<html>
   <body>
      
      <form action = "http://localhost:5000/login" method = "post">
         <p>Enter Name:</p>
         <p><input type = "text" name = "nm" /></p>
         <p><input type = "submit" value = "submit" /></p>
      </form>
      
   </body>
</html>
```

```
from flask import Flask, redirect, url_for, request
app = Flask(__name__)

@app.route('/success/<name>')
def success(name):
   return 'welcome %s' % name

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      user = request.form['nm']
      return redirect(url_for('success',name = user))
   else:
      user = request.args.get('nm')
      return redirect(url_for('success',name = user))

if __name__ == '__main__':
   app.run(debug = True)
```

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231017162700598.png" alt="image-20231017162700598" style="zoom:67%;" />

## 3.Flask模板

Flask 模板
在大型应用中,把业务逻辑和表现内容放在一起,会增加代码的复杂度和维护成本.

模板其实是一个包含响应文本的文件,其中用占位符(变量)表示动态部分,告诉模板引擎其具体的值需要从使用的数据中获取
使用真实值替换变量,再返回最终得到的字符串,这个过程称为’渲染’
Flask 是使用 Jinja2 这个模板引擎来渲染模板
使用模板的好处

视图函数只负责业务逻辑和数据处理(业务逻辑方面)
而模板则取到视图函数的数据结果进行展示(视图展示方面)
代码结构清晰,耦合度低
使用 render_template() 方法可以渲染模板，你只要提供模板名称和需要作为参数传递给模板的变量就行了。

 render_template() ：就是将flask参数传给html的方法

**Flask 将表单数据发送到模板**

```
from flask import Flask, render_template, request
app = Flask(__name__)
@app.route('/')
def student():
   return render_template('student.html')


@app.route('/result',methods = ['POST', 'GET'])
def result():
   if request.method == 'POST':
      result = request.form
      return render_template("result.html",result = result)


if __name__ == '__main__':
   app.run(debug = True)
```

student.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <form action="http://localhost:5000/result" method="POST">
     <p>Name <input type = "text" name = "Name" /></p>
     <p>Physics <input type = "text" name = "Physics" /></p>
     <p>Chemistry <input type = "text" name = "chemistry" /></p>
     <p>Maths <input type ="text" name = "Mathematics" /></p>
     <p><input type = "submit" value = "submit" /></p>
  </form>
</body>
</html>
```

 result.html 

```
<!doctype html>
  <table border = 1>
     {% for key, value in result.items() %}
    <tr>
       <th> {{ key }} </th>
       <td> {{ value }}</td>
    </tr>
 {% endfor %}
</table>
```





route层：对数据库数据处理，转发到html层

html层：对页面交互数据处理接受，并与route层交互

```
result,fields = GetSql(conn, sql)
返回的result是整个二维数组，fields是属性
result：[(201003009, '李博博', '男', 20, '张掖', '服务外包'),(20100301, '李博博', '男', 20, '张掖', '服务外包')]
```

bootstrap

![image-20231018172553720](C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231018172553720.png)

## 4.项目流程

拷贝：<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029122406154.png" alt="image-20231029122406154" style="zoom:67%;" />

dbSqlite3.py修改数据库名称

main.py模板

```
from flask import Flask, render_template, request, url_for, redirect,session

from dbSqlite3 import *


app = Flask(__name__)

#--------------流程开始
@app.route('/',methods=['GET'])
def index():
    tablename = "student_info"
    sql = "SELECT * FROM student_info"
    result, fields = GetSql2(sql)
    return render_template('show1.html', datas=result, fields=fields)
#--------------流程结束
if __name__ == '__main__':
    # app.run("0.0.0.0",debug=True)
    app.run(debug=True)
    # app.run()
```

main.py

```python
from flask import Flask, render_template, request, url_for, redirect,session

from dbSqlite3 import *


app = Flask(__name__)


@app.route('/',methods=['GET'])
def index():
    tablename = "student_info"
    sql = "SELECT * FROM student_info"
    result, fields = GetSql2(sql)
    return render_template('show1.html', datas=result, fields=fields)

@app.route('/add', methods=['GET','post'])
def add():
    if request.method == "GET":
        return render_template('add.html')

    else:
        data = dict(
            stu_id=request.form['stu_id'],
            stu_name=request.form['stu_name'],
            stu_sex=request.form['stu_sex'],
            stu_age=request.form['stu_age'],
            stu_origin=request.form['stu_origin'],
            stu_profession=request.form['stu_profession']
        )

        InsertData(data, "student_info")
        return redirect(url_for("index"))

@app.route('/del', methods=['GET'])
def delete():
    id=request.args["stuid"]
    DelDataById("stu_id", id, "student_info")
    return redirect(url_for("index"))

@app.route('/del2/<id>', methods=['GET'])
def delete2(id):
    DelDataById("stu_id", id, "student_info")
    return redirect(url_for("index"))

@app.route('/update', methods=['GET','post'])
def upadte():
    if request.method == "GET":
        id = request.args['id']
        result, _ = GetSql2("select * from student_info where stu_id='%s'" % id)
        print(result[0])
        print(type(result[0]))
        # for p in pro:
        #     print(p)+
        return render_template('update.html', data=result[0])
    else:
        data = dict(
            stu_id=request.form['stu_id'],
            stu_name=request.form['stu_name'],
            stu_sex=request.form['stu_sex'],
            stu_age=request.form['stu_age'],
            stu_origin=request.form['stu_origin'],
            stu_profession=request.form['stu_profession']
        )
        UpdateData(data, "student_info")

        return redirect(url_for("index"))

if __name__ == '__main__':
    # app.run("0.0.0.0",debug=True)
    app.run(debug=True)
    # app.run()
```

渲染模板的模板

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数据显示</title>
    <!-- Bootstrap -->
    <link href="/static/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
    <script src="/static/bootstrap/3.2.0/js/bootstrap.min.js"></script>
</head>
<body>
<!--内容-->
</body>
</html>
```

show1.html（基础显示页面）

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数据显示</title>
    <!-- Bootstrap -->
    <link href="/static/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
    <script src="/static/bootstrap/3.2.0/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
    <div class="row text-center"><a href="/add">add</a></div>
    <div class="row">
        <table border="1" class="table">
            <tr>
                {% for a in fields %}
                <td>{{a}}</td>
                {% endfor %}
                <td>修改</td>
                <td>删除</td>
            </tr>
            {%for item in datas%}
            <tr>
                {%for a in item%}
                <td>{{a}}</td>
                {% endfor %}
                <td><a href="update?id={{item[0]}}">修改</a></td>
<!--                <td><a href="del?stuid={{item[0]}}">删除</a></td>-->
                <td><a href="del2/{{item[0]}}">删除</a></td>
            </tr>
            {% endfor %}
        </table>
    </div>
</div>
</body>
</html>
```

添加功能：show1.html的add按钮给main.py发送get请求--------然后在add.html里填写input信息--------然后给main.py发送post请求

-----添加数据库信息-----回到index函数

注意：href跳转的是add网页

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029123918989.png" alt="image-20231029123918989" style="zoom:67%;" />

form表单提交的是main函数里的/add函数

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029124008807.png" alt="image-20231029124008807" style="zoom:67%;" />

修改功能：<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029184710655.png" alt="image-20231029184710655" style="zoom:67%;" />获取id后，先去get请求到main.py通过select查找该学号所有信息，到update.html提交表单，post请求到main.py

1.href=localhost/update/学号

```
#模板代码
 <td><a href="update?id={{item[0]}}">修改</a></td>
```

```
#主程序
@app.route('/update', methods=['GET','post'])
def upadte():
    if request.method == "GET":
        id = request.args['id']
        result, _ = GetSql2("select * from student_info where stu_id='%s'" % id)
        print(result[0])
        print(type(result[0]))
        # for p in pro:
        #     print(p)+
        return render_template('update.html', data=result[0])
    else:

        data = dict(
            stu_id=request.form['stu_id'],
            stu_name=request.form['stu_name'],
            stu_sex=request.form['stu_sex'],
            stu_age=request.form['stu_age'],
            stu_origin=request.form['stu_origin'],
            stu_profession=request.form['stu_profession']
        )
        UpdateData(data, "student_info")

        return redirect(url_for("ins"))
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改记录</title>
    <link href="/static/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
    <script src="/static/bootstrap/3.2.0/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container">
    <div class="col-sm-6 col-sm-offset-4">
        <h3>请输入要更新记录的相关信息</h3>
        <form id="form1" name="form1" method="post" action="/update">
           <label>学号：{{data[0]}}</label>
            <input type="hidden" name="stu_id" value='{{data[0]}}'/>
            <p>
                <label>姓名： </label>
                <input type="text" name="stu_name" value='{{data[1]}}'/>

            </p>
            <p>
                <label>性别：</label>
                {% if data[2]=="男" %}

                <input name="stu_sex" type="radio" value="男" checked="checked"/>
                男

                <input type="radio" name="stu_sex" value="女"/>
                女
                {%else%}

                <input name="stu_sex" type="radio" value="男"/>
                男

                <input type="radio" name="stu_sex" value="女" checked="checked"/>
                女
                {%endif%}
                <br/>
            </p>
            <p>
                <label>年龄： </label>
                <input type="text" name="stu_age" value='{{data[3]}}'/>

            </p>
            <p>
                <label>籍贯：</label>
                <input type="text" name="stu_origin" value='{{data[4]}}'/>

            </p>
            <p>
                <label>专业：</label>
                <input type="text" name="stu_profession" value='{{data[5]}}'/>

            </p>
            <p>

                <input type="submit" name="button" value="提交"/>

                <input type="reset" name="button2" value="重置"/>

                <br/>
            </p>
        </form>
    </div>
</div>
</body>
</html>
```

注意：学号后面不能修改，所以直接取值，然后隐藏文本框

**修改和添加都是show.html发送get请求到main.py，然后到对应的add.html或update.html，再通过表单形式填写信息，然后post到main.py**

删除功能：

两种方式

1.

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029180316006.png" alt="image-20231029180316006" style="zoom:67%;" />

href=localhost/del2/学号

```
#模板代码
<td><a href="del2/{{item[0]}}">删除</a></td>
```

```
#主程序
@app.route('/del2/<id>', methods=['GET'])
def delete2(id):
    DelDataById("stu_id", id, "student_info")
    return redirect(url_for("ins"))
```

2.<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029180524847.png" alt="image-20231029180524847" style="zoom:67%;" />

```
#模板代码
<td><a href="del?stuid={{item[0]}}">删除</a></td>
```

```
#主程序
@app.route('/del', methods=['GET'])
def delete():
    id=request.args["stuid"]
    DelDataById("stu_id", id, "student_info")
    return redirect(url_for("ins"))
```

**改进版**

数据库改进：

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029205730482.png" alt="image-20231029205730482" style="zoom:67%;" />

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231029205735145.png" alt="image-20231029205735145" style="zoom:57%;" /><img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231204095544155.png" alt="image-20231204095544155" style="zoom:50%;" />

添加了一个表，增加了专业表

**动态查询**

```
def index():
    tablename = "student_info"
    sql = "SELECT s.*,  p.stu_profession FROM student_info s INNER JOIN stu_profession p on s.stu_profession_id=p.stu_profession_id"

    strWhere = []
    if "name" in request.args:
        name = request.args["name"]
        if name != "":
            strWhere.append("stu_name like '%%%s%%'" % name)

    if "stuno" in request.args:
        stuno = request.args["stuno"]
        if stuno != "":
            strWhere.append("stu_id = '%s'" % stuno)

    if len(strWhere)>0:
        sql=sql + " where " + " and ".join(strWhere)
        print (sql)

    result, fields = GetSql2(sql)
    return render_template('show1.html', datas=result, fields=fields)
```

add功能

**专业改为选项**

add.html

```
<label>专业：</label>
                <select name="stu_profession">
                    {% for pro in datas -%}
                    <option value='{{pro[0]}}'>{{pro[1]}}</option>
                    {%endfor-%}
                </select>
```

```
#主程序
@app.route('/add', methods=['GET', 'post'])
def add():
    if request.method == "GET":
        datas, _ = GetSql2("select * from stu_profession")
        return render_template('add.html', datas=datas)

    else:
        data = dict(
            stu_id=request.form['stu_id'],
            stu_name=request.form['stu_name'],
            stu_sex=request.form['stu_sex'],
            stu_age=request.form['stu_age'],
            stu_origin=request.form['stu_origin'],
            stu_profession_id=request.form['stu_profession']
        )

        InsertData(data, "student_info")
        return redirect(url_for("index"))
```



Update功能

```
<select name="stu_profession">

                    {% for pro in datas -%}
                    <option value='{{pro[0]}}' {{'selected' if pro[0]==data[5] }}>{{pro[1]}}</option>
                    {%endfor-%}
                </select>
```

登录session功能

```
def CheckLogin():
    if 'userid' not in session:
        return False
    else:
        return True
```

```
@app.route('/login', methods=['POST', 'GET'])
def login():

    if request.method=="GET":
        return render_template('login.html')
    if 1 ==1 :

        result, _ = GetSql2("select * from users where username='%s' and pwd='%s'" % (request.form['username'],request.form['pwd']))
        print(result)
        if len(result) > 0:
        #将userid存入session中
            session["userid"]=result[0][0]
            return redirect(url_for('index'))
        else:
            return render_template('login.html')
```

在其他功能上添加以下代码

```
    if not CheckLogin():
        return redirect(url_for('login'))
```

在main文件开头时添加

```
app = Flask(__name__)
app.secret_key = 'abcdefgh!@#$%'
```



## 5.伪类选择器

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
<style>
a:link {color:#000000;}      /* 未访问链接*/
a:visited {color:#00FF00;}  /* 已访问链接 */
a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
a:active {color:#0000FF;}  /* 鼠标点击时 */
</style>
</head>
<body>
<p><b><a href="/css/" target="_blank">这是一个链接</a></b></p>
<p><b>注意：</b> a:hover 必须在 a:link 和 a:visited 之后，需要严格按顺序才能看到效果。</p>
<p><b>注意：</b> a:active 必须在 a:hover 之后。</p>
</body>
</html>
```

## 6.浮动元素

```
<!DOCTYPE html>
<html>
<head>
	<title>Float浮动示例</title>
    <meta charset="UTF-8">
<style type="text/css">
    .box{ background:skyblue;overflow:hidden }
    .kid{ width: 100px;height: 100px; float:left;}
    .kid1{ background: yellow; }
    .kid2{ background: orange; }
    .wrap{ width: 300px; height: 150px; background: blue; color: white; }
</style>
</head>
<body>
    <div class="box">
        <div class="kid kid1">子元素1</div>
	<div class="kid kid2">子元素2</div>
    </div>
    <div class="wrap">其他部分</div>
</body>
</html>
```

当父级元素内部的子元素全部都设置浮动float之后，子元素会脱离标准流，不占位，父级元素检测不到子元素的高度，父级元素高度为0由于父级元素没有高度，下面的元素会顶上去，造成页面的塌陷。因此，需要给父级加个overflow:hidden属性，这样父级的高度就随子级容器及子级内容的高度而自适应

## 7.sqlite语句

```
CREATE TABLE perpolinfo(
	id  INTEGER PRIMARY KEY autoincrement NOT NULL,
	name TEXT NOT NULL,
	age INT NOT NULL,
	sex TEXT NOT NULL,
	address TEXT NOT NULL,
	phone TEXT NOT NULL,
);
```

```
DROP TABLE tablename
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN);
SELECT * FROM tablename；
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```

## 8.JavaScript

```
<!DOCTYPE html>
<html>
<body>

<h1>Document 对象</h1>

<h2>createElement() 方法</h2>

<p>创建一个 p 元素并将其追加到 "myDiv"：</p>

<div id="myDIV" style="padding:16px;background-color:lightgray">
<h3>DIV 元素</h3>
</div>

<script>
// 创建元素：
const para = document.createElement("p");
para.innerHTML = "This is a paragraph.";

// 追加到另一个元素：
document.getElementById("myDIV").appendChild(para);
</script>

</body>
</html>
```

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231204110122149.png" alt="image-20231204110122149" style="zoom:50%;" />

```
<!DOCTYPE html>
<html>
<body>
<h1>Element 对象</h1>

<h2>appendChild() 方法</h2>

<ul id="myList">
  <li>Coffee</li>
  <li>Tea</li>
</ul>

<p>单击“追加”可将一个项目附加到列表的末尾：</p>

<button onclick="myFunction()">追加</button>

<script>
function myFunction() {

// 创建 "li" 节点：
const node = document.createElement("li");

// 创建文本节点：
const textnode = document.createTextNode("Water");

// 把文本节点附加到 "li" 节点：
node.appendChild(textnode);

// 把 "li" 节点附加到列表：
document.getElementById("myList").appendChild(node);
}
</script>

</body>
</html>
```

<img src="C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231204110414753.png" alt="image-20231204110414753" style="zoom:50%;" />

## 9.实验

静态表单页面指的是static

![image-20231204102852833](C:\Users\13030\AppData\Roaming\Typora\typora-user-images\image-20231204102852833.png)

路由定义为main函数

模板页面为template

