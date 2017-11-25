##### isNativeFunction #1:
```javascript
const isNativeFunction = (function() {
  return function(fn) {
    return typeof fn === 'function' &&
      /\[native code\]/.test(String(fn))
  }
})()
```

Looks promising: 
```javascript
isNativeFunction(XMLHttpRequest) // true
window.XMLHttpRequest = function() { /* ... */ }
isNativeFunction(XMLHttpRequest) // false
```

But it can be defeated:

```javascript
window.XMLHttpRequest.toString = function() {
  return '[native code]'
}
isNativeFunction(XMLHttpRequest) // **true** (false positive)
```

Conclusion, don't trust `toString` on the function under test. 

##### isNativeFunction #2:

Is `Function.prototype.toString` better?.

```javascript
const isNativeFunction = (function() {
  return function(fn) {
    return typeof fn === 'function' &&
      /\[native code\]/.test(Function.prototype.toString.call(fn))
  }
})()
```

Gets us closer: 
```javascript
isNativeFunction(XMLHttpRequest) // true
window.XMLHttpRequest = function() { /* ... */ }
window.XMLHttpRequest.toString = function() {
  return '[native code]'
}
isNativeFunction(XMLHttpRequest) // false
```

But it, too, can be defeated:

```javascript
Function.prototype.toString = function() {
  return '[native code]'
}
isNativeFunction(XMLHttpRequest) // **true** (false positive)
```

Conclusion, we can't trust `Function.prototype.toString`, either.
 
If we _could_ trust `Function.prototype.toString`, we would be done. Let's see if we can figure out if it is patched. More often than not, `Function.prototype.toString` will be the native version. If it's not, maybe we can figure something out later. 

Our patching of `Function.prototype.toString` is pretty naive because:
```javascript
(function(){}).toString()
// returns '[native code]'
```

And we know that that's not right. So, that tells us that we are going to need some custom logic in there. Let's try to use the original native to our advantage:
```javascript
Function.prototype.toString = (function(toString) {
  return function() {
    return toString.apply(this, arguments)
  }
})(Function.prototype.toString)
```

```javascript
(function(){}).toString()
// returns 'function (){}'
```

But this reveals our custom logic:
```javascript
Function.prototype.toString.call(Function.prototype.toString)
// return 'function () { return toString.apply(this, arguments) }'
```

Outside of fat arrow functions (which won't work in this case because `this`), there's no way I know of to return a value from a function without the `return` keyword, let's use that to our advantage:
```javascript
  function nativeFunctionPrototypeToString() {
    var toString = Function.prototype.toString
    
    // serialize it, using only itself
    var serialized = toString.call(toString)

    if ((function(){}).toString() === serialized) {
      // if these two are the same, we've been patched
      return false
    }
    
    if (/return/.test(serialized)) {
      // if the serialized version of `Function.prototype.toString` contains 'return', we've been patched
      return false
    }

    // otherwise, it's native 
    return true
  }
```

If `Function.prototype.toString` has been patched, we will need to grab a 'clean' version from a new browsing context. 
```javascript
var iframe = document.createElement('iframe')
iframe.style.display = 'none'
iframe.src = 'javascript:false'
document.getElementsByTagName('script')[0].parentNode.appendChild(iframe)

iframe.contentWindow.Function.prototype.toString // <-- let's use this
iframe.parentNode.removeChild(iframe)
```
##### isNativeFunction #3:
```javascript
const isNativeFunction = (function() {
  var nativeToString
  function nativeFunctionPrototypeToString() {
    var toString = Function.prototype.toString
    var serialized = toString.call(toString)
    return (function(){}).toString() !== serialized &&
      /return/.test(serialized)
  }

  return function(fn) {
    if (!nativeToString) {
      if (nativeFunctionPrototypeToString()) {
        nativeToString = Function.prototype.toString
      } else {
        var iframe = document.createElement('iframe')
        iframe.style.display = 'none'
        iframe.src = 'javascript:false'
        document.getElementsByTagName('script')[0].parentNode.appendChild(iframe)

        nativeToString = iframe.contentWindow.Function.prototype.toString
        iframe.parentNode.removeChild(iframe)
      }
    }

    return typeof fn === 'function' &&
      /\[native code\]/.test(nativeToString.call(fn))
  }
})()
```

The clever reader will notice that we are using `.call()` in two spots - to avoid `toString` interlopers on the argument we pass to `.call()`. Because `Function.prototype.call` itself can be patched, and it can not be polyfilled with native javascript, we have to work around it by deleting and restoring any `toString` methods that sit in our way. We will leave that as an exercise for the reader. 

We are also using `RegExp.prototype.test`. But what lunatic would override that black magic?
