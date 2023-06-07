# Event Emitter

Generally in NodeJS, when we want to trigger an action on completion of other action, we use asynchronous programming like callback/promise/async_await. But these provides tight coupling at the code.

**Event Emitters provides pub-sub relationship to handle such usecases with low coupling.** Means, publisher publishes the message, and consumers consumes it.

```
events - module that provides EventEmitter

emit - producer uses to produce messages, emit('event_name', params)
        return true for if listener available
        return false for if no listeners available

on - consumer uses to consume
    on('event_name', params => {})

once - consumer uses to consume only once, and removes the listner
    once('event_name', params => {})

off - consumer uses to remove listner
    off('event_name')

listenerCount -  check the cound of number of listners
    listenerCount('event_name')

removeAllListeners - removes all the listners
    removeAllListeners('event_name')
```


```
const EventEmitter = require('events')
class TicketManager extends EventEmitter{
  constructor(supply){
    super()
    this.supply = supply;
  }
  buy(email, price){
    if(this.supply>0){
      this.supply--;
      this.emit("buy",email, price, Date.now());
    }else{
      this.emit("error", new Error("Failed to supply..."))
    }
  }
}


const ticketManager = new TicketManager(10);
ticketManager.on("buy", (...payload)=>{
  console.log(payload, 'payload')
  // save db
  // send email
  // print copy
});
ticketManager.on("error", (...payload)=>{
  console.log(payload, 'error')
});

ticketManager.buy("bala@gmail.com", 1)
ticketManager.buy("satish@gmail.com", 1)


ticketManager.listenerCount("buy") // to get the number of listeners to that event

ticketManager.off("buy", ()=>{
  // remove the listener
}); 

ticketManager.removeAllListeners("buy")
```