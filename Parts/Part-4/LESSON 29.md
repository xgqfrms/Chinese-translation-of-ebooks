# LESSON 29: 网络服务器

***
***

# 网络服务器

 Up until this point in the book, you have been opening web pages in the browser directly from the file system. Obviously, if you intend your web pages to be viewed by others, you need to make them available over a network. The software used for exposing web pages over a net-work is a web server.

#### 读到书中的这里，你已经可以在文件系统中直接使用浏览器打开网页了。很明显，如果你打算让你的网页被别人浏览到，你需要把这些网页放在一个可以访问的网络上。网络服务器就是一个用于在网络上暴露网页的软件！


The term “web server” can either denote the hardware of the underlying server or the software running on that server. For the purposes of this lesson, the term “web server” will be used to denote the software, whereas “server” will be used to denote the hardware this software runs on.

#### 这个术语“网络服务器” 既可以表示底层服务器的硬件，也可以表示运行在服务器上的软件。基于本课的目的，这个术语“网络服务器” 将会被用于指代软件，反之“服务器”将会被用于指代运行该软件的硬件。

 The primary purpose of a web server is to expose a set of resources from the file system of a network enabled server via protocols such as HTTP and HTTPS.Resources are typically files such as HTML pages, images, and CSS files, to video and audio files.

#### 一个网络服务器的首要目的就是通过HTTP,HTTPS之类的网络协议，暴露一组位于启用网络服务的服务器的文件系统上的资源。这些资源都是典型的文件，例如 HTML页面，图片，CSS文件，音频和视频文件。

    NOTE  This view is slightly simplistic because many resources in real-world web
    applications are dynamically generated and therefore do not exist on a physical
    file system, but you will ignore this complication for the most part.

    注释 这个观点是稍微简单化的，毕竟在真实世界里的网络应用程序的很多资源是动态生成的，
    因此不存在一个物理的文件系统，然而大多数情况下你将会忽略复杂的部分。
    (然而你将会忽略它复杂的大部分。)
...
...

 In this lesson, you will migrate the web application developed so far to a web server. You are doing this for two main reasons:

#### 在本课，你将学会把到目前为止开发的网络应用程序迁移到一个网络服务器上。你这样做有两个主要原因：

  ➤
   To give you an understanding of how a web server works: Any web page or web application you develop will be uploaded eventually to a web server, so it is useful to gain some understanding of how they work, and how to configure one.
#### ➤
#### 为了让你明白网络服务器是如何工作的：你开发的任何网页或网络应用程序最终都会被上传到一个网络服务器上，所以这是非常有用的对于你理解他们是如何工作的，以及如何配置一个网络服务器。
  ➤ 
   Many of the APIs introduced in this section need to run inside web servers: The APIs included in this section will cover more advanced JavaScript APIs such as storing data inside the browser.
   These APIs therefore need a mechanism for segregating the data from different websites: Without this segregation, any website would be able to access data stored by any other website, which would obviously create a security loophole.
#### ➤
#### 本节课介绍的很多API都需要运行在网络服务器中：本节课包含的这些API会涉及更高级的JavaScript API, 例如在浏览器中存储数据。这些API因此需要一个机制用来隔离来自于不同网站的数据：没有这种隔离机制，任何网站都可以访问到其他网站存储的数据，这明显会创造出一个安全漏洞。
...
...

URLS
Before looking at web servers, this lesson will quickly cover the related topic of URLs (Uniform
Resource Locators). A URL provides a unique network address for a resource (image, web page,
CSS file). The “network” will often be the public Internet, but URLs can also be used to locate
resources on private networks, such as your personal WiFi network at home.

#### URLS(统一资源定位符)
#### 在了解网络服务器之前，本节课将会快速的介绍与URLs(统一资源定位符)相关的话题。一个URL为资源(图像，网页，CSS文件)提供了一个唯一的网络地址。这个“网络”通常是指互联网，但是 URLs也可以被用来访问位于私有网络上的资源，比如你家里的wifi网络。



The web server is responsible for parsing the URL and determining the resource that should be
returned. The URL is also used by lower-level protocols, however, to determine how to route the
request to the appropriate server on the network.

#### 网络服务器是负责用来解析URL(网址)和决定什么样的资源应该被返回。网址也被用于一些低级的协议，然而，主要是为了确定如何将请求路由到位于网络上的适当的服务器上。

URLs are surprisingly complex, but the most familiar pattern is as follows:
    http://testing.com:80/test1/test.html
    
#### URLs 是非常复杂的，但是最常用的模式是下面这样的：
    http://testing.com:80/test1/test.html


This URL consists of the following components:
#### 这个URL包含以下几个部分：
➤
http : The protocol that is being used to access the resource; other common protocols are https and  ftp .

