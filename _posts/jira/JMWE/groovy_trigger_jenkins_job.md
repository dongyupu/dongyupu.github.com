---
layout: post
title: "Groovy trigger Jenkins Job with token"
subtitle:   "JWME post function"
date:       2023-04-22 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
### Jenkins : Error 403 No valid crumb was included in the request ...
https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-masters/csrf-protection-explained
- If you authenticate your API calls with a username and a user API token then a crumb is not required from Jenkins 2.96



~~~ groovy
import com.google.common.collect.ImmutableMap
import groovy.json.JsonBuilder
import groovy.json.JsonSlurper
import groovyx.net.http.ContentType
import groovyx.net.http.HTTPBuilder
import groovyx.net.http.Method
import org.apache.http.client.methods.HttpGet
import org.apache.http.impl.client.HttpClientBuilder

def IM(msg){
    def MyhttpBuilder = new HTTPBuilder("http://yourfeishuroboturl/robot/sendFeiShuMsgToUser")
    MyhttpBuilder.request(Method.POST, ContentType.JSON) {
        def data = [
                "title": "Jira " + issue.key,
                "msg":  "Jenkins CI 触发结果："+msg,
                "users": [
                       'dongyupu'
                ]
        ]
        body = new JsonBuilder(data).toString()
        response.success = { resp, JSON ->
            //log.warn(JSON)
        }
        response.failure = { resp, JSON ->
            //log.warn("Failed : " + JSON)
        }
    }
}


def username='dongyupu'
def password='your token'
String userPassBase64 = "$username:$password".toString().bytes.encodeBase64()

def MyhttpCIBuilder
// 默认构建SS2
if(issue.getAsString("customfield_22104")contains('SS2')){
	MyhttpCIBuilder = new HTTPBuilder('https://JENKINS_URL/job/JobName/buildWithParameters?token=SS2&jira_key='+issue.key+'&JIRA_ISSUE_KEY='+issue.key)
}
// 如果 配置平台 为SS3 则构建SS3
if(issue.getAsString("customfield_22104")contains('SS3')){
    MyhttpCIBuilder = new HTTPBuilder('https://JENKINS_URL/job/JobName/buildWithParameters?token=TOKEN&jira_key='+issue.key+'&JIRA_ISSUE_KEY='+issue.key)
}
if(!MyhttpCIBuilder){
  msg = 'failure 配置平台字段配置错误！请联系管理员处理！'
  IM(msg)
}
MyhttpCIBuilder.setHeaders(["Authorization": "Basic $userPassBase64"])
MyhttpCIBuilder.request(Method.POST, ContentType.JSON) {
        def data = []
        body = new JsonBuilder(data).toString()
        response.success = { resp, JSON ->
            log.warn(JSON)
          	msg = 'success'
          	IM(msg)
        }
        response.failure = { resp, JSON ->
            log.warn("Failed : " + JSON)
          	msg = 'failure' + JSON
          	IM(msg)
        }
}





~~~
