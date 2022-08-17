---
layout: post
title: "script runner Fragment button post requests to export feishu pdf"
subtitle:   "script runner Fragment button post requests to export feishu pdf,and show a flag message"
date:       2022-08-16 2:21:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---

## Custom web itemHelp
### Creates a web item (a button or link) at the location you specify
* http://yourjira.com/rest/scriptrunner/latest/custom/makefeishupdf?issueId=$issue.key
<br>
<img src="{{ site.baseurl }}/img/jira/scriptrunner/post_requests_feishu_pdf.jpg" />
<br>



## Custom endpointHelp
### Define a REST endpoint in a script

~~~ groovy

import groovy.json.JsonBuilder
import groovyx.net.http.ContentType
import groovyx.net.http.HTTPBuilder
import groovyx.net.http.Method

import javax.ws.rs.core.Response

import com.onresolve.scriptrunner.runner.rest.common.CustomEndpointDelegate
import groovy.transform.BaseScript

import javax.ws.rs.core.MultivaluedMap

import com.atlassian.jira.component.ComponentAccessor
import com.atlassian.jira.issue.Issue

import groovy.json.JsonOutput
import javax.servlet.http.HttpServletRequest
import com.atlassian.sal.api.user.UserManager

@BaseScript CustomEndpointDelegate delegate

def PDF(data){
  def MyhttpBuilder = new HTTPBuilder("http://yourjira.com/gateway/export/jira/")
	MyhttpBuilder.request(Method.POST, ContentType.JSON) {
    	
    	body = new JsonBuilder(data).toString()
        log.warn(body)
		response.success = { resp, JSON ->
            log.warn(JSON)
            return 'ok'
    	}
    	response.failure = { resp, JSON ->
        	log.warn("Failed : " + JSON)
            return JSON
    	}
	}
}

makefeishupdf(httpMethod: "GET") { MultivaluedMap queryParams, String body, HttpServletRequest request ->
    def userManager = ComponentAccessor.getOSGiComponentInstanceOfType(UserManager)
    def userProfile = userManager.getRemoteUser(request)

    def issuekey = queryParams.getFirst("issueId") as String
    log.warn(issuekey)
    
	def data = [
        "type": "frd", 
        'email': userProfile?.email, 
        "epic_ids": [issuekey]
         ]
    log.warn('post data:')
    log.warn(data)
	def resp_pdf = PDF(data)
	if(resp_pdf == 'ok'){
        def flag = [
                    type : 'success',
                    title: "飞书PDF生成任务触发成功",
                    close: 'auto',
                    body : "飞书PDF生成任务触发成功"
        ]
        Response.ok(JsonOutput.toJson(flag)).build()
    }
    else{
         def flag = [
                    type : 'success',
                    title: "飞书PDF生成任务触发失败",
                    close: 'auto',
                    body : resp_pdf.toString()
        ]
        Response.ok(JsonOutput.toJson(flag)).build()
    }
}


~~~