#### http : 这个协议是被用于访问资源；其他常用的协议是https和ftp.
➤
testing.com : The domain name resolved by the browser to an IP address using a Domain
Name Server (DNS). The IP address (for instance 192.168.199.133) in turn maps to a server running on a network.

#### testing.com : 这个域名会被浏览器通过一个域名服务器解析成一个IP地址。这个IP地址(例如 192.168.199.133)会被映射到一个运行在网络上的服务器。
➤
80 : The port number of the web server. Because a single server may expose multiple services
(for example, an FTP server and an HTTP server), port numbers provide a mechanism to
logically differentiate them. You will not usually see the port number in URLs because 80 is
the default for the HTTP protocol, and 443 is the default for the HTTPS protocol, and the
port number can therefore usually be omitted.

#### 80 : 这台网络服务器的端口号。由于一台独立的服务器可能会暴露多种服务(例如，一个FTP服务器和一个HTTP服务器)，端口号提供了一种机制用来在逻辑上区分它们。你通常不会在URL中看到端口号，因为80是HTTP协议默认的端口号，443是是HTTPS协议默认的端口号, 所以端口号通常会被省略掉。

➤
test1 : The directory on the web server. Typically, the web server will map its root directory
to a directory on the file system. In this case, there would be an assumption that this direc-
tory contains a subdirectory called  test1 .

#### test1 : 网络服务器的目录。典型地，网络服务器会映射他的根目录到一个位于文件系统上的目录。在本例中，假设这个根目录下有一个名为test的子目录。

➤
test.html : The name of the resource being accessed.

The two most important components used by the APIs in this section are the domain name and the
port. These are referred to as the “origin” of a resource. Typically, a resource will only be able to
interact with resources or information from the same origin: This is referred to as the same origin
policy.

#### test.html : 被访问的资源名。域名和端口号是本节课使用到的API中最重要的两个部分。他们被称为一个资源的“起源”。通常情况下，资源只能与来自同一起源的资源或信息进行交互：这被称为同源策略。

...
...

CHOOSING A WEB SERVER
#### 选择一个网络服务器

There are many web servers available, both commercial and open source. Many factors come into
play when choosing a web server, but these discussions are beyond the scope of this book. It is,
however, worth mentioning that by far the most popular web server, almost since the advent of the
World Wide Web, is the Apache web server.

#### 有很多可用的网络服务器，既有商业的也有开源的。当我们选择一个网络服务器是要考虑很多因素，但是这些讨论不在本书的范围之内。(但这些讨论超出了本书的范围。)然而，这是值得一提的目前为止最流行的网络服务器，几乎自从万维网问世以来，就是Apache网络服务器。

Apache is an open source web server, and provides an excellent combination of stability, features,
and performance. If you use a hosting service, they will almost certainly make the Apache web
server available to you.

#### Apache是一个开源的网络服务器，它提供一个有稳定性，特性和性能构成的优秀的联合体。如果你是用一个主机服务，他们几乎肯定会提供给你可以使用的Apache网络服务器。

You will not use Apache in this book, mainly because it takes slightly more effort to install and con-
figure than the web server you will use, but you may opt to use it if you choose. It can be accessed
from  http://httpd.apache.org/ , and tutorials are available for guiding you through the installation and configuration process.

#### 本书中你不会使用Apache网络服务器，主要是因为它比你要使用的网络服务器需要花费稍微额外的努力去安装和配置，但如果你选择，你可以选择使用它。它可以被访问从http://httpd.apache.org/ ，并且有可用的教程来指导你从头到尾完成安装和配置的过程。

In this book, you will use the Mongoose web server (free edition). The main reason for choosing this is its simplicity: 
It requires either very little or no configuration and is therefore ideal during the development phase of your web application.

#### 本书中，你将会用到Mongoose网络服务器(免费版)。选择它的主要原因是它很简单：它需要很少或不用配置，因而，在你的网络应用程序的开发阶段是理想的。


...
...

TRY IT
#### 试一试 

In this Try It, you install and configure the Mongoose web server. This Try It contains two sets of
steps, one for Windows and one for OS X.

