---
layout: post
title: "Jfrog Artifactory Failures while uploading Large Files to Artifactory"
subtitle:   "Failures while uploading Large Files to Artifactory"
date:       2025-09-17 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-03.jpg"
---

# [Failures while uploading Large Files to Artifactory](https://jfrog.com/help/r/artifactory-failures-while-uploading-large-files-to-artifactory/artifactory-failures-while-uploading-large-files-to-artifactory)

As we use Artifactory regularly to store and build the binaries within the Artifactory repository, in our daily work we will be using Artifactory to upload packages on a regular basis.

Based on the requirement and depending on the use case the Artifact size will be growing and you will keep uploading the larger artifacts to the Artifactory repository often. One of the common issues we have seen is “failures during the large files upload”.


There are multiple reasons for the failure while uploading large files.

1.  Uploading from UI can fail due to the size limit which can be achieved by changing the value as given in the Article. The limit does not apply in case you deploy files via REST API or CLI. We also recommend using JFrog CLI to upload large files. The possibility of failures while uploading via UI can be found in the Article

2.  If we are running short on disk space on the Artifactory host we will observe the below error message,  
~~~
022-12-20T17:35:24.367Z [jfrt ] [INFO ] [5b119e7900f] [o.a.e.UploadServiceImpl:465  ] [ttp-nio-8081-exec-98] - Deploy to 'local-repo:test.zip' Content-Length: 48369116923 (estimation) artificial: false

2022-12-20T17:37:57.477Z [jfrt ] [ERROR] [5b119e7900f] [o.a.w.s.RepoFilter:314    ] [ttp-nio-8081-exec-98] - Upload request of local-repo:test.zip failed due to {}

org.jfrog.storage.StorageException: Failed to transform BinaryStream to PreProcessChecksumBinaryStream

Caused by: org.jfrog.storage.binstore.ifc.ClientInputStreamException: Failed to read stream: No space left on device

at org.jfrog.storage.binstore.ifc.ClientStream.read(ClientStream.java:36)

Caused by: java.io.IOException: No space left on device
~~~

Please be informed that during the uploads the artifactory-uploads  and work folder   (located in the $ARTIFACTORY_HOME/data/tmp folder) is used temporarily to save these files before indexing them and when the deployment finishes, they will be deleted from this folder. Thus make sure you have allocated extra storage for the Artifactory server.  


3.  And also most of the time we encounter an "Unexpected EOF" error and a Time out error.  we find it's related to Reverse Proxy. We can test similar uploads by bypassing Nginx or uploading a file from the server where Artifactory is installed and using the localhost address along with the REST API for upload.


Usually, time out occurs at the Application level or due to the Reverse proxy limitations. When we say at the application side Tomcat server plays a very important role in Artifactory.

We can tune the Tomcat server connection timeout value, to increase the connection timeout value for the tomcat layer, for the Artifactory version 7.x, we would need to update the system.yaml with the below parameters in order to set/update the connection timeout.
system.yaml example: 
~~~
artifactory: 
  tomcat: 
    connector: 
      extraConfig: "connectionTimeout='360000'" 
~~~

In 6.x Please set the connectionTimeout  in the “server.xml” under $ARTIFACTOORY_HOME/artifactory/tomcat/conf/server.xml file as shown in the below example and  

Example: 
<Service name="Catalina">
       <Connector port="8081" sendReasonPhrase="true" relaxedPathChars='[]' relaxedQueryChars='[]' maxThreads="200" connectionTimeout="360000"/>
⧉
 Please note a restart will be required after the above changes.

At the reverse proxy level, these errors will occur when there is a default timeout taken into consideration from the proxy configuration before serving the actual request to the client. To overcome this, it is suggestible to add the below parameters to the reverse proxy configuration file used for Artifactory under the virtual host based on your use case.


In case of using apache reverse proxy:  
~~~
Timeout 3600
ProxyTimeout 3600
KeepAliveTimeout 1200
~~~

Further, with regards to the Nginx, as you may know, by default proxy buffering will be enabled and Nginx will try to save the response from the proxied server as soon as possible, saving it into the buffers set by the proxy_buffer_size and proxy_buffers directives, in this case, if the entire response doesn't fit into memory, a part of it can be saved to a temporary file on the disk. Because of this when you are trying to download/upload large files there can be a timeout occurring on the Nginx. In order to overcome this, you may try adding the below parameters to the Nginx configuration used for Artifactory which will help in disabling the buffering and increase the default timeouts
~~~
proxy_read_timeout  3600;
proxy_send_timeout  3600;
proxy_buffering     off;
proxy_request_buffering off;
proxy_http_version  1.1;
proxy_max_temp_file_size 0;
~~~
