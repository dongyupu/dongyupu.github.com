---
layout: post
title: "4.3 Dockerfile最佳实践"
subtitle:   "容器与容器云第二版4.3"
date:       2021-08-07 16:45:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

## 4.3 Dockerfile最佳实践

&emsp;&emsp;Dockerfile是Docker用来构建镜像的文本文件，包含自定义的指令和格式。可以通过docker build命令从Dockerfile中构建镜像。这个过程与传统分布式集群的编排配置过程相似，且提供了一系列的资源配置语法。用户可以用这些统一的语法命令来根据需求进行配置，通过这份统一的配置文件，在不同的平台上进行分发，需要使用时就可以根据配置文件自动构建，这解决了开发人员构建镜像的复杂过程。同时，Dockerfile与镜像配合使用，使Docker在构建时可以充分利用镜像的功能进行缓存，大大提升了Docker的使用率。

### 4.3.1
  
* 本节主要介绍Docker构建镜像的过程以及Dockerfile的使用方式。
  
* 1.docker build命令和镜像构建过程

* 3.5节中曾详细介绍了docker build命令的使用，知道其参数有3种类型（PATH、-、URL），表示构建上下文（context）的3种来源。这里的构建上下文（简称上下文）是指传入docker build命令的所有文件。一般情况下，将本地主机的一个包含Dockerfile的目录中的所有文件作为上下文。上下文通过docker build命令传入到Docker daemon后，便开始按照Dockerfile中的内容构建镜像。

* Dockerfile描述了组装镜像的步骤，其中每条命令都是单独执行的。除了FROM指令，其它每一条指令都会在上一条指令所生成镜像的基础上执行，执行完后会生成一个新的镜像层，新的镜像层覆盖在原来镜像之上从而形成了新的镜像。Dockerfile所生成的最终镜像就是在基础镜像上面叠加一层层的镜像组建的。

* 为了提高镜像构建的速度，Docker daemon会缓存构建过程中的中间镜像。当从一个已经在缓存中的基础镜像开始构建新镜像时，会将Dockerfile中的下一条指令和基础镜像的所有子镜像做一个新的镜像。在寻找缓存的过程中，COPY和ADD指令与其它指令稍有不同，其它指令只对比生成镜像的指令字符串是否相同；ADD和COPY指令除了要对比字符串，还要对比容器中的文件内容和ADD、COPY所添加的文件内容是否相同。此外，镜像构建过程中，一旦缓存失效，则后续的指令都将生成新的镜像，而不再使用缓存。

* 2.Dockerfile指令

~~~
#Comment
INSTRUCTION arguments
~~~

* 在Dockerfile中，指令（INSTRUCTION）不区分大小写，但是为了与参数区分，推荐大写。Docker会顺序执行Dockerfile中的指令，第一条指令必须是FROM指令，它用于指定构建镜像的基础镜像。在Dockerfile中以#开头的行是注释，而在其它位置出现的#会被当成参数，示例如下：

~~~
#Comment
RUN echo 'we are running some # of cool things'
~~~

