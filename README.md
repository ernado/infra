# Infra

Infrastructure for programs, with focus on go language.

## Goals

This is WIP attempt to organize knowledge about development of applications
with some key features, that seems to sound generic while being extremely
important.

### Observability

This property is about being able to understand what is happening with app.

#### Logs

Historically log is just a string message, optionally having some arbitrary
context.

In reality this is not so useful, we can have thousands of requests per second
that generate log messages concurrently across multiple services and it is 
impossible to distinguish messages related to one request from another.

Log is not a string, it is an *event*. Logbooks, the physical version of logs
are designed to aid in reconstruction of incident. It is your *flight recorder*.
Imagine that all planes flying right now will simultaneously write their 
telemetry to one big flight recorder, without any way to identify one exact
aircraft. Not so useful?

So, log entry should have a context. Like *request id*, *user id*, name it.
You log system should be able to answer those questions:
* What happen while processing this request?
* What is happening with specific service right now?
* What was the order of requests made by that *user id* from that *session* lead to failure?

With advanced system, those questions can be answered too:
* How much users are affected with that error?
* How much resources were spent to process that specific request?

The last question is more about tracing, but anyway, tracing and logging are tightly
connected.

Just for example, log message can have following structure:
```json
{
  "time": "2018-06-12T23:44:01.123045Z0",
  "request_id": "jbq3145Hz",
  "session_id": "4412-13-01",
  "user_id": "1004134",
  "app": "cats",
  "service": "upload",
  "msg": "successful upload",
  "severity": "debug",
  "meta": {
    "size": 10024
  }
}
```

Apart the common fields, there is *meta* that can have arbitrary structure, but
can aid in debugging/statistics.


*Work in progress*