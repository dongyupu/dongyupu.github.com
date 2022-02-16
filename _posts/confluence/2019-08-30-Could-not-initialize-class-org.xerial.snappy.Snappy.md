---
layout: post
title: "呈现宏 'jira' 时发生错误Could not initialize class org.xerial.snappy.Snappy"
subtitle:   "Error loading shared library ld-linux-x86-64.so.2"
date:       2019-08-30 12:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---
* 现象：confluence插入 `jira问题过滤器` 无法正常显示，提示 `呈现宏 'jira' 时发生错误Could not initialize class org.xerial.snappy.Snappy`

* 查看日志发下如下错误：

~~~
2019-08-29 15:58:54,530 ERROR [http-nio-8090-exec-6] [xhtml.view.macro.ViewMacroMarshaller] handleMacroExecutionException Error rendering macro: jira                                   
 -- referer: http://confluence.hyxfjr.com/pages/editpage.action?pageId=327691 | url: /pages/rendercontent.action | traceId: f5b98cd46342d5e8 | userName: dongyupu | action: renderconten
java.lang.UnsatisfiedLinkError: /opt/atlassian/confluence/temp/snappy-unknown-94527178-7972-4f75-ab79-2a309b8c0adb-libsnappyjava.so: Error loading shared library ld-linux-x86-64.so.2: 
        at java.lang.ClassLoader$NativeLibrary.load(Native Method)                                                                                                                      
        at java.lang.ClassLoader.loadLibrary0(ClassLoader.java:1941)                                                                                                                    
        at java.lang.ClassLoader.loadLibrary(ClassLoader.java:1824)                                                                                                                     
        at java.lang.Runtime.load0(Runtime.java:809)                                                                                                                                    
        at java.lang.System.load(System.java:1086)                                                                                                                                      
        at org.xerial.snappy.SnappyLoader.loadNativeLibrary(SnappyLoader.java:166)                                                                                                      
        at org.xerial.snappy.SnappyLoader.load(SnappyLoader.java:145)                                                                                                                   
        at org.xerial.snappy.Snappy.<clinit>(Snappy.java:47)                                                                                                                            
        at com.atlassian.confluence.extra.jira.JiraIssuesManager$ByteStreamBasedSupplier.compress(JiraIssuesManager.java:116)                                                           
        at com.atlassian.confluence.extra.jira.JiraIssuesManager$ByteStreamBasedSupplier.<init>(JiraIssuesManager.java:106)                                                             
        at com.atlassian.confluence.extra.jira.Channel.<init>(Channel.java:28)                                                                                                          
        at com.atlassian.confluence.extra.jira.request.JiraChannelResponseHandler.handleJiraResponse(JiraChannelResponseHandler.java:33)                                                
        at com.atlassian.confluence.extra.jira.applink.JiraAppLinkResponseHandler.handle(JiraAppLinkResponseHandler.java:33)                                                            
        at com.atlassian.applinks.oauth.auth.OAuthApplinksResponseHandler$1.handle(OAuthApplinksResponseHandler.java:120)                                                               
        at com.atlassian.applinks.oauth.auth.OAuthApplinksResponseHandler.handle(OAuthApplinksResponseHandler.java:76)                                                                  
        at com.atlassian.plugins.rest.module.jersey.JerseyRequest$2.handle(JerseyRequest.java:134)                                                                                      
        at com.atlassian.sal.core.net.HttpClientRequest.executeAndReturn(HttpClientRequest.java:106)                                                                                    
        at com.atlassian.plugins.rest.module.jersey.JerseyRequest.executeAndReturn(JerseyRequest.java:131)                                                                              
        at com.atlassian.applinks.core.auth.ApplicationLinkRequestAdaptor.execute(ApplicationLinkRequestAdaptor.java:58)                                                               
        at com.atlassian.applinks.oauth.auth.ThreeLeggedOAuthRequest.execute(ThreeLeggedOAuthRequest.java:51)                                                       
        at com.atlassian.confluence.extra.jira.columns.DefaultJiraIssuesManager.retrieveXML(DefaultJiraIssuesManager.java:92)                                                           
        at com.atlassian.confluence.extra.jira.cache.CacheJiraIssuesManager.retrieveXML(CacheJiraIssuesManager.java:99)                                             
        at com.atlassian.confluence.extra.jira.columns.DefaultJiraIssuesManager.retrieveXMLAsChannel(DefaultJiraIssuesManager.java:163)                                       
        at com.atlassian.confluence.extra.jira.JiraIssuesMacro.populateContextMapForStaticTable(JiraIssuesMacro.java:885)                                           
        at com.atlassian.confluence.extra.jira.JiraIssuesMacro.createContextMapFromParams(JiraIssuesMacro.java:441)                         
        at com.atlassian.confluence.extra.jira.JiraIssuesMacro.execute(JiraIssuesMacro.java:1148)                                                                                  
        at com.atlassian.confluence.extra.jira.executor.StreamableMacroFutureTask.renderValue(StreamableMacroFutureTask.java:74)                                    
        at com.atlassian.confluence.extra.jira.StreamableJiraIssuesMacro.marshallMacroInBackground(StreamableJiraIssuesMacro.java:261)                                                  
        at com.atlassian.confluence.extra.jira.StreamableJiraIssuesMacro.executeToStream(StreamableJiraIssuesMacro.java:124)                                        
        at com.atlassian.confluence.content.render.xhtml.view.macro.ViewMacroMarshaller.executeMacro(ViewMacroMarshaller.java:247)                                                      
        at com.atlassian.confluence.content.render.xhtml.view.macro.ViewMacroMarshaller.marshalInternal(ViewMacroMarshaller.java:169)                               
        at com.atlassian.confluence.content.render.xhtml.view.macro.ViewMacroMarshaller.lambda$marshal$0(ViewMacroMarshaller.java:124)         
        at com.atlassian.confluence.impl.content.render.xhtml.analytics.MetricsCollectingMarshaller.marshal(MetricsCollectingMarshaller.java:52)                                        
        at com.atlassian.confluence.content.render.xhtml.view.macro.ViewMacroMarshaller.marshal(ViewMacroMarshaller.java:131)                                       
        at com.atlassian.confluence.content.render.xhtml.view.macro.ViewMacroMarshaller.marshal(ViewMacroMarshaller.java:58)                                                            
        at com.atlassian.confluence.content.render.xhtml.UnmarshalMarshalFragmentTransformer.transform(UnmarshalMarshalFragmentTransformer.java:29)                 
        at com.atlassian.confluence.content.render.xhtml.transformers.DefaultFragmentTransformer.transformFragment(DefaultFragmentTransformer.java:141)                                 
        at com.atlassian.confluence.content.render.xhtml.transformers.DefaultFragmentTransformer.transform(DefaultFragmentTransformer.java:123)                                         
        at com.atlassian.confluence.content.render.xhtml.storage.pagelayouts.StoragePageLayoutUnMarshaller.unmarshal(StoragePageLayoutUnMarshaller.java:75)         
        at com.atlassian.confluence.content.render.xhtml.storage.pagelayouts.StoragePageLayoutUnMarshaller.unmarshal(StoragePageLayoutUnMarshaller.java:28)   
~~~


* 无法加载 `ld-linux-x86-64.so.2` 

* google的结果：
* https://jira.atlassian.com/browse/CONFSERVER-43095

* 发现类似问题，采用最简单的解决办法，设置 `$LD_LIBRARY_PATH` 值

* 我的confluence部署在docker中，所以设置环境变量是最简单的办法。

~~~
cd /
find |grep ld-linux-x86-64.so.2
~~~


* 找到该库位于 `/lib64` 下面

* 于是在yaml文件里增加环境变量

~~~
      - env:
        - name: LD_LIBRARY_PATH
          value: /lib64
~~~

* 重新部署以下容器，然后就可以正常显示了。





