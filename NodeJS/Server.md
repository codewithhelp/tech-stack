## NodeJS server - with default modules
```
const http = require('http')

http.createServer((req,res)=>{
    // res.writeHead(200, { "Content-Type" : "application/json"})
    // res.end(JSON.stringify({name: "bala"}))
    res.end("hello");
}).listen(3001);
```


## Using ExpressJS to create server
```
const express = require("express");
const app = express();
app.listen(3000);
app.get("/", (req, res) => {
  res.send("Hello world..");
});
```