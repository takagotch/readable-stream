### readable-stream
---
https://github.com/nodejs/readable-stream

```
npm install --save readable-stream
```

```js
const {
  Readable,
  Writable,
  Transform,
  Duplex,
  pipeline,
  finished
} = require('readable-stream')
```

```js
const stream = require('stream');

const http = require('http');
const server = http.createServer((req, res) => {
  let body = '';
  req.setEncoding('utf-8');
  req.on('data', (chunk) => {
    body += chunk;
  });
  req.on('end', () => {
    try {
      const data = JSON.parse(body);
      res.write(typeof data);
      res.end();
    } catch(er){
      res.statusCode = 400;
      return res.end(`error: ${er.message}`);
    }
  });
});
server.listen(1337);

const myStream = getWriteableStreamSomehow();
myStream.write('dome data');
myStream.write('some more data');
myStream.end('done writing data');

function writeOneMillionTimes(writer, data, encoding, callback){
  let i = 100000000;
  write();
  function write(){
    let ok = true;
    do {
      i--;
      if(i === 0){
        writer.write(data, encoding, callback);
      } else {
        ok = writer.write(data, encoding);
      }
    } while(i > 0 && ok);
    if(i > 0){
      writer.once('drain', write);
    }
  }
}

const writer = getWritableStreamSomehow();
for(let i = 0; i < 100; i++){
  writer.write(`hello, #${i}!\n`);
}
writer.end('This is the end\n');
writer.on('finish', () => {
  console.log('All writes are now complete.');
});

const writer = getWritableStreamSomehow();
const reader = getReadableStreamSomehow();
writer.on('pipe', (src) => {
  console.log('Something is piping into the writer.');
  assert.equal(src, reader);
});
reader.pipe(writer);

const writer = getWritableStreamSomehow();
const reader = getReadableStreamSomehow();
writer.on('unpipe', (src) => {
  console.log('something has stopped piping into the writer.');
  assert.equal(src, reader);
});
reader.pipe(writer);
reader.unpipe(writer);

const fs = require('fs');
const file = fs.createWriteStream('example.txt');
file.write('hello, ');
file.end('world!');

stream.cork();
stream.write('some ');
stream.write('data ');
process.nextTick(() => stream.uncork());

stream.cork();
stream.write('some ');
stream.cork();
stream.write('data ');
process.nextTick(() => {
  stream.uncork();
  stream.uncork();
});

function write(data, cb){
  if(!stream.write(data)){
    stream.once('drain', cb);
  } else {
    process.nextTick(cb);
  }
}
write('hello', () => {
  console.log('Write completed, do more writes now.');
});

const { PassThrough, Writable } = require('stream');
const pass = new PassThrough();
const writable = new Writable();
pass.pipe(writable);
pass.unpipe(writable);
pass.on('data', (chunk) => { console.log(chunk.toString()); });
pass.write('ok');
pass.resume();

```


