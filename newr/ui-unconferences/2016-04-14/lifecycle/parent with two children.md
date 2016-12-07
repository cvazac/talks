```
<Parent>
  <Child childIndex={0} />
  <Child childIndex={1} />
</Parent>
```

### startup
* componentWillMount - Parent Object {cav: 1}
* render - Parent Object {cav: 1}
* componentWillMount - Child Object {childIndex: 0, cav: 1}
* render - Child Object {childIndex: 0, cav: 1}
* componentWillMount - Child Object {childIndex: 1, cav: 1}
* render - Child Object {childIndex: 1, cav: 1}
* componentDidMount - Child Object {childIndex: 0, cav: 1}
* componentDidMount - Child Object {childIndex: 1, cav: 1}
* componentDidMount - Parent Object {cav: 1}

### update
* componentWillReceiveProps - Parent nextProps Object {cav: 1}
* shouldComponentUpdate - Parent nextProps Object {cav: 1}
* componentWillUpdate - Parent nextProps Object {cav: 1}
* render - Parent Object {cav: 1}
* componentWillReceiveProps - Child nextProps Object {childIndex: 0, cav: 1}
* shouldComponentUpdate - Child nextProps Object {childIndex: 0, cav: 1}
* componentWillUpdate - Child nextProps Object {childIndex: 0, cav: 1}
* render - Child Object {childIndex: 0, cav: 1}
* componentWillReceiveProps - Child nextProps Object {childIndex: 1, cav: 1}
* shouldComponentUpdate - Child nextProps Object {childIndex: 1, cav: 1}
* componentWillUpdate - Child nextProps Object {childIndex: 1, cav: 1}
* render - Child Object {childIndex: 1, cav: 1}
* componentDidUpdate - Child Object {childIndex: 0, cav: 1}
* componentDidUpdate - Child Object {childIndex: 1, cav: 1}
* componentDidUpdate - Parent Object {cav: 1}

### teardown
* componentWillUnmount - Parent Object {cav: 2}
* componentWillUnmount - Child Object {childIndex: 0, cav: 2}
* componentWillUnmount - Child Object {childIndex: 1, cav: 2}
