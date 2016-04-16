---
layout: post
title: "Implement MVC With TypeScript"
date: 2016-04-16 00:04:06
tags: Javascript Typescript
description: Implement MVC With TypeScript
---

Since the TypeScript Support inheritance, implementing MVC with javascript becomes easier!

Base classes:
{% highlight javascript %}
interface IEventDispatcher<T>{
  // maintain a list of listeners
    addEventListener(theEvent: T, theHandler:any);

  // remove a listener
    removeEventListener(theEvent:T, theHandler:any);

  // remove all listeners
    removeAllListeners(theEvent:T);

  // dispatch event to all listeners
    dispatchAll(theEvent: T,data?:any);

  // send event to a handler
    dispatchEvent(theEvent: T, theHandler:any,data?:any);
}

class Dispatcher implements IEventDispatcher<string> {
  private _eventHandlers = {};

  // maintain a list of listeners
  public addEventListener(theEvent:string, theHandler:any) {
    this._eventHandlers[theEvent] = this._eventHandlers[theEvent] || [];
    this._eventHandlers[theEvent].push(theHandler);
  }

  // remove a listener
  removeEventListener(theEvent: string, theHandler:any) {
    // TODO
  }

  // remove all listeners
  removeAllListeners(theEvent: string) {
    // TODO
  }

  // dispatch event to all listeners
  dispatchAll(theEvent:string,data?:any) {
    var theHandlers = this._eventHandlers[theEvent];
    if(theHandlers) {
      for(var i = 0; i < theHandlers.length; i += 1) {
        this.dispatchEvent(theEvent, theHandlers[i],data);
      }
    }
  }

  // send event to a handler
  dispatchEvent(theEvent: string, theHandler:any,data?:any) {
    theHandler(theEvent);
  }
}
{% endhighlight %}  

{% highlight javascript %}
    abstract class ModelBase implements IModel
    {
        static change:string="change";
        private dispatcher:Dispatcher;
        constructor(){
            this.dispatcher=new Dispatcher();
        }
        addListener=(eventHandler:any)=>{
            this.dispatcher.addEventListener(ModelBase.change,eventHandler);    
        }
        abstract getModel(): any;
        modelChange=()=>{
            this.dispatcher.dispatchAll(ModelBase.change);
        }
    }
    class ViewBase{
    protected Model:IModel;
    constructor(model:IModel){
        this.Model=model;
    }
}
{% endhighlight %}  


For the class To inherit them:

{% highlight javascript %}

class Icon  extends  ModelBase{
    open:boolean;
    toggle=()=>{
        this.open=!this.open;
    }
    getModel(){
       return {
         isOpen:this.open
       };
   }
}
class MenuView extends ViewBase{
    Menu:JQuery=$(".menu");
    constructor(icon: IModel) {
        super(icon);
        this.Model.addListener(this.open);
    }
    open = ()=> {
        // get model status
        if(this.Model.getModel().isOpen){
            this.Menu.fadeIn();
        }else{
            this.Menu.fadeOut();
        }
    }
 }
 var iconModel;
 $(document).ready(function(){
    iconModel=new IconModel();
    new  Icon(iconModel);
 })
{% endhighlight %}  