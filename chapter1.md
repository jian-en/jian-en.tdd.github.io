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
