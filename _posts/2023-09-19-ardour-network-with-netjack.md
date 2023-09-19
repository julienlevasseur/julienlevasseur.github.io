---
layout: post
title: Ardour Network With NetJack
description: 
tags:
  - Ardour
  - NetJack
image:
---

> Article under construction

Ever wanted to have your Linux DAW doint network audio like mixers does with Dante ?
Have you ever heard of NetJack ?

[Netjack](https://netjack.sourceforge.net/) is a Realtime Audio Transport over a generic IP Network, fully integrated into JACK.

![diagram_1](https://raw.githubusercontent.com/julienlevasseur/julienlevasseur.github.io/master/assets/images/posts/2023-09-19-ardour-network-with-netjack/diagram_1.png)


## Starting NetJack

### Server Side

```bash
jack_load netmanager
```

### Client Side

```bash
jackd -d net
```

### Jack Graphs

#### Server

![diagram_2](https://raw.githubusercontent.com/julienlevasseur/julienlevasseur.github.io/master/assets/images/posts/2023-09-19-ardour-network-with-netjack/diagram_2.png)

#### Client

![diagram_3](https://raw.githubusercontent.com/julienlevasseur/julienlevasseur.github.ioc/master/assets/images/posts/2023-09-19-ardour-network-with-netjack/diagram_3.png)