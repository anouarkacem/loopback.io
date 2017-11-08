---
lang: en
title: 'Server'
keywords: LoopBack 4.0, LoopBack 4
tags:
sidebar: lb4_sidebar
permalink: /doc/en/lb4/Server.html
summary:
---

## Overview

The [Server](https://apidocs.strongloop.com/@loopback%2fcore/#Server) interface defines the minimal required functions (start and stop) to implement for a LoopBack application. Servers in LoopBack 4 are used to represent implementations for inbound transports and/or protocols such as REST over http, gRPC over http2, graphQL over https, etc. They typically listen for requests on a specific port, handle them, and return appropriate responses. A single application can have multiple server instances listening on different ports and working with different protocols.


## Usage

LoopBack 4 currently offers the [`@loopback/rest`](https://github.com/strongloop/loopback-next/tree/master/packages/rest) package out of the box which provides an HTTP based server implementation handling requests over REST called `RestServer`. In order to use it in your application, all you need to do is register the `RestComponent` to your application class, and it will provide you with an instance of RestServer listening on port 3000. The following shows how to make use of it:

```ts
import {Application} from '@loopback/core';
import {RestComponent, RestServer} from '@loopback/rest';

export class HelloWorldApp extends Application {
  constructor() {
    super({
      components: [RestComponent],
    });
  }

  async start() {
    const rest = await this.getServer(RestServer);
    rest.handler((sequence, request, response) => {
      sequence.send(response, 'Hello World!');
    });
    await super.start();
    console.log(`REST server running on port: ${await rest.get('rest.port')}`);
  }
}
```
