---
layout: post
title: "docker-compose-up-permission-error-fix"
subtitle:   "docker-compose-up-permission-error-fix"
date:       2023-7-14 16:45:00 +0800 +0800
author:     "Dongyupu"
header-img: "img/post-bg-02.jpg"
---


chown -R 2001:2001 atlassian-agent.jar

docker-compose
~~~
version: '2'
networks:
  bridge:
    driver: bridge
services:
  jira:
    image: "atlassian/jira-software:latest"
    container_name: jira-software-server
    restart: always
    ports:
      - "80:8080"
    volumes:
      - /Users/dongyupu/work/docker/jira/data:/var/atlassian
      - /Users/dongyupu/work/docker/jira/data/logs:/opt/atlassian/jira/logs
      - /etc/localtime:/etc/localtime:ro
      - /etc/timezone:/etc/timezone
    networks:
      - bridge
    environment:
      - JAVA_OPTS=-Duser.timezone=Asia/Shanghai -javaagent:/var/atlassian/atlassian-agent.jar
      - JVM_MINIMUM_MEMORY=4096m
      - JVM_MAXIMUM_MEMORY=7168m
~~~
