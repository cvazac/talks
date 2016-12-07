# Patterns and Anti-Patterns

## anti-patterns
### [jsx-no-bind](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

* Anti-pattern [(gist)](https://source.datanerd.us/gist/cvazac/b8d251cd17dffb7b9b75)
* Pattern [(gist)](https://source.datanerd.us/gist/cvazac/94226e23e3fd009c6654) [(babeljs.io)](https://babeljs.io/repl/#?evaluate=true&presets=es2015%2Creact%2Cstage-1%2Cstage-2&code=class%20View%20%7B%0A%20%20%0A%20%20render%20()%20%7B%0A%20%20%20%20return%20%3Cdiv%20onClick%3D%7Bthis.click%7D%20onDoubleClick%3D%7Bthis.doubleClick%7D%20%2F%3E%0A%20%20%7D%0A%20%20%0A%20%20click%20()%20%7B%0A%20%20%20%20console.info(this%20%3D%3D%3D%20null)%20%2F%2F%20true%0A%20%20%7D%0A%20%20doubleClick%20%3D%20()%20%3D%3E%20%7B%0A%20%20%20%20console.info(this%20%3D%3D%3D%20null)%20%2F%2F%20false%0A%20%20%7D%0A%20%20%0A%7D)

## [React Lifecycle Methods](https://facebook.github.io/react/docs/component-specs.html)
* [single component](./lifecycle/single component.md)
* [parent with two children](./lifecycle/parent with two children.md)
* [three generations](./lifecycle/three generations.md)
* Lessons:
    * all of the `componentDid<Mount | Update>()` hooks are bottom-up
    * `componentWillUnmount()` (and everything else) is top-down
    * when it comes to cross component communication, all components don't know about their `this.props` until the last parent's `componentDidUpdate()`

## [Container Components (pattern)](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0#.vb8tco6wl)
* init rev 
[poller.js](https://source.datanerd.us/gist/cvazac/01c70032f0df4f34638d)
[metrics.js](https://source.datanerd.us/gist/cvazac/001dc0fd44f7aed88d2e)

* more better
[poller.js](https://source.datanerd.us/gist/cvazac/f51ba7e89b9b8c263261)
[metrics.js](https://source.datanerd.us/gist/cvazac/2c70b886f8c5bff8634c)

```
<Parent>
  <Child>
    <GrandChild/>
  </Child>
</Parent>
```
* Un-nested components [(gist)](https://source.datanerd.us/gist/cvazac/2a3ccf6978119f57f06c)
* Nested components [(gist)](https://source.datanerd.us/gist/cvazac/4e40a202103b893239e0)
* Notes
    * benefits
        * don't have to pass props thru children that don't care about them
        * easier to refactor things later
        * parents can have dynamic children
    * gotchas
        * this.props.children refer to the children passed into you, not your own children [(link)](https://facebook.github.io/react/tips/children-undefined.html)
        * this.props.children is either an array or a singleton

## Gotchas
* [`key` attribute](https://source.datanerd.us/gist/cvazac/448e1ee71537ab42c6bb)
* [`componentWillReceiveProps (nextProps)`](https://facebook.github.io/react/blog/2016/01/08/A-implies-B-does-not-imply-B-implies-A.html)

## Etc.
* [`Reselect` for memoization](https://medium.com/@esamatti/react-js-pure-render-performance-anti-pattern-fb88c101332f#.aq3ltvv54)