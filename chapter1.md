[原文链接](http://chimera.labs.oreilly.com/books/1234000000754/ch01.html)

#第一章：用一个功能测试来启用Django
TDD不是与生俱来的东西，它是一项纪律，就像很多功夫电影中的武术，你需要一位蛮不讲理的和暴脾气的师傅来逼迫你学习这项纪律。我们的师傅就是测试山羊.

##听测试山羊的话！在有一个测试前不要做任何事情
测试山羊在Python测试社区是TDD的民间吉祥物，对于不同人它可能意味着不同的事情，但对我来说，测试山羊是我脑海中的一种声音，就像卡通片里当遇到烦恼时，总有小天使或者小魔鬼跳到你的肩膀上。我希望你们在阅读这本书的过程中也能把测试山羊装到脑海中。

我们已经决定要创建一个网站，尽管我们还不太确定我们要用这个网站做什么。一般来说网站开发的第一步是安装和配置网站框架。*下载这个包，安装那个包，配置这个文件，运行那个脚本*，但TDD需要另一种思维模式。当你在执行TDD时，你脑海中始终有一只固执的测试山羊咩咩叫：“先写测试，先写测试！”

在TDD中第一步总是不变的：*写一个测试*。

第一步写测试然后运行测试，看到它如预想到的一样没通过。*然后*我们才开始构建程序。把这些用羊咩咩的声音重复说给你自己。反正我是这样做的。

山羊每次都只迈出一步，所以不管山有多险，他们很少从山上摔下来。请欣赏下图：

![山羊比你想象的灵活多了](http://orm-chimera-prod.s3.amazonaws.com/1234000000754/images/twdp_0101.png)

我们将会小碎步优雅地前进，我们会使用很流行的Python网页框架Django来构建应用。

首要的事情是成功安装Django，我们检查的方法是确定我们能够开启Django的开发服务器并且在我们本机的浏览器上看到它提供的网页，我们会使用Selenium浏览器自动化工具完成这些。

在你的项目目录下创建一个名为*functional_tests.py*的python文件，文件内容如下。如果在你输代码时，你想发出一些噪音，这对你有帮助：

functional_tests.py.

	from selenium import webdriver
	
	browser = webdriver.Firefox()
	browser.get('http://localhost:8000')
	
	assert 'Django' in browser.title

> **告别罗马数字**
> 很多介绍TDD的内容都会使用罗马数字作为例子，这已经成为了一个笑话 － 我甚至也写了一个例子，如果你感兴趣，可以看看我的[Github](https://github.com/hjwp/tdd-roman-numeral-calculator/)。
> 罗马数字作为一个例子，也好也不好，它是一个‘玩具’问题，涉及的范围有限，但你能够用它解释清楚TDD。
> 问题在于很难把它很现实世界联系起来，这也是为什么我决定从零开始构建一个真正的网页应用，把它作为例子。虽然这是一个简单的网页应用，我希望当你开始下一个真正的项目时能够变得容易一些。

这就是我们第一个功能性测试（以下简称FT），我之后将解释FT以及FT与单元测试的区别。现在我们确定理解了代码做的事情就够了：
* 使用Selenium网页驱动打开一个Firefox窗口
* 用它打开我们想从本机打开的网页
* 检查（使用测试断言）网页的标题中有"Django"这个单词

我们先运行一下这段代码：

	$ python3 functional_tests.py (python functional_tests.py)
	Traceback (most recent call last):
	  File "functional_tests.py", line 6, in <module>
       assert 'Django' in browser.title
	AssertionError

你应该看见一个浏览器窗口自动打开并试图打开*localhost:8000*，然后上面的错误就出现了，看到桌面上等待你关闭的浏览器窗口你可能会生气，不急，我们呆会来修复错误。

>  当你想引入Selenium时如果你遇到了错误，可能需要你再看看[预备知识和准备工作](http://chimera.labs.oreilly.com/books/1234000000754/pr02.html)这一章节。

现在我们有一个*failing test*，这意味着我们即将开始创建应用。

## 让Django跑起来吧
你肯定已经阅读过[预备知识和准备工作](http://chimera.labs.oreilly.com/books/1234000000754/pr02.html)，所以你已经安装了Django。让Django跑起来的方法是创建一个*项目(project)*，项目就是我们要建立网站的地方，Django提供了如下的命令行工具建立项目:

	$ django-admin.py startproject superlists

这将会建立如下的目录结构：

	.
	├── functional_tests.py
	└── superlists
    	├── manage.py
    	└── superlists
        	├── __init__.py
        	├── settings.py
        	├── urls.py
        	└── wsgi.py

没错，在*superlists*中还有一个*superlists*文件夹，这看上去可能令人费解，但是如果你了解Django的历史，这些是有原因的。现在你只需要记住*superlists/superlists*目录下的文件是应用在整个项目中的－比如*settings.py*，这里写了网站的全局配置信息。

你可能也注意到了*manage.py*，这可是Django的瑞士军刀，它其中一个功能是运行一个开发服务器，我们现在试一试。输入**cd superlists**进入*superlists*文件夹（很多工作都会从这个目录开始）然后运行：

	$ python3 manage.py runserver
	Performing system checks...

	System check identified no issues (0 silenced).

	You have unapplied migrations; your app may not work properly 	until they are
	applied.
	Run 'python manage.py migrate' to apply them.

	Django version 1.8, using settings 'superlists.settings'
	Development server is running at http://127.0.0.1:8000/
	Quit the server with CONTROL-C.
	
>现在可以忽略"unapplied migrations"，我们会在[第五章](http://chimera.labs.oreilly.com/books/1234000000754/ch05.html)了解migrations。

现在打开另一个命令行终端，在其中再次运行我们的测试文件（在最开始的目录下）：

	$ python3 functional_tests.py
	$

在命令行中并没有错误信息，但是你能发现两点：1. 没有AssertError信息了 2. Selenium启动的Firefox窗口显示了不同的网页。

好吧，这看上去并不怎样，但是也要庆祝一下！这是我们第一个通过的测试。

如果这感觉虚幻地像魔术，那我们自己打开浏览器访问：http://localhost:8000，你会看到一个很类似的网页：

![welcome to django](http://orm-chimera-prod.s3.amazonaws.com/1234000000754/images/twdp_0102.png)

你可以使用Ctrl+C退出开发服务器回到正常的shell中。

## 建立Git仓库

在我们结束这一章之前还要做一件事情：把我们的代码提交到版本控制系统(VCS)，如果你是一个有经验的程序员你不需要听我讲授版本控制，但如果你是一个新手请相信我VCS是必备技能。随着你的项目从最初的几行代码到几周后的状态，有一个可用的工具帮助自己审查旧版本的代码，撤回改动，安全地探索新想法甚至是备份，这太重要了！TDD离不开版本控制，所以我必须教会大家怎么在TDD流程中使用版本控制。

那么，我们第一次提交，永远都不会晚，我们会使用Git作为我们的版本控制系统，因为它是最好的。

我们先把*functional_tests.py*移到*superlists*文件夹中，使用**git init**建立代码库:

	$ ls
	superlists          functional_tests.py
	$ mv functional_tests.py superlists/
	$ cd superlists
	$ git init .
	Initialised empty Git repository in /workspace/superlists/.git/

>从现在开始，顶层的*superlists*目录就是我们的工作目录，每当我敲击一个命令时，默认的路径就是这个目录，类似的，每当我提到一个文件的目录时，都是相对于*superlists*目录的。因此*superlists/settings.py*指的是第二层superlists目录下的*settings.py*文件。很清楚吧？如果还觉得困惑，找到*manage.py*，你要和*manage.py*在同一个目录下。

现在我们看看我们需要提交哪些文件：

	$ ls
	db.sqlite3  manage.py   superlists  functional_tests.py

**db.sqlite3**是数据库文件，我们不想让它出现在代码控制系统中，我们把它加入到一个名为*.gitignore*的特殊文件中告诉Git忽略哪些文件：

	$ echo "db.sqlite3" >> .gitignore

下来我们把当前目录的其他文件加进去，使用 ".":

	$ git add .
	$ git status
	On branch master

	Initial commit

	Changes to be committed:
  		(use "git rm --cached <file>..." to unstage)

	        new file:   .gitignore
	        new file:   functional_tests.py
	        new file:   manage.py
	        new file:   superlists/__init__.py
	        new file:   superlists/__pycache__/__init__.cpython-34.pyc
	        new file:   superlists/__pycache__/settings.cpython-34.pyc
	        new file:   superlists/__pycache__/urls.cpython-34.pyc
	        new file:   superlists/__pycache__/wsgi.cpython-34.pyc
	        new file:   superlists/settings.py
	        new file:   superlists/urls.py
	        new file:   superlists/wsgi.py

糟糕，我们加进去了许多*.pyc*文件，提交这些文件是没有意义的，我们把它从Git中移出去也加到*.gitignore*文件中：

	$ git rm -r --cached superlists/__pycache__
	rm 'superlists/__pycache__/__init__.cpython-34.pyc'
	rm 'superlists/__pycache__/settings.cpython-34.pyc'
	rm 'superlists/__pycache__/urls.cpython-34.pyc'
	rm 'superlists/__pycache__/wsgi.cpython-34.pyc'
	$ echo "__pycache__" >> .gitignore
	$ echo "*.pyc" >> .gitignore
	
现在