* Dockerfile中的指令FROM、RUN、CMD、EXPOSE、ENV、ADD、ENTRYPOINT、VOLUME、USER、WORKDIR、ONBUILD，错误的指令会被忽略。下面详细讲解一些重要的Docker指令
  - ENV
    - 格式：ENV <KEY> <VALUE>或 <key>=<value> ...
    - ENV指令可以为镜像创建出来的容器声明变量。并且在Dockerfile中，ENV指令声明的环境变量会被后面的特定指令（即ENV、ADD、COPY、WORKDIR、EXPOSE、VOLUME、USER）解释使用。其它指令使用环境变量时，使用格式为$variable_name或者${variable_name}。在变量前面添加斜杠\可以转义，如\$foo或者\${foo},将会被分别转换为$foo和${foo}，而不是环境变量所保存的值。另外，ONBUILD指令不支持环境变量替换。
  - FROM
    - 格式：FROM <image> 或 FROM <image>:<tag>
    - FROM指令的功能是为后面的指令提供基础镜像，因此一个有效的Dockerfile必须以FROM指令作为第一条非注释指令。从公共镜像库中拉取镜像很容易，基础镜像可以选择任何有效的镜像。在一个Dockerfile中，FROM指令可以出现多次，这样会构建多个镜像。在每个镜像创建完成后，Docker命令行界面会输出该镜像的ID。若FROM指令中参数tag为空，则tag默认是latest；若参数image或tag指定的镜像不存在，则返回错误。
  - COPY
    - 格式：COPY <src> <dest>
    - COPY指令复制src所指向的文件或目录，将它添加到新的镜像中，复制文件或目录在镜像的路径是<dest>。<src>所指定的源可以有多个，但必须在上下文中，即必须是上下文根目录的相对路径。不能使用形如COPY ../something /something这样的指令。此外，<src>可以使用通配符指向所有匹配通配符的文件或目录，例如，COPY hom* /mydir/表示添加所有以hom开头的文件到目录/mydir/中。
    - <dest>可以是文件或目录，但必须是目标镜像中的绝对路径或者相对于WORDIR的相对路径（WORKDIR即Dockerfile中WORKDIR指令指定的路径，用来为其它指令设置工作目录）。若<dest>以反斜杠/结尾则其指向的是目录；否则指向文件。<src>同理。若<dest>是一个文件，则<src>的内容会被写入到<dest>中；否则<src>所指向的文件或目录中的内容会被复制添加到<dest>中。当<src>指向多个源时，<dest>必须是目录。另外，如果<dest>不存在，则路径中不存在的目录会被创建。
  - ADD
    - 格式：ADD <src> <dest>
    - ADD与COPY指令在功能上很相似，都支持复制本地文件到镜像的功能，但ADD指令还支持其它功能。 <src>可以是一个指向一个网络文件的URL，此时若<dest>指向一个目录，则URL必须是完全路径，这样可以获得该网络文件的文件名filename，该文件会被复制添加到 <dest>/<filename> 。例如，ADD http://example.com/foobar /会创建文件/foobar。
    -  <src> 还可以指向一个本地压缩归档文件，该文件在复制到容器中时会被解压缩提取，如ADD example.tar.xz /。但若URL中的文件为归档文件则不会被解压提取。
    -  ADD和COPY指令虽然功能相似，但一般推荐使用COPY，以为COPY只支持本地文件，相比ADD而言，它更透明。
  - RUN
    - RUN指令有两种格式
      - RUN <command> (shell)
      - RUN ["executable","param1","param2"] (exec格式，推荐格式)
    - RUN指令会在前一条指令创建出的镜像的基础上创建一个容器，并在容器中运行命令，在命令结束运行后提交容器为新镜像，新镜像被Dockerfile中的下一条指令使用。
    - RUN指令的两种格式表示命令在容器中的两种运行方式。当使用shell格式时，命令通过/bin/sh -c运行；当使用exec格式时，命令是直接运行的，容器不调用shell程序，即容器中没有shell程序。exec格式中参数会当成JSON数组被Docker解析，古必须使用双引号而不能使用单引号。因为exec格式不会在shell中执行，所以环境变量的参数不会被替换，例如，当执行CMD ["echo","$HOME"]指令时，$HOME不会做变量替换。如果希望运行shell程序，指令可以写成CMD ["sh","-c","echo","$HOME"]。
  - CMD
    - CMD有3种格式：
      - CMD <command> (shell格式)
      - CMD ["executable","param1","param2"] (exec格式，推荐格式)
      - CMD ["param1","param2"] (为ENTRYPOINT指令提供参数)
    - CMD指令提供容器运行时的默认值，这些默认值可以是一条指令，也可以是一些参数。一个Dockerfile中可以有多条CMD志林个，但只有最后一行有效。CMD ["param1","param2"]格式是在CMD指令和ENTRYPOINT指令配合时使用的，CMD指令中的参数会添加到ENTRYPOINT指令中。使用shell或exec格式时，命令在容器中的运行方式与RUN指令相同。不同在于，RUN指令在构建镜像时执行命令，并生成新的镜像；CMD指令在构建镜像时并不执行任何命令，而是在容器启动时默认将CMD指令作为第一条执行的命令。如果用户在命令行界面运行docker run命令时指定了命令参数，则会覆盖CMD指令中的命令。
  - ENTRYPOINT
    - ENTRYPOINT指令有两种格式
      - ENTRYPOINT <command> (shell)
      - ENTRYPOINT ["executable","param1","param2"] (exec格式，推荐格式)
    - ENTRYPOINT指令和CMD指令类似，都可以让容器在每次启动时执行相同的命令，但它们之间又有不同。一个Dockerfile中可以有多条ENTRYPOINT指令，但只有最后一条ENTRYPOINT指令有效。当使用shell格式时，ENTRYPOINT指令会忽略任何CMD指令和docker run命令的参数，并且会运行在bin/sh -c中。这意味着ENTRYPOINT指令进程为bin/sh -c的子进程，进程在容器中的PID将不再是1，且不能接受Unix信号。即当使用docker stop <container>命令时，命令进程接收不到SIGTERM信号。我们推荐使用exec格式，使用此格式时，docker run传入的命令参数会覆盖CMD指令的内容并且附加到ENTRYPOINT指令的参数中。从ENTRYPOINT的使用中可以看出，CMD可以是参数，也可以是指令，而ENTRYPOINT指令只能是命令；另外，docker run命令提供的运行命令参数可以覆盖CMD，但不能覆盖ENTRYPOINT。
  - ONBUILD
    - 格式：ONBUILD [INSTRUCTION]
    - ONBNUILD指令的功能是添加一个将来执行的触发器指令到镜像中。当该镜像做为FROM指令的参数时，这些触发器指令就会在FROM指令执行时加入到构建过程中。尽管任何指令都可以注册成一个触发器指令，但ONBUILD指令中不能包含ONBUILD指令，并且不会出发FROM和MAINTAINER指令。当需要制作一个基础镜像来构建其它镜像时，ONBUILD是很有用的。例如，当需要构建的镜像是一个可以重复使用的python环境镜像时，它可能需要将应用源代码加入到一个指定目录中，还可能需要执行一个构建脚本。此时不能仅仅调用ADD和RUN指令，因为现在还不能访问应用源代码，并且不能应用的源代码是不同的。我们不能简单的提供一个Dockerfile模版给应用开发者，它与特定应用代码耦合，会引发低效、易错、难以更新等问题。这些场景的解决方案是使用ONBUILD指令注册触发器指令，利用ONBUILD指令构建一个语言栈镜像，该镜像可以构建任何用该语言编写的用户软件的镜像。
    - ONBUILD指令的具体执行步骤如下
    - （1）在构建过程中，ONBUILD指令会添加到触发器指令镜像元数据中。这些触发器指令不会站在当前构建过程中执行。
    - （2）在构架过程最后，触发器志林工会被存储在镜像详情中，其主键是OnBuild，可以使用docker inspect命令查看。
    - （3）之后该镜像可能作为其它Dockerfile中FROM指令的参数。在构建过程中，FROM指令会寻找ONBUILD触发器指令，并且会一它们注册的顺序执行。若有触发器指令执行失败，则FROM指令被终止，并返回失败；若所有触发器指令执行成功，则FROM指令完成并继续执行下面的指令。在镜像构建完成后，触发器指令会被清除，不会背子孙镜像继承。
    - 使用包含ONBUILD指令的Dockerfile构建的镜像应该有特殊标签，如ruby：2.0-onbuild。在ONBUILD指令中添加ADD或COPY指令时要额外注意。加入新构建过程的上下文缺失被添加的资源，那么新构建过程会失败。给OBUILD镜像添加标签，可以提示编写Dockerfile的开发人员小心应对。
