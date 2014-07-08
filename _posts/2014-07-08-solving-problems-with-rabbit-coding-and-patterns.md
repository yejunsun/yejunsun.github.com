---
layout: post
title: "Solving problems with Rabbit: coding and patterns"
description: ""
category: "RabbitMQ in Action 学习笔记"
tags: []
---

## brief contents 前言

### This chapter covers 这一章主要涵盖
> - Designing applications toward messaging
> - Messaging patterns
> - Fire-and-forget moderns
> - RPC with RabbitMQ

### RabbitMQ 的几种应用场景

1. decoupling problems 应用于系统间的解耦

> 如何将程序中一段时间密集型的任务（或者说花费很多时间的一段代码）
> 移出系统外单独触发，以便能快速的响应其他的request
>
> 怎样将不同语言编写的系统整合到一起？
>
> 以上两种情况虽然属于不同的问题，但是都可以描述为： 如何请求和具体动作解耦
> decoupling the request from the actoin.
> 或者说 如果将请求和动作 异步进行

2. 将一个同步系统改写为一个异步系统

> 花大量的时间改写系统的所有代码，使其变为一个异步程序
> 或者使用Mq改写 少量代码

## 
