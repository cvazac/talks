# Forensic Tools

## 2.5 seconds between `componentWillUnmount` and `componentWillMount`
* situation I was seeing
* blamed react / redux magic
* debugger in `c'tor` or `componentWillMount`, `render`, `componentWillMount` wouldn't tell me anything
* tried to profile manually hitting the button in chrome
* found `console.profile(<name>)` and `console.profileEnd(<name>)` on Google, showed no activity
* instrumented `setTimeout()`, `setInterval()`, `requestAnimation()`, nothing
* instrumented `window.XMLHttpRequest`, found a hit
* revealed the pattern: 
```
render() {
    const {data} = this.props
    return data && <Component data={data} />
}
```

## other tips
* when did this value change, IIFE:
```
(function() {
    let value = obj[prop]
    Object.defineProperty = (obj, prop, descriptor) => {
        get: () => value, 
        set: (newValue) => {
            value = newValue
        }, 
    }
})()
```

* `performance.mark()` and `performance.measure()`

* [Lesser-Known JavaScript Debugging Techniques - good for debugging code on remote machines](http://amasad.me/2014/03/09/lesser-known-javascript-debugging-techniques/)

* online tools
    * [HTML to JSX Compiler](https://facebook.github.io/react/html-jsx.html)
    * [ES6 to Javascript convertor](https://babeljs.io/repl)
    * [JS / JSON beautifier](http://jsbeautifier.org/)