### 4.3.2 Dockerfile实践心得

* 在了解如何使用Dockerfile以后，我们总结归纳了以下几点实践心得。在构建Dockerfile文件时，如果遵守这些实践方式，可以更高效地使用Docker。
  
* 使用标签
  * 给镜像打上标签，易读的镜像标签可以帮助了解镜像的功能，如docker build -t=“ruby：2.0-onbuild”
* 谨慎选择基础镜像
  * 选择基础镜像时，尽量选择当前官方库中的镜像。不同镜像的大小不同，目前Linux镜像大小有如下关系：
    * busybox < debian < centos < ubuntu
  * 同时在构建自己的Docker镜像时，只安装和更新必须使用的包。此外，相比Ubuntu镜像，更推荐使用debian镜像，因为它非常轻量级（目前大小是在100MB以下），并且仍然是一个完成的发布版本。
  * FROM指令应该包含参数tag，如使用FROM debian:jessie 而不是FROM debian
* 充分利用缓存
  * Docker daemon会顺序执行Dockerfile中的指令，而且一旦缓存失效，后续命令将不能使用缓存。为了有效地利用缓存，需要保证指令的连续性，尽量将所有Dockerfile文件中相同的部分都放在前面，而将不同的部分放在后面。
* 正确使用ADD与COPY指令
  * 尽管ADD和COPY用法和作用很相近，但COPY仍是首选。COPY相对于ADD而言，功能简单够用。COPY仅提供本地文件向容器的基本复制功能。ADD有额外的一些功能，比如支持复制本地压缩包（复制到容器中会自动解压）和URL远程资源。因此，ADD比较符合逻辑的使用方式是ADD root.tar.gz /。
  * 当在Dockerfile中的不同部分需要用到不同的文件时，不要一次性地将这些文件都添加到镜像中去，而是在需要时逐个添加，这样也有利于充分利用缓存。另外，考虑到镜像大小的问题，如果使用ADD指令去获取远程URL中的压缩包不是推荐的做法。应该使用RUN wget或者RUN curl代替。这样可以删除解压后不再需要的文件，并且不需要在镜像中再添加一层，示例如下。

