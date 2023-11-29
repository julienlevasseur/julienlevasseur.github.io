---
layout: post
title: API Architectures
description: A presentation API Architectures
tags:
  - API
  - Architecture
  - SOAP
  - REST
  - GraphQL
  - gRPC
  - WebSocket
  - Webhook
---

# SOAP

Simple Object Access Protocol (SOAP) is a message specification for exchanging information between systems and applications.
SOAP is in the field of a long time (1998) and is mostly used (but not limited to) in financial services.

The exchange format for SOAP is XML, which makes it verbose and heavy to implement.

![soap](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/soap.png)

# REST

REST is an acronym for REpresentational State Transfer and an architectural style for distributed hypermedia systems.
Presented in 2000, REST is one of the most common approaches for building web based APIs.

Relying on HTTP methods to interact with resources, REST uses JSON as data exchange format.

REST is not a protocol or a standard, it is an architectural style. There's multiple ways of implementing REST APIs.

![rest](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/rest.png)

# GraphQL

GraphQL is not only an architectural style, but also a query language.
GraphQL enables declarative data fetching where a client can specify exactly what data it needs from an API.
Created in 2012 it was released as Open Source in 2015.

GraphQL supports reading, writing (mutating), and subscribing to changes to data (realtime updates – commonly implemented using WebSockets).

![graphql](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/graphql.png)

# gRPC

gRPC (gRPC Remote Procedure Calls) is a cross-platform open source high performance remote procedure call (RPC) framework.
Released in Open Source in 2015 (from the RPC Stubby infrastructure created in 2001).

gRPC uses Protocol Buffers to encode data. Protocol buffers provide a serialization format and an Interface Definition Language.
One of the strength of gRPC + Protobuf is the enforced API interface definition via `.proto` files. Of course, the binary serialization of data makes it also very fast.

![grpc](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/grpc.png)

# WebSocket

WebSocket is a two communication protocol standardized by the IETF in 2011.
This is a perfect solution for real time applications (chat, gaming).

The WebSocket protocol specification defines ws (WebSocket) and wss (WebSocket Secure) as two new uniform resource identifier (URI) schemes that are used for unencrypted and encrypted connections respectively.

![websocket](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/websocket.png)

# Webhook

Webhook is an asynchronous event driven HTTP callback architecture.
It relies on HTTP and JSON to trigger events.
It is particularly useful to replace polling: rather than having a client asking a service the status of a task every X minutes/seconds, the service will notify via webhook the client when the `task successful` event is triggered.

![webhook](https://github.com/julienlevasseur/julienlevasseur.github.io/blob/master/assets/images/posts/2022_05_10-api-architectures/webhook.png)