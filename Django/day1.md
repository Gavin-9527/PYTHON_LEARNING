## 1.创建项目
### 1.1
- 打开终端
- 进入某个目录（项目放在哪里）``（C:\Users\12943\vscode\projects）
- 执行命令创建项目``
  `` django-admin startproject 项目名  目录
	- manage.py 命令行入口文件
		``` 
		manage.py [项目的管理，启动项目，创建app，数据管理]  不懂  [常用]
		
		_init_.py
		asgi.py [接收网络请求]  不动
		wsgi.py [接收网络请求]  不动
		
		urls.py [URL和函数的对应关系][常常操作]
		setting.py [项目配置]  [常常操作]
		
	- 
		- cd 进入 `projects`目录
		- `python manage.py runserver`启动项目
			- 网址后加`/admin`登录
		- 修改settings里的DATABASES后 
		- `python manage.py migrate`迁移数据库（同步数据库）
		- `python manage.py createsuperuser`创建超级用户
			- `name:admin
			- `password:1294`
		- `python manage.py startapp 应用名`（应用名不可与项目名重名）
		- 在应用models里编写好表数据类后，将模型创建到数据库中去，在项目的settings的INSTALLED_APPS中  加入 models应用路径`apps.blogs`，还需再应用的模型注册到admin里（不注册的话后台管理系统没有此表）
		- ```from apps.blogs.models import Blog
		   admin.site.register([Blog])```
		- 注册完后，`python manage.py makemigrations`（对models改动后）创建迁移文件
		- 再`python manage.py migrate`
	前端渲染
	- 在前端的应用里`view`下创建响应，
	- 然后创建文件夹（放模板），接着在项目settings里TEMPLATES的DIRS设置`os.path.join(BASE_DIR,"模板文件夹")`
	- 在模板文件夹创建一个index.html
	- 在项目urls中导入创建响应的方法,urlpatterns里还要访问此方法

数据库渲染
- 在前端的view里导入后端models的模板
- 在下方调用数据并传入字典(字典注意前面有空格！！！)
- index.html里编写固定语法{% for blog in blogs %}
-  \<div>{{  }}</div>     注意{  }里面前后要有空格
- {% endfor %}

编译器打开文件
 
## settings里
ALLOWED_HOSTS表示主机的权限
['*']表示所有主机均可访问。否则['***.**.**.*']

```python
DATABASES = {

    'default': {

        'ENGINE': 'django.db.backends.mysql',

        'NAME': 'blog',

        'HOST':'localhost',

        'PROT':3306,

        'USER':'root',

        'PASSWORD':'yu18165546583'

    }

}
```
## 终端
进入projects文件 python mange.py runserver启动项目
（若不成功，按下ctrl+c退出terminal运行的程序，输入`python manage.py migrate`)

Django ORM
```
对象关系映射
ORM在业务逻辑层和数据库层之间充当了桥梁的作用。
1.ORM 会将 Python 代码转成为 SQL 语句
2.SQL 语句通过 pymysql 传送到数据库服务端
3.在数据库中执行 SQL 语句并将结果返回


	ORM               数据库
	Models类   对应    数据表
	对象实例    对应    一条记录
	属性        对应    字段
```



注：
应用apps.py里的name应加路径（若不在根目录下）