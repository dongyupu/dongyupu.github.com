---
layout: post
title: "Jekyll loop images"
subtitle:   "loop images from site.static_files"
date:       2016-01-26 20:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---
load all images in  [static_file](http://jekyllrb.com/docs/variables/)

~~~bash
{:% for image in site.static_files %}
	{:% if image.path contains 'img/ARTMIA-curtain' %}
		{:% if image.extname == '.jpg' %}
			<a href="{{ site.baseurl }}{{ image.path }}" title="{{ site.baseurl }}{{ image.path | remove:'/img/ARTMIA-curtain/' | remove: '.jpg' }}">
			<div style="width:200px; height:365px; float:left; margin:3px 3px; display:inline; background-image:url({{ site.baseurl }}{{ image.path }}); background-repeat:no-repeat; background-size:auto;">{{ image.path | remove: '/img/ARTMIA-curtain/' | remove: '.jpg' }}</div>
			</a>
		{:% endif %}
	{:% endif %}
{:% endfor %}
~~~

if you have file list ,just use for loop

~~~bash
{:% for i in (1..98) %}
	<a href="{{ site.baseurl }}/img/ARTMIA-curtain/{{ i }}.jpg">
	<div style="width:200px; height:356px; float:left; margin:3px 3px; display:inline; background-image:url(/img/ARTMIA-curtain/{{ i }}.jpg);">{{ i }}</div>
	</a>
{:% endfor %}
~~~
