# node.js

node is a technology allowing server based apps to be written in Javascript.  Why?  I get why...but why?

In all seriousness it's pretty cool, but requires a different approach than the standard php based web applications that are so common.

## Serialize/Unserialize

With node, and in all honesty any serialization framework, there can be issues if the application accepts un-trusted input.  The below links cover this form of vulnerability fairly well:

[https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/)[https://blog.websecurify.com/2017/02/hacking-node-serialize.html](https://blog.websecurify.com/2017/02/hacking-node-serialize.html)

