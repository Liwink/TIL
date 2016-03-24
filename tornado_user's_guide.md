## Tornado User's Guide

#### Introduction

Tornado can be roughly divided into four major components:

* A **web framework** (including `RequestHandler` which is subclassed to create web supporting classes).
* Client- and server-side implementations of HTTP (`HTTPServer` and `AsyncHTTPClient).
* An asynchronous networking library (`IOLoop` and `IOStream`), which serve as the building blocks for the HTTP components and can also be used to implement other protocols.
* A coroutine library (tornado.gen) which allows asynchronous code to be written in a more straightforward way than chaining callbacks.

