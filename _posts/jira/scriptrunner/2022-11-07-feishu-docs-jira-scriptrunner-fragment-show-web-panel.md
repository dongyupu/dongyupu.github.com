---
layout: post
title: "Jira集成飞书文档"
subtitle:   "在问题详情页集成飞书文档"
date:       2022-11-06 2:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-01.jpg"
---
## [Jira集成飞书文档](https://open.feishu.cn/document/uYjL24iN/uYDO3YjL2gzN24iN3cjN/introduction?lang=en-US)

~~~
// 飞书云文档 集成
import groovy.json.JsonBuilder
import groovyx.net.http.ContentType
import groovyx.net.http.HTTPBuilder
import groovyx.net.http.Method
import javax.ws.rs.core.Response
import java.security.*
import javax.script.*

// 获取应用的 tenant_access_token
def feishu_app_token(){
    def MyhttpBuilder = new HTTPBuilder("https://open.feishu.cn/open-apis/auth/v3/app_access_token/internal/")
	MyhttpBuilder.request(Method.POST, ContentType.JSON) {
    	def data = [
        	    "app_id": 'cli_XXX',
                "app_secret": 'XXXXXXXX'
			]
    	body = new JsonBuilder(data).toString()
        log.warn(body)
		response.success = { resp, JSON ->
            log.warn(JSON)
            return JSON['tenant_access_token']
    	}
    	response.failure = { resp, JSON ->
            return JSON
        	log.warn("Failed : " + JSON)
    	}
	}
}
// 获取 jsapi_ticket
def feishu_jsapi_ticket(){
    def MyhttpBuilder = new HTTPBuilder("https://open.feishu.cn/open-apis/jssdk/ticket/get")
	MyhttpBuilder.request(Method.POST, ContentType.JSON) {
    	headers.'Authorization' = "Bearer " + feishu_app_token()
		response.success = { resp, JSON ->
            log.warn(JSON)
            return JSON['data']['ticket']
    	}
    	response.failure = { resp, JSON ->
            return JSON
        	log.warn("Failed : " + JSON)
    	}
	}
}

// 飞书jssdk鉴权
def jsapi_ticket = feishu_jsapi_ticket()
def signature_timestamp = new Date().getTime()
def signature_string = "jsapi_ticket=" + jsapi_ticket + "&noncestr=Y7a8KkqX041bsSwT&timestamp=" + signature_timestamp + '&url=http://jira.it.chehejia.com/browse/' + context.issue.key.toString()

MessageDigest sha1 = MessageDigest.getInstance("SHA1")
byte[] digest  = sha1.digest(signature_string.getBytes())
def signature = new  BigInteger(1, digest).toString(16)

import com.atlassian.jira.component.ComponentAccessor
def customFieldManager = ComponentAccessor.getCustomFieldManager()
// 文档链接
def cField_wdlj = customFieldManager.getCustomFieldObject("customfield_16511")

def jssdk_auth_script = '<div id="feishu_docs-mod-content"></div><script src="https://lf1-cdn-tos.bytegoofy.com/goofy/locl/lark/external_js_sdk/h5-js-sdk-1.1.3.js"></script><script type = "text/javascript">window.onload = function(){ window.webComponent.config({ signature: "' + signature + '", appId:"cli_a2de62f6ecf8d013", timestamp:' + signature_timestamp.toString() + ', nonceStr:"Y7a8KkqX041bsSwT", url:"http://domai.com/browse/' + context.issue.key.toString() + '", jsApiList:["DocsComponent"], lang: "zh-CN-中文" }).then(res=>{myComponent = window.webComponent.render("DocsComponent",{src: "'+ context.issue.getCustomFieldValue(cField_wdlj) +'",minHeight: "500px",width: "100%",height: "1200px",},document.querySelector("#feishu_docs-mod-content"), ) }) }  </script>'

writer.write(jssdk_auth_script)
~~~
