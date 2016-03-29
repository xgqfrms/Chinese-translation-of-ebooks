# LESSON 29: 网络服务器

***
***

# 网络服务器

#### Up until this point in the book, you have been opening web pages in the browser directly from the file system. Obviously, if you intend your web pages to be viewed by others, you need to make them available over a network. The software used for exposing web pages over a net-work is a web server.

#### 读到书中的这里，你已经可以在文件系统中直接使用浏览器打开网页了。很明显，如果你打算让你的网页被别人浏览到，你需要把这些网页放在一个可以访问的网络上。网络服务器就是一个用于在网络上暴露网页的软件！


#### The term “web server” can either denote the hardware of the underlying server or the software running on that server. For the purposes of this lesson, the term “web server” will be used to denote the software, whereas “server” will be used to denote the hardware this software runs on.

#### 这个术语“网络服务器” 既可以表示底层服务器的硬件，也可以表示运行在服务器上的软件。基于本课的目的，这个术语“网络服务器” 将会被用于指代软件，反之“服务器”将会被用于指代运行该软件的硬件。

#### The primary purpose of a web server is to expose a set of resources from the file system of a network enabled server via protocols such as HTTP and HTTPS.Resources are typically files such as HTML pages, images, and CSS files, to video and audio files.

#### 一个网络服务器的首要目的就是通过HTTP,HTTPS之类的网络协议，暴露一组位于以启用网络服务的服务器的文件系统上的资源。这些资源都是典型的文件，例如 HTML页面，图片，CSS文件，音频和视频文件。

    NOTE  This view is slightly simplistic because many resources in real-world web
    applications are dynamically generated and therefore do not exist on a physical
    file system, but you will ignore this complication for the most part.

    **注释** 这个观点是稍微简单化的，毕竟在真实世界里的网络应用程序的很多资源是动态生成的，
    因此不存在一个物理的文件系统，然而大多数情况下你将会忽略复杂的部分。
    (然而你将会忽略它复杂的大部分。)
...
...

In this lesson, you will migrate the web application developed so far to a web server. You are
doing this for two main reasons:
➤
To give you an understanding of how a web server works: Any web page or web
application you develop will be uploaded eventually to a web server, so it is useful to
gain some understanding of how they work, and how to confi gure one.
➤
Many of the APIs introduced in this section need to run inside web servers: The APIs
included in this section will cover more advanced JavaScript APIs such as storing data
inside the browser. These APIs therefore need a mechanism for segregating the data
from different websites: Without this segregation, any website would be able to access
data stored by any other website, which would obviously create a security loophole.

...
...

URLS
Before looking at web servers, this lesson will quickly cover the related topic of URLs (Uniform
Resource Locators). A URL provides a unique network address for a resource (image, web page,
CSS fi le). The “network” will often be the public Internet, but URLs can also be used to locate
resources on private networks, such as your personal WiFi network at home.
The web server is responsible for parsing the URL and determining the resource that should be
returned. The URL is also used by lower-level protocols, however, to determine how to route the
request to the appropriate server on the network.
URLs are surprisingly complex, but the most familiar pattern is as follows:
http://testing.com:80/test1/test.html
This URL consists of the following components:
➤
http : The protocol that is being used to access the resource; other common protocols are
https and  ftp .
➤
testing.com : The domain name resolved by the browser to an IP address using a Domain
Name Server (DNS). The IP address (for instance 192.168.199.133) in turn maps to a server
running on a network.
➤
80 : The port number of the web server. Because a single server may expose multiple services
(for example, an FTP server and an HTTP server), port numbers provide a mechanism to
logically differentiate them. You will not usually see the port number in URLs because 80 is
the default for the HTTP protocol, and 443 is the default for the HTTPS protocol, and the
port number can therefore usually be omitted.
➤
test1 : The directory on the web server. Typically, the web server will map its root directory
to a directory on the fi le system. In this case, there would be an assumption that this direc-
tory contains a subdirectory called  test1 .
➤
test.html : The name of the resource being accessed.
The two most important components used by the APIs in this section are the domain name and the
port. These are referred to as the “origin” of a resource. Typically, a resource will only be able to
interact with resources or information from the same origin: This is referred to as the same origin
policy.

