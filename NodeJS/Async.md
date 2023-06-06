https://www.digitalocean.com/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript#the-event-loop

**Synchronous:** Code that executes sequentially, line by line

**Asynchronous:** Code that doesn't execute sequentially, scenarios like network calls

# How Event Loop works?
JavaScript handles asynchronous code with the event loop.
Suppose, If no async WebApis used then all code runs sequentially.
But, If we use webapi's like setTimeout, we should run asyncrously otherwise It will block the complete flow (browser will be frozen)

Eventloop uses callstack(Stack) and MessageQueue/TaskQueue(Queue) to handle the execution order of code.

**Stack: LIFO (LastIn FirstOut)**
```
f1(){console.log(1)}
f2(){console.log(2)}
f3(){console.log(3)}
```
```
adds f1 to the stack, runs f1 and logs 1, removes f1 from stack
adds f2 to the stack, runs f2 and logs 2, removes f2 from stack
adds f3 to the stack, runs f3 and logs 3, removes f3 from stack 
```

**Queue: FIFO (FistIn FirstOut)**
Whenever the stack is empty, it checks the queue for waiting messages
```
f1(){console.log(1)}
f2(){ setTimeout(()=>console.log(2), 2000) }
f3(){console.log(3)}
```
1.adds f1 to the stack, runs f1 and logs 1, removes f1 from stack

2.adds f2 to the stack, runs f2 and 
    adds setTimeout to the stack, runs setTimeout webapi starts timer and adds anonymous function to queue, removes setTimeout from stack
removes f2 from stack

3.adds f3 to the stack, runs f3 and logs 3, removes f3 from stack 
4.event loop checks the queue for pending messages to process, and picks up the anonymous function from queue, adds to the stack and process it, removes from stack

Asyncronous behavior is handled by using Callbacks/Promises/AsyncAwait

# Callbacks:
callback takes function as a parameter and invokes the function once the work completes
```
const cb = () => console.log("I am CB");
const logger = (fn) => { 
    //do some work
    fn();
    }
logger(cb);
```
**callback hell/ pyramid of doom** : nested call backs results in code messy
```
setTimeout(()=>{
    setTimeout(()=>{
        setTimeout(()=>{

        }, 3000)
    }, 2000)
}, 1000);
```



# Promises: Promise is an object, that might return a value in the future.
```
const promise = new Promise((resolve, reject) => {})
__proto__: Promise
[[PromiseStatus]]: "pending"
[[PromiseValue]]: undefined
```
```
const promise = new Promise((resolve, reject) => {
  resolve('We did it!')
})
__proto__: Promise
[[PromiseStatus]]: "fulfilled"
[[PromiseValue]]: "We did it!"
```

Pending - Initial state before being resolved or rejected
Fulfilled - Successful operation, promise has resolved
Rejected - Failed operation, promise has rejected


**Consuming promise:** using then, catch and finally
then -> handles resolve
catch -> handles reject
finally-> runs after all the promises settled


**Promise Chaining**: using then, to call one after other promise
```
promise
  .then((firstResponse) => {
    return firstResponse + ' And chaining!'
  })
  .then((secondResponse) => {
    console.log(secondResponse)
  })
```
**Error Handling:**
In order to handle the error, you will use the catch instance method. This will give you a failure callback with the error as a parameter.
```
promise
  .then((response) => {
    console.log(response)
  })
  .catch((error) => {
    console.error(error)
  })
```


# Async/Await:
it handle asynchronous code in a manner that appears synchronous. async functions still use promises under the hood

```
const fn = () =>{
    const res = await function1();
    const res2 = await function2();
}
```