~~~
#错误的做法
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all

#正确的做法
RUN mkdir -p /usr/src/things \
    && curl -SL http://example.com/big.tar.xz \
    | tar -xJC /usr/src/things \
    && make -C /usr/src/things all
~~~
  * 另外，尽量使用docker volume共享文件，而不是使用ADD或COPY指令添加文件到镜像中。
* RUN指令
  * 为了使Dockerfile易读、易理解和可维护，在使用比较长的RUN指令时可以使用反斜杠\分隔多行。大部分使用RUN指令的场景是运行apt-get命令，在该场景下请注意以下几点。
    * 不要尝试在一行中单独使用RUN apt-get update。当软件资源更新后，这样会引起缓存问题，导致RUN apt-get install指令运行失败。所以RUN apt-get update 和 RUN apt-get install应该写在一行，如RUN apt-get update &&apt-get install -y package-bar package-foo package-baz/
    * 避免使用RUN apt-get upgrade和RUN apt-get dist-upgrade。因为在一个无特权的容器中，一些必要的包会更新失败。如果需要更新一个包（比如foo），直接使用指令RUN apt-get install -y foo
  * 在Dockerfile的核心概念中，提交镜像是廉价的，镜像之间有层级关系，像一棵树。不要害怕镜像的层数过多，我们可以在任一层创建一个容器。因此不要将所有的命令写在一个RUN指令中。RUN指令分层符合Docker的核心概念，这很像源代码控制。
* CMD和ENTRYPOINT指令
  * CMD和ENTRYPOINT指令指定了容器运行的默认命令，推荐二者结合使用。使用exec格式的ENTRYPOINT指令设置固定的默认命令和参数，然后使用CMD指令设置可变的参数。
* 不要在Dockerfile中做端口映射
  * Docker的两个核心概念是可重复性和可移植性，镜像应该可以在任何主机上运行多次。使用Dockerfile的EXPOSE指令，虽然可以将容器端口映射到主机端口上，但会破坏Docker的可移植性，且这样的镜像在一台主机上只能启动一个容器。所以端口映射应该在docker run命令中用-p参数指定。

~~~
#不要在Dockerfile中做如下映射
EXPOSE 80:8080

#仅仅暴露80端口，需要另做映射
EXPOSE 80
~~~

* 使用Dockerfile共享Docker镜像
  * 若要共享镜像，只需共享Dockerfile文件即可。共享Dockerfile文件具有以下优点。
    * Dockerfile文件可以加入版本控制，这样可以追踪文件的变化和回滚错误。
    * 通过Dockerfile文件，可以清楚镜像构建的过程。
    * 使用Dockerfile文件构建镜像具有确定性。