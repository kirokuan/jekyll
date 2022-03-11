---
layout: post
title: "Coroutine usage of Caliburn.Micro"
date: 2022-03-01 00:00:00
tags: coroutine caliburn-micro
description: Some usage of coroutine implemented by Caliburn.Micro
---

Though the doc indicated that the `coroutine` exists for long before they have 4. But the only available doc seems is this [IResult and Coroutines](https://caliburnmicro.com/documentation/coroutines). While the doc seems not up to date, since `CoroutineExecutionContext` the signature is changed.

In short, the author implemented `IResult` this interface to support Coroutine.

{% highlight csharp %}

Page1 p1;
Page2 p2;
private IEnumerator<IResult> AppStart()
{
    yield return p1 = new Page1();
    // do whatevet you want when Page1 is finished correctly by user 
    yield return p2 = new Page2();
    // do whatevet you want when Page2 is finished correctly by user 
}

{% endhighlight %}
The following 2 are identical except when the exception happens.
{% highlight csharp %}

Coroutine.BeginExecute(AppStart());
// this can't catch exception.
{% endhighlight %}

{% highlight csharp %}
try
{
    await Coroutine.Execute(AppStart());
    //if the Context object is required, then..
    //await Coroutine.Execute(AppStart(), new CoroutineExecutionContext(){....});
} 
catch(TaskCancellationException ex)
{
    // if the process is interrupted.
}

{% endhighlight %}

And then `Page1` and `Page2` definitely are derived from `IResult`. So 

{% highlight csharp %}

class Page1 : IResult
{
    //context can be supplied from outside
    public void Execute(CoroutineExecutionContext context)
    {
        //do what ever you want
        //if you want to should in the page, something like...
        Screen.ActivateItemAsync(IoC.Get<Page1ViewModel>())
    }

    public event EventHandler<ResultCompletionEventArgs> Completed = delegate { };
    
    public void PageDismiss(bool isOk) 
    {
        Completed?.Invoke(this, new ResultCompletionEventArgs()
        {
            WasCancelled = !isOK, Error = null
        });
    }
}

{% endhighlight %}

When user click something on UI, PageDismiss will be triggered, once `Completed` this event is triggered, the function AppStart continues.

So when the `Completed` is called, the `Page1` is finished. While there are 2 cases here, one is    `isOk=true`, and the other is `isOK=false`. Where the former case, the process keeps going. When `WasCancelled` is true, the AppStart throws `TaskCancellationException` and the process is interrupted and Page2 is never executed. There is another field called `Error` which can indicates what kind of exception is thrown.

The implementation is quite useful when we want to maintain the sequence of ViewModel Display.

Conventional way is to use function triggered by the button, so if you get lot of Page1...10, then you need to put sequential bahavior in these pages function, it's something similar to callback hell. While with Coroutine, the function can be simplified as continue or not continue.