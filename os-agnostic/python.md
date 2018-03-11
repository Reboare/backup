# Python

## python2 input

In python 2, the `input` function worked in an interesting manner.  It executed the input given to it directly, allowing us to, for example, have an integer returned directly from the result of the function.  However, this also allowed any arbitrary code to be injected in poorly coded applications.

```
__import__('os').system('/bin/bash')
```

In python 3 this was removed, and `raw_input` from python 2 replaced it.

## Module Overwrite

```
import pandas
```

## Pickle Deserialization



