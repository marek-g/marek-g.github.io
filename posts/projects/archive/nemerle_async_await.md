---
layout: page
title: Nemerle Async Await (2012-07)
---

# Nemerle Async Await (2012-07)

## Introduction

In the first month of learning Nemerle language I wrote AsyncAwait macro which is an implementation of async/await keywords known from C#. There is a similar approach for asynchronous programming in Nemerle - [Computation Expression](https://github.com/rsdn/nemerle/wiki/Computation-Expression-macro) (which was a starting point for me), but it is not compatible with C#'s Task-based Asynchronous Pattern. You can use my solution to integrate with code written in C#.

You can find excellent introduction to the "Task-based Asynchronous Pattern" here:

http://www.microsoft.com/en-us/download/details.aspx?id=19957

Reading this document before using my macro is highly recommended.

## Features

- compatible with C# 5.0 Task-based Asynchronous Pattern
- syntax resembles that of C#
- .NET 4.5 and VS2012 are not required (works fine with .NET 4.0 and VS2010 with AsyncTargetingPack.NET4)
- support for the following language constructs: do / while / for / foreach / try / catch / finally / using
- support for GetAwaiter
- support for ConfigureAwait
- support for IProgress
- support for CancellationToken

## Usage

You need to:

- add Macro Reference to Nemerle.Async.Macros assembly
- add normal reference to Nemerle.Async assembly
- add Nemerle.Async using to your source code file

Syntax is very similar to that known from C#:

C# and Nemerle example

C#:

```csharp
public async Task<int> AsyncMethod(int a)
{
    int res = await AnotherMethod(a);
    return res + 10;
}
```

Nemerle:

```nemerle
public async AsyncMethod(a : int) : Task[int]
{
    def res = await AnotherMethod(a);
    res + 10;
}
```

You can also use constructions not possible with C#:

Async function

```csharp
def proc(cur, max)

{
    async
    {
        def res = await fib(10);
        textBox1.Text = $"fib(10) = $res\n";
    }
}
```

Async block defined inside method body

```nemerle
await async
{
    def res = await fib(10);
    textBox1.Text = $"fib(10) = $res\n";
}
```

## Exception Handling

What I don't like in TAP pattern is that's very easy to miss uncaught exception. The same applies to Nemerle's and C#'s implementation, so there is no difference here but it deserves a mention. Unhandled exceptions are rethrown outside Task encapsulation (and can cause application crash for example) only in the case of async handlers (async methods that return void):

Unhandled exception

```nemerle
public async OnButtonClick() : void
{
    throw Exception("Unhandled user exception!");
}
```

In every other situation exceptions are encapsulated inside Task's Exception property and won't rethrow it if not checked explicitly. For example, calling fallowing method won't crash your application:

It won't crash

```nemerle
public Test() : void
{
    _ = async
    {
        throw Exception("It won't crash!");
    }
}
```

Because of that I recommend always to use try / catch statements when calling asynchronous code:

Always use try/catch when calling asynchronous code

```nemerle
public async Test() : Task
{
    try
    {
        await async
        {
            throw Exception("This time it will be caught!");
        }
    }
    catch
    {
        | _ => .... exception caught ....
    }
}
```

## Deficiencies

In the current version there are few gaps. I don't know if I will ever fill them. The reason of this is they are minor for me:

- current solution is not optimized at all and uses closures, so it is slower than C#'s implementation and allocates more memory (rewriting to state machine is a tremendous work and I'm not considering it now), but if you will use it wisely, it should be no problem for you
- block statements are not supported
- you cannot use await as a parameter for a function/method call
- the output program has a dependency of "Nemerle.Async" assembly

## Downloads

You can download the latest version from [download page](https://sites.google.com/site/gibekm/downloads/nemerle/asyncawait). It is also part of snippets for Nemerle. You can download it with Nemerle source code.
