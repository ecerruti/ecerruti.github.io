---
layout: post
title: "HyperSQL Debugging"
date: 2013-08-01 09:57
comments: true
categories: [java debug]
---
This is one manner in which to debug HyperSQL, there are others, but this is probably the most straightforward. Since HyperSQL runs in memory you must be in debug mode with a breakpoint set in your application.

> The breakpoint **MUST BE** configured to just stop the current thread, otherwise the HyperSQL browser will lock up since all threads will be stopped.

* Set a breakpoint in your application - make sure it is right after where you want to examine data
* Bring up the code evaluation window for your IDE
   * Eclipse: Display Mode
   * IntelliJ: Evaluate Expression | Code Fragment Evaluation
* Paste in one of the following and evaluate (the Swing one has a nicer UI)
   * You can also paste one of these directly into your test class, e.g. in a before method, a test method, etc, but that may mess things up for others if you commit the code

```
org.hsqldb.util.DatabaseManagerSwing.main(new String[] {
    "--url",  "jdbc:hsqldb:mem:testdb", "--noexit"
});
```


```
org.hsqldb.util.DatabaseManager.main(new String[] {
    "--url",  "jdbc:hsqldb:mem:testdb", "--noexit"
});
```

As long as you are stopped at the breakpoint the DB browser will remain. You can continue to other breakpoints, but if you reach the end of the debug session the DB browser will exit.

One alternative is to run HyperSQL in server mode, you'll still need to be in debug mode with a breakpoint in your application, but the advantage is you can use your DB client of preference to attach to HyperSQL. The connection string would look something like this (adjust accordingly)

```
jdbc.url=jdbc:hsqldb:mem:testdb;sql.enforce_strict_size=true
```