#### 在这个试一试 ，你将安装和配置Mongoose网络服务器。这个试一试包含了两套步骤，一个用于Windows和一个用于OS X。

    NOTE  Linux source code is also available at: (http://code.google.com/p/mongoose/downloads/list) . 
    To run this, the simplest option is to  cd to the
    directory containing  contacts.html and run  mongoose .
    This runs in the foreground so use  ctrl-C to stop.
	
	注释 Linux 源代码也可以从http://code.google.com/p/mongoose/downloads/list获得。
	为了运行它，最简单的选择是通过命令行打开contacts.html所在的目录，并且运行mongoose。
	这是运行在前台的，所以所以Ctrl+C 来停止它。
...
...

Lesson Requirements

As part of the steps outlined next, you will need to download the Mongoose web server from the site listed.
This will involve agreeing to the non–commercial license agreement.
You will also need the Chrome web browser to test that the web server is working.

#### 课程要求

#### 作为下一步的步骤的一部分，你需要从站点列表下载Mongoose网络服务器。这将会涉及接受非商业许可协议。同时你也需要个Chrome网络浏览器，用来测试网络服务器是否正常工作。
 
Step-by-Step (OS X)
#### 一步接一步

1. Download the Free Edition OS X installer from:  http://cesanta.com/downloads.html .
This requires you to accept the license agreement.

#### 1.下载免费版的 OS X安装包，从[http://cesanta.com/downloads.html]( http://cesanta.com/downloads.html) .这要求你接受许可协议。

2. Once this has downloaded, double-click on the DMG file and drag it to Applications, just as
you would when installing any other application.

#### 2.一旦下载完成，双击这个DMG文件，并把它拖到应用程序中，就像当你安装其他应用程序一样。

3. Open the Finder and navigate to the Applications folder. Find the Mongoose application, and
double-click on the icon to start the Mongoose server.

#### 3. 打开Finder并且导航到应用程序文件夹。找到Mongoose应用程序，在小图标上双击来启动Mongoose网络服务器。

4. The Mongoose application can now be configured via the icon in the taskbar at the top of the
screen, as shown in Figure 29-1.
[pic](url)

#### 4. 可以通过位于屏幕顶部任务栏上的小图标来配置Mongoose 应用程序，如图29-1中所示：
![FIGURE 29-1](https://github.com/xgqfrms/Chinese-translation-of-ebooks/blob/gh-pages/images/part%20img/p29-1.PNG)
FIGURE 29-1

5. Select the Edit configuration option. This will open a browser window with the configuration
settings. Locate the  document_root text field, and change the directory to the directory that
contains the  contacts.html file. For example  /Users/dane/html5/CRM . Once entered,
click “Save settings to the config file”.

#### 5.选择编辑配置选项。这将会打开一个包含配置设置的浏览器窗口。定位到document_root的文本字段，改变这个目录，使其包含contacts.html文件。例如 /Users/dane/html5/CRM 。一旦输入，点击“将设置保存到配置文件”。

6. The Mongoose server operates by default on port 8080 rather than port 80. Because it is
running on your local machine, you can also use the hostname  localhost . Therefore, to
open the contacts page, open Chrome, and enter http://localhost:8080/contacts.html. This
should show the main contacts web page.

#### 6. Mongoose 网络服务器运行在默认的8080端口，而不是80端口。因为它运行在你的本地机器上，你也可以使用主机名localhost。因此，要想打开通信录网页，打开chrome输入http://localhost:8080/contacts.html. 这应显示主要联系人的网页。


...
...

Step-by-Step (Windows)
#### 一步接一步

1. Download the Free Edition Windows installer from  http://cesanta.com/downloads.html .
This requires you to accept the license agreement.

#### 1.下载免费版的Windows安装包，从[http://cesanta.com/downloads.html]( http://cesanta.com/downloads.html) .
这要求你接受许可协议。

2. Once the download is complete, copy the  .exe file to the same directory that contains
contacts.html .

#### 2.一旦下载完成，复制这个.exe文件到与contacts.html所在目录相同的目录下。

3. Double-click on the executable to start Mongoose.

#### 3.双击这个可执行文件，启动Mongoose。

4. The Mongoose application can now be configured via the icon in the taskbar at the bottom
of the screen, as shown in Figure 29-2 (although no configuration is required in this case).
[pic](url)

#### 4.现在可以通过位于屏幕底部任务栏上的小图标来配置Mongoose 应用程序，如图29-2中所示：
![Figure 29-2](https://github.com/xgqfrms/Chinese-translation-of-ebooks/blob/gh-pages/images/part%20img/p29-2.PNG)
Figure 29-2

5. The Mongoose server operates by default on port 8080 rather than port 80. Because it is
running on your local machine, you can also use the hostname  localhost . Therefore, to
open the contacts page, open Chrome, and enter http://localhost:8080/contacts.html.
This should show the main contacts web page.

#### 5. Mongoose 网络服务器运行在默认的8080端口，而不是80端口。因为它运行在你的本地机器上，你也可以使用主机名localhost。因此，要想打开通信录网页，打开chrome输入http://localhost:8080/contacts.html.

    REFERENCE  Please go to the book’s website at [](http://www.wrox.com/go/html5jsj-query24hrto) 
    to view the video for Lesson 29, as well as download the code and resources for this lesson.
	
	参考 请访问这本书的网站，在[](http://www.wrox.com/go/html5jsj-query24hrto)，
	观看Lesson 29的视频，也可以下载本课的源代码和相关资源。


	
...
...

