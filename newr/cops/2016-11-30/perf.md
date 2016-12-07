# React Perf

## react-perfometer

how fast is react?
you see all kinds of performance comparison diatribes online, contrived  

the answer is: fast enough
and the best way to keep react fast is to avoid shallow-comparison breaking anti-patterns

WHY IS THIS IMPORTANT:
animations, huge lists, mobile devices, responding to user input, scrolling

While it might not matter to your Component, it’s just a good pattern to follow, good habit to get into

define a PureComponent:
A PureComponent is a Component whose `render()` method return value is based only on props and state, not local members on `this`, or global variables. Essentially what you get is a Component whose `shouldComponentUpdate()` shallowly compares props and state, which is very performant.

But, you lose these performance benefits when you follow an anti-pattern in your Component’s `render()` method or the redux `mapStateToProps()`.

Because Javascript:
https://source.datanerd.us/gist/cvazac/74691541002eb87e7b6e#file-triple-equal-js

so i wrote a thing, to prove one pattern over another
https://source.datanerd.us/meatballs/react-perfometer
show examples, run examples, talk about results

## DataWrapper

Workarounds are easy for most cases, just move the prop value into a const. For the callback pattern, it’s a little more annoying. 

* problem: https://source.datanerd.us/gist/cvazac/ce74a5ed04ab6cd00ff3
* workaround: https://source.datanerd.us/gist/cvazac/a97e769c81bd057464d5
* more better: https://source.datanerd.us/gist/cvazac/a5e4ecde241eea0cf8c6

available in @datanerd/nr-ui-community as DataWrapper:
https://source.datanerd.us/commune/nr-ui-community/blob/master/src/components/DataWrapper/index.js

## eslint-plugin-react-perf

So, I spend a lot of my time in code reviews finding these patterns, and its really distracting. Keeps my focus away where it ought to be. So, I thought, if we agree on all of these patterns, really should be handled by lint.

So I wrote a thing:
eslint-plugin-react-perf
https://source.datanerd.us/meatballs/eslint-plugin-react-perf

I included it in meatballs-ui, hopefully it will go into @datanerd/newr one day. Found a few things, but also:
https://source.datanerd.us/gist/cvazac/930b605ce94099250392

I had a hunch that for some reason, this would NOT be a performance problem, like it would be if it were a custom capital letter ‘C’ Component.

So I tested it using the react-performeter:
DEMO

## Conclusions
1) to work around the callback bind() anti-pattern, use DataWrapper in @datanerd/nr-ui-community

2) to compare update cycle performance between any two or more patterns, use:
https://source.datanerd.us/meatballs/react-perfometer

3) to safe-guard against these patterns creeping into your render() / connect() methods use:
@datanerd/eslint-plugin-react-perf
