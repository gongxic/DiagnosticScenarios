主要参考的是这个[文章](https://www.cnblogs.com/wu_u/p/14109333.html)的内容
比官网多了docker 相关的内容。

都是哪些命令：

docker exec -it diagnostic bash

find / -name createdump

/usr/share/dotnet/shared/Microsoft.NETCore.App/3.1.10/createdump 1 （如果容器内只有一个应用，一般PID默认为1，也可以使用top命令来查看PID）

docker cp 6b6f0a7ebe10:/tmp/coredump.1 /mnt/dumpfile/coredump.1 (window /dumfile/coredump.1 会在C盘 dumpfile 文件夹)

clrstack （调用堆栈信息）

syncblk （分析死锁）

Owning Thread Info列下面有3个子列，第一列是地址，第二列是操作系统线程 ID，第三列是线程索引

setthread 线程索引

 分析内存泄漏

dumpheap -stat   -min [byte]可选参数

dumpheap -mt 00007f61876a0f90

gcroot -all 00007fcfabc378d8   

---
languages:
- csharp
products:
- dotnet-core
page_type: sample
name: "Diagnostic scenarios sample debug target"
urlFragment: "diagnostic-scenarios"
description: "A .NET Core sample with methods that trigger undesirable behaviors to diagnose."
---
# Diagnostic scenarios sample debug target

The sample debug target is a simple `webapi` application. The sample triggers undesirable behaviors for the [.NET Core diagnostics tutorials](https://docs.microsoft.com/dotnet/core/diagnostics/index#net-core-diagnostics-tutorials) to diagnose.

## Download the source

To get the code locally on your machine, click on '<> Code' in the top left corner of this page. This will take you to the root of the repo. Once at the root, clone the samples repo onto your local machine and navigate to samples/core/diagnostics/DiagnosticScenarios/.

## Build and run the target

After downloading the source, you can easily run the webapi using:

```dotnetcli
dotnet build
dotnet run
```

## Target methods

The target triggers undesirable behaviors when hitting specific URLs.

### Deadlock

```http
http://localhost:5000/api/diagscenario/deadlock
```

This method will cause the target to hang and accumulate many threads.

### High CPU usage

```http
http://localhost:5000/api/diagscenario/highcpu/{milliseconds}
```

The method will cause to target to heavily use the CPU for a duration specified by {milliseconds}.

### Memory leak

```http
http://localhost:5000/api/diagscenario/memleak/{kb}
```

This method will cause the target to leak memory (amount specified by {kb}).

### Memory usage spike

```http
http://localhost:5000/api/diagscenario/memspike/{seconds}
```

This method will cause intermittent memory spikes over the specified number of seconds. Memory will go from base line to spike and back to baseline several times.
