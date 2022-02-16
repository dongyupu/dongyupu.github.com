---
layout: post
title: "关于代码评审的思考"
subtitle:   "code review thinking"
date:       2016-02-28 10:18:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---

## 代码评审的思考

- 我们建立代码评审的机制，选择了合适的工具，但似乎一切并没有想象中的那么简单。
	
## 思考：

- 什么是code review ？

	- 代码评审是一个流程，开发人员写好代码后，需要让别人来review检查一下他的代码，找出问题，改进代码。越早发现问题，修复的成本就越低。
	
	- 我们应该评审的内容：
		- 编码规范：命名规范、注释格式、log要求......
		- 代码结构：重复代码、分层不当......
		- 工具、框架使用：使用开源框架工具前应确认使用的协议、STL库函数禁止使用...
		- 错误验证、异常处理、事物划分、线程、性能、安全、可读性、扩展性、测试覆盖度
		- 代码评审不负责检查功能、逻辑是否符合预期，这些应该由UT和QA来保证。
		
	- 为什么要做code review
		- 1.保证项目质量
			- code review可以通过reviewers的建议增进代码的质量，也能提高owner的编程态度
			- code review有利项目流转，可以让其它并不熟悉代码的人知道作者的意图和想法，从而在以后轻松维护代码
			- code review鼓励程序员们相互学习对方的长处和优点，同时可以达到知识传播
			- code review可以用来确认自己的设计和实现是清楚和合理的
			- code review帮助找到程序的bug和保证代码风格和编码标准
			
		- 2.加速个人和团队的成长，知识和经验的积累。
		
- 我们实践中的一些问题：

	1.  时间不足问题：在我们的流程里面并没有安排专门的时间给reviewer去 评审代码 ，导致很多时候评审流于形式
	2.  需求变化频繁：需求变化快，代码生命周期短，每天忙忙多的版本提交，根本来不及评审
	3.  态度问题：不想精益求精或者评审的reviewer敷衍了事，恶性循环就此产生
	4.  没有统一的评审标准：评审很混乱
	5.  静态检测工具使用不足：应该先在本地运行这些工具进行自检与修改
	
## Single Review与Review Meeting

### review meeting

- 代码评审会议，我称之为Code Review Meeting，就是将团队成员都组织起来开会，让代码Owner上去讲自己代码的实现和思路，其它人发表意见和进行讨论。
 
	- 优点： 
	
		- 团队内新技术/新观点的交流Meeting、项目开发思路、解决方案讨论，偏头脑风暴式；

		- 各类项目都适合进行；
			
	- 缺点： 
	
		- 依赖于主持者（项目Owner）的素质、时间成本高（为会议上所有人时间的总和）；

		- 集体评审的代码行数有限；


### Single Review或者叫做交叉评审

- 项目owner提交代码之后，让reviewer在空闲的时候帮忙评审代码，并且写出批注，owner收到批注后，进行修改或者回复。但注意这里的reviewer并不是只有技术主管或架构师之类的才能做，代码质量监管仅仅靠架构师是不够的，需要所有经验丰富或有专长的同学参与其中。

	- 优点： 
		- 更偏重与具体的代码评审，人员分散参与，评审代码行数有保证；
		-  时间自由，Reviewer什么时候进行评审时间可自控；
	- 缺点： 
		- 流程稍复杂，只适用于重要项目或核心模块；	
- 如何平衡
  - 采用Review Meeting与Single Review相结合的模式。 即每次代码提交后，针对代码的变更进行Single Review，然后每周进行一次Review Meeting。
 
## 工具的选择：
	> gerrit-jenkins-jira-confluence
	
- 我们目前是使用上面这些工具来进行实践的。
	> gerrit和jenkins做代码评审和持续集成，jira作为任务和bug管理追踪，confluence作为wike和需求管理工具。
	
- 除了Google的gerrit还有哪些工具可以选择：

# Facebook的 [Phabricator](http://www.Phabricator.org) 

- 这个工具的好处在于它做的比较多，不像gerrit仅仅做代码的评审。Phabricator是开源的，支持各种主流的版本控制工具，有代码评审功能、任务管理功能（支持拖拽的面板）、bug跟踪、Wiki、邮件通知系统等等
	
	- Host Git/Mercurial/SVN Repositories, or connect other hosts
	- Browse and Audit Source Code
	- Track bugs
	- Write Things Down
	- Touch your Tasks
	- Co-ordinate Lunch Plans
	- Watch for Danger
	- CLI
	- API
		
这个可以试试，可以摆脱jira这个收费的软件（最重要的是自己部署不受其他部所限制）。


