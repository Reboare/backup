# PHP

PHP is an interesting language with a number of quirks.  Since it was designed for simple personal websites and not as a full blown enterprise grade language, this comes as no shock.  Unfortunately, this means older version are easily exposed to a number of obvious exploits.

## strcmp

## Type Juggling

With weak-comparisons in PHP, such as in the case of the `==` operator:

| Hash | Magic String |  |
| :--- | :--- | :--- |
| MD5 | 240610708 | 0e462097431906509019562988736854 |
| SHA1 | 10932435112 | 0e07766915004133176347055865026311692244 |

**Source**: [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/PHP%20juggling%20type](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/PHP juggling type)

## Regular Expressions

The [madirish guide](http://www.madirish.net/402), and [bitquark guide](https://bitquark.co.uk/blog/2013/07/23/the_unexpected_dangers_of_preg_replace) explains this fairly well, as well as going over various filter evasions.  The issue stems from the /e regular expression modifier, which can allow you to execute arbitrary code.

For example in `preg_replace`:

```
preg_replace('/(.*)/', 'A', 'B');
```

If we change the regular expression however as below we can achieve arbitrary command execution:

```
preg_replace('/(.*)/e', 'phpinfo()', 'B');
```

The `/e` modifier causes the result of the expression to be evaluated as PHP code, effectively giving you arbitrary command execution.