...
...

CHOOSING A WEB SERVER
There are many web servers available, both commercial and open source. Many factors come into
play when choosing a web server, but these discussions are beyond the scope of this book. It is,
however, worth mentioning that by far the most popular web server, almost since the advent of the
World Wide Web, is the Apache web server.

Apache is an open source web server, and provides an excellent combination of stability, features,
and performance. If you use a hosting service, they will almost certainly make the Apache web
server available to you.
You will not use Apache in this book, mainly because it takes slightly more effort to install and con-
fi gure than the web server you will use, but you may opt to use it if you choose. It can be accessed
from  http://httpd.apache.org/ , and tutorials are available for guiding you through the installa-
tion and confi guration process.
In this book, you will use the Mongoose web server (free edition). The main reason for choosing
this is its simplicity: It requires either very little or no confi guration and is therefore ideal during the
development phase of your web application.

...
...

TRY IT
In this Try It, you install and confi gure the Mongoose web server. This Try It contains two sets of
steps, one for Windows and one for OS X.

    NOTE  Linux source code is also available at:  http://code.google.com/p/
    mongoose/downloads/list . To run this, the simplest option is to  cd to the
    directory containing  contacts.html and run  mongoose . This runs in the
    foreground so use  ctrl-C to stop.

...
...

Lesson Requirements
As part of the steps outlined next, you will need to download the Mongoose web server from the
site listed. This will involve agreeing to the non–commercial license agreement. You will also need
the Chrome web browser to test that the web server is working.
Step-by-Step (OS X)
1. Download the Free Edition OS X installer from:  http://cesanta.com/downloads.html .
This requires you to accept the license agreement.
2. Once this has downloaded, double-click on the DMG fi le and drag it to Applications, just as
you would when installing any other application.
3. Open the Finder and navigate to the Applications folder. Find the Mongoose application, and
double-click on the icon to start the Mongoose server.
4. The Mongoose application can now be confi gured via the icon in the taskbar at the top of the
screen, as shown in Figure 29-1.
[pic](url)
5. Select the Edit confi guration option. This will open a browser window with the confi guration
settings. Locate the  document_root text fi eld, and change the directory to the directory that
contains the  contacts.html fi le. For example  /Users/dane/html5/CRM . Once entered,
click “Save settings to the confi g fi le”.
6. The Mongoose server operates by default on port 8080 rather than port 80. Because it is
running on your local machine, you can also use the hostname  localhost . Therefore, to
open the contacts page, open Chrome, and enter http://localhost:8080/contacts.html. This
should show the main contacts web page.

...
...

Step-by-Step (Windows)
1. Download the Free Edition OS X installer from  http://cesanta.com/downloads.html .
This requires you to accept the license agreement.
2. Once the download is complete, copy the  .exe fi le to the same directory that contains
contacts.html .
3. Double-click on the executable to start Mongoose.
4. The Mongoose application can now be confi gured via the icon in the taskbar at the bottom
of the screen, as shown in Figure 29-2 (although no confi guration is required in this case).
[pic](url)
5. The Mongoose server operates by default on port 8080 rather than port 80. Because it is
running on your local machine, you can also use the hostname  localhost . Therefore, to
open the contacts page, open Chrome, and enter http://localhost:8080/contacts.html.
This should show the main contacts web page.
    REFERENCE  Please go to the book’s website at  [www.wrox.com/go/html5jsj-
    query24hr](http://www.wrox.com/go/html5jsj-query24hrto)  to view the video for Lesson 29, as well as download the code and
    resources for this lesson.
...
...

