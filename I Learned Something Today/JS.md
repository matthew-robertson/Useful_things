## General Javascript
1. `debugger;` functions exactly like putting a breakpoint in the code.
1. There is no difference between `npm install` and `npm i`. It is there because JS people type `npm install` so much.
1. `NaN` always compares to false. Only `isNaN` can compare `NaN`.
1. `const` can't be used in normal for loops, because it can't be reassigned. It *can* however be used in a `for (const foo in bar)`
1. es2019 adds a "pipeline" operator `|>`, which lets you turn `b = bar(foo(a))` into `b = a |> foo |> bar`.
1. `0 === -0` will return true. If you explicitly want to check for negative 0, you should use `Object.is(x, -0)` instead. [Source](https://eslint.org/docs/rules/no-compare-neg-zero)
1. As of ES6, you can use variables as object keys like so `const key = 'asdf'; const bar = {[key]: 'foo'}`
1. You can destructure arrays in js like so `const [asdf, fdsa] = [1, 2]`. This is similar to object destructuring, but has existed longer.
1. the `arguments` keyword exists in functions, which is... cool, I guess? It doesn't exist in the scope of an arrow function.
1. `var` is function scoped, while `let` is block scoped.
1. `console.log` doesn't print the full object, and will evaluate it when expanded. This can lead one to tear their hair out when logging objects that change. Get around this by cloning the object before the initial print.
1. You can assign default parameters like `function foo(bar=10) {...}`, which is pretty tame. It gets nutty when you start using arguments as parameters to other default values like so: `function foo(bar, baz=test(bar)) {...}`.
1. Want to insert an asynchronous sleep, for use in tests or something? `const delay = ms => new Promise(res => setTimeout(res, ms)); await delay(2000)` has you covered.

## Typescript
1. In interface definitions, `?` can be used to mark optional parameters, like 
```
interface Person {
    name: string;
    age: number;
    phone?: string;
}
```
1. TypeScript only supports numeric and string-based enums. If you want an object, you need to emulate them with a class, e.g.:
```
export class PizzaSize {
  static readonly SMALL = new PizzaSize('SMALL', 'A small pizza')
  static readonly MEDIUM = new PizzaSize('MEDIUM', 'A medium pizza')

  private constructor(private readonly key: string, public readonly value: any) {}

  toString() {
    return this.key
  }
}
```

## Chai
1. `expect(y).to.be.closeTo(x, delta)` can be used to test the equality of floats.
1. `expect([1,2,3]).to.eql([1,2,3])` can be used to test deep equality between arrays

## React
1. React hooks are super cool, letting you write functional components instead of class-based ones.
1. `useEffect` in particular is used for handling both component lifecycle stuff like `componentDidMount`, `componentDidUpdate`, etc..
1. `useEffect`'s second argument is an array of things whose updating should trigger the effect to trigger again. No argument will run always, `[]` will only execute once (mimicing `componentDidMount`).
1. `useState` takes in an initial value, and will return an array containing the state variable and a method for updating it.
`let [currentIndex]. setCurrentIndex] = useState(0)`.
1. Hooks need to always be created, and in the same order. As a fun joke "You really love hooks. In fact, you love them unconditionally."
1. To do your cleanup stuff in hooks, like in `componentDidUnmount`, return a void function describing what it should do.
1. If can get annoying using a billion useState bits, so you may want to combine it into a single object. You can do that with the builtIn `useReducer`.
1. the `set` part of `useState`, like the `setState` in class-based components, executes on the next tick. This can cause timing/overwriting issues if you call it without batching updates.
1. The Container/view pattern is a pretty handy simple pattern, where you factor out views (which render the dom elements), and the containers (which deal wth side-effects and state and whatnot).
1. Higher-order components are functions that take at least one component as a param, and return a component, usually augmenting it. These components compose super well
1. The Render Props pattern is used almost like when composing partial functions, and lets you do stuff like HOC does. They don't compose well though:
```
export default () => {
  <DagobahRenderProp
    render={({ loading, error }) => {
      if (loading) {
        return <LoadingView />
      } else if (error) {
        return <Error />
      }
    }}
  />
}
```

## Ember

## Testing Ember?
1. When constructing components in tests, the elements you pass in have are found from the `this` scope. Local variables don't exist in its space, it's weird. You once spent comically long trying to track down a test that was failing because of this.
1. Tests like to do things by referencing classnames. A neat trick you can use is to pop a garbage classname on things just for testing purposes, like `js-search-box`.
1. A thing to note with this, at least in Capybara, when trying to find an element to click on `When I click on element '.js-search-box'` for example, a period has to be added to the front to ensure it's looking for an element with that classname. Without it, it'll look for a tag named that, like `When I click on element 'input'`.
1. `ember-template-lint` is a rad accessibility linter. Notably, they now have a todo functionality to help with introducing it to older code. Instead of ignoring errors, you can use `ember-template-lint . --update-todo` to create todos for them, which eventually degrade to warnings and then errors. `--include-todo` will list how many todos remain, and `--fix` will remove todos.
1. Adding your own lint rule to extend ESLint is pretty straightforward. Such an example is done in [this talk](https://youtu.be/nAIiXcisknc?t=559) in a few minutes.
1. `eslint-plugin-qunit` can apparently help with finding invalid test assertions and whatnot.
1. [Ember-asset-sizes-action](https://github.com/simplabs/ember-asset-size-action) helps compare asset sizes between master and the feature branch. This is useful for noting when you're about to blow up the bundle size.
