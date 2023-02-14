---
title: "NodeJS is broken"
date: 2023-02-13T08:06:25+06:00
description: NodeJS is broken
hero: images/vault.png
menu:
  sidebar:
    name: NodeJS is broken
    identifier: nodejs
    parent: technical
    weight: 10
tags: ["Programming", "Multi-langues"]
categories: ["Tech"]

---

I'm not here to only critisize NodeJS. In fact it is the framework we use at my work, and in a LOT of companies.
It has served us well over the years in the browser, and after that, it has continued to help us on the server. Given the widespread knowledge of javascript, it's understandable that many people also reach for it when implementing their backends.

## CPU
JavaScript's main disadvantage on the server is its single-threaded nature, which makes it difficult to fully utilize all the cores available. Developers must manually distribute the work to enable more cores, which is challenging and can result in inefficiencies compared to other platforms. The most common approach is to create multiple application instances, one per core, to take advantage of available processing power. However, developers using Rust, Go, or .NET do not have to worry about this issue, as their selected async runtime will handle this for them.

## Memory

Another flaw that might hit you if you are not aware of it is that V8 (the javascript runtime that powers node) is limited to a heap size of 2Gb/4Gb (differs between versions). It doesn't matter how much ram you have in your machine, if you reach this heap size usage, your application will crash, which could be devastating if the crash is due to high usage and not a memory leak. The way to counter this is to set the max-old-space-size setting on your node process to a limit that you deem correct for your server.

Developers working in Rust, Go or .net don't have to worry about this, as there is no hidden application crashing memory limit.

## Performance

Thanks to the competition over the years to get the fastest browser, we have gotten to a point where the performance of javascript can be pretty incredible. With that said, it's not guaranteed to be running at its peak performance. You see, javascript has two problems, first, it needs to have a fast “start from source”-time, then it needs to be able to run the actual application logic fast. This is solved by having a multi-step compilation process, where the first pass just gets the code up and running without spending much time on optimizations. After the application has been running for a while, V8 knows where its worth spending time applying optimizations and it will put its optimizing compiler on the task to do the optimizations.

Usually, this is fine, after a while when your node process has started up, it gets optimized and servers have historically been long-running processes. In the current days of serverless though, things are different. Sometimes code starts up to only do one pass through the code, this is not optimal for the V8 engine as it will never get a chance to apply its optimizations. With that said, it would be even worse if it did do its optimizations, as that would mean it spent lots of CPU cycles to optimize something that will be thrown away only milliseconds later.

A Rust developer doesn't have to worry about this, as the optimization happens at compile time, and when the application starts it runs at peak performance from the getgo.

## What should we do then ?

Shifting away from Node.js to Rust for server-side code can bring benefits such as improved performance, memory safety, and better concurrency handling. However, making the transition requires careful consideration of the project's requirements, available resources, and the developer team's experience. A gradual migration approach may be more feasible, starting with small components and gradually moving to more complex features. Alternatively, some components can be written in Rust as a separate service and integrated with the existing Node.js application. Adequate training and documentation should also be provided to ensure a smooth transition for the development team. Ultimately, the decision to switch should be based on the project's needs, available resources, and long-term goals.
