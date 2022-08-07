### Observable ###
We probably already know **Promise** , it was born to solve **callback hell** , 
designed to *handle 1 task*

>Observable(OS) was born to **handle many tasks**, OS is unsupported object (Promises are supported in JS), the user will have to *define* or *use
related libraries*

>All models in the form of **Producer** managing and sending data and events to **Consumer** can be applied with **Observable Pattern**.
To standardize the Producer - Consumer models with the same API and the same processing mechanism
Compensating for the missing of those models in JS
Not only **asynchronous** processing, but also **synchronous** processing

In youtube: 
- **Producer** look like content creator
- **Consumer** look like user
  
example with code:

```js
function next(){ //consumer
	//todo...
};
setTimeout(next, 1000); // producer
```

```js
function previous(){//consumer : subscribe data from event
	//todo...
}
document.addEeventListener('mousemove',previous) //producer: create this event which can be considered as a browser , javascript DOM
```

**Compare state between Promise and Observable**

|          | Promise(Eager) | Observable(Lazy) |
| -------- | :-----: | :--------: |
| SUCCESS  | &#9745; |  &#9745;   |
| ERROR    | &#9745; |  &#9745;   |
| COMPLETE | &#9744; |  &#9745;   |


All models with **subscribe** and **unsubscribe**(Promise not have) can be applied to **Observable**

exp : 
- setTimeout/Interval - clearTimeout/Interval 
- document.addEventListener - document.removeEventListener...

*in Promise(Eager)*
```js
const promiseObj = Promise.timeout(1000) // activated timeout
//timeout is same setTimeout but we can custom milisecond  
```

*get data*

```js
promiseObj
.then ((data)=>{
	console.log('promise.timeout data',data);
});
```
>if you change setTimeout to setInterval, Promise will only display 1 line console.log

*in Observable(Lazy)*

```js
const obsTimeout = Observable.interval(1000)// not active timeout
//subscribe
obsTimeout
.subscribe((data)=>{
	console.log('obsTimeout.timeout data',data)
})
```

>different to Promise # Observable will display more line console.log

**Below we will learn about lazy in Observable** 

```js
function Observable(funcWaitToRun) {
	this.subcribe = funcWaitToRun;
}

Observable.timeout = function(miliseconds){
	function timeoutWaitToRun(next){
		setTimeout(()=>{
			next();
	},miliseconds);
}
	return new Observable(timeoutWaitToRun)
}

Observable.interval = function(miliseconds){
	function intervalWaitToRun(next){
		setInterval(()=>{
			next();
	},miliseconds);
}
	return new Observable(intervalWaitToRun)
}
```

There are 2 object: *timeout* and *interval* hold 2 function **timeoutWaitToRun** and **intervalWaitToRun** but the same API is **subscribe** look at line 2 in function **Observable**

```js
const obsTimeout$ = Observable.timeout(1000);
const obsInterval$ = Observable.interval(1000);
console.log(obsTimeout$);
console.log(obsInterval$);	
```

Both have subscribe . subscribe of **obsTimeout** has function **timeoutWaitToRun** and **obsInterval$** has **intervalWaitToRun** but different with **Promise** , *interval* in **Observable** can run so much and **Promise** can't do (Promise only 1) .


#### Subscription và unsubscribe ####

>When we proceed to **subscribe** to an OS , according to its rules, it will *return* 1 object which is **Subscription**
**Subscription** has a method that is **unsubscribe**, if *setTimeout* is used, it clearTimeout and interval is clearInterval

Same with Observable , we must create Subscription

```js
function Subscription(unsubscribe){
	this.unsubscribe = unsubscribe
}

Observable.timeout = function(miliseconds){
    function timeoutWaitToRun(next){
    const timeoutID = setTimeout(()=>{
        next();
    },miliseconds);
    return new Subscription(() => {
	    clearTimeout(timeoutID)}) 
    }
    return new Observable(timeoutWaitToRun)
}
```

example with **setTimeout**

```js
const obsTimeout$ = Observable.timeout(5000)
const subs = obsTimeout$
	.subscribe(() => {
		console.log('timeout...');
})
```

You can run **subs** and wait for 5s, you can see console.log but you type **subs.unsubscribe** in order to **clearTimeout** and console.log will never display.

**Observer**
>Collection of connect , which helps us to *listen for values* ​​sent from **Observable** (next, **complete**, **error**)

* **complete**: when setTimeout run only one and complete (Interval never complete);
> **complete** different from clearTimeout,clearInterval
* **error**: normally appear when **calling API**

