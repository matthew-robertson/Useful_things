## General Javascript
1. `debugger;` functions exactly like putting a breakpoint in the code.
1. There is no difference between `npm install` and `npm i`. It is there because JS people type `npm install` so much.
1. `NaN` always compares to false. Only `isNaN` can compare `NaN`.
1. `const` can't be used in normal for loops, because it can't be reassigned. It *can* however be used in a `for (const foo in bar)`
1. es2019 adds a "pipeline" operator `|>`, which lets you turn `b = bar(foo(a))` into `b = a |> foo |> bar`.
1. `0 === -0` will return true. If you explicitly want to check for negative 0, you should use `Object.is(x, -0)` instead. [Source](https://eslint.org/docs/rules/no-compare-neg-zero)
1. As of ES6, you can use variables as object keys like so `const key = 'asdf'; const bar = {[key]: 'foo'}`

## Typescript
1. In interface definitions, `?` can be used to mark optional parameters, like 
```
interface Person {
    name: string;
    age: number;
    phone?: string;
}
```

## Chai
1. `expect(y).to.be.closeTo(x, delta)` can be used to test the equality of floats.
1. `expect([1,2,3]).to.eql([1,2,3])` can be used to test deep equality between arrays
