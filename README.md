# What?
`Clojure` is a general-purpose, functional programming language, that runs on `JVM` (Java Virtual Machine). It is a `Lisp` dialect. So by learning `Clojure`, you automatically learn `Lisp`. It is a compiled language and the produced output is `JVM` bytecode.

`Clojure` offers a `REPL` driven development, which enables the developer to interact with the running application without restarting / reloading it (i.e. change state or evaluate new functionality).

# Why?
First of all, you might be wondering what does `Clojure` have to do with Front End, right? Well, there's also `ClojureScript`. `ClojureScript` is a compiler that targets `JavaScript`. So we can write `Clojure` code and have it compiled down to `JavaScript`.

So on this page, every time I mention `Clojure`, I mean `Clojure` through `ClojureScript` (since we are Front Enders).

And now the question - why would we do it? And a bit broader question would be - why do we use compilers in general? It's another tool / language we have to learn after all. And the reason why we use them is that they tend to have solutions for many different issues around `JavaScript` and they also offer us more readable, maintainable and testable way of writing code.

*some healthy ranting*

Let's face it - `JavaScript` ecosystem is a total chaos. Configuring and scaffolding your application can be a pain sometimes. A typical `package.json` file includes plugins that have nothing to do with the application I'm trying to build. There's usually at least 6 lines starting with `babel-` and another 6 - 7 lines of `eslint-`, not to mention `enzyme-` or `jest-`, in a typical `package.json`. And then there's a config file for `jest`, `webpack`, `babel`, `polyfill`, `shim` and etc. So I end up creating these random config files to actually get to the point where I can start working on my application. And I'm not even going to mention the version incompatibilities in some cases where one plugin doesn't like another.

Anyway, the point is that this should be a simple task (ideally a one-liner).

There's also a ton of functionality missing from vanilla `JavaScript` that we try to add by including libraries like `ramda` or `lodash` or even something as simple as `uuid generator`. 

Also, if we're not careful, it's super easy to mutate the state, which just adds more time to development when trying to find the cause.

And what about optimisations? How much time do we spend thinking about unnecessary iterations or caching the result of a function that will always return the same value for a specific argument?

*end of some healthy ranting*

Well, `Clojure` has it all. Typical `Clojure` config (`project.clj`) is all about the project dependencies and very specific to what and how we are trying to build, without unnecessary boilerplates. Everything happens in a single, readable, `json-like` file, including how to compile an application in various envs like `dev` and `production`.

Because `Clojure` comes with laziness by default, it is super easy to avoid unnecessary iterations.
Because `Clojure` comes with immutability by default, we don't mutate the state, which also makes is possible to cache / memoize the results of a function call with a built-in `memoize` function easily.

It also offers us the way to test the code we write straight within an IDE / text editor, without writing a test for it yet or refreshing the browser for that matter.

It offers us a way to interact with our application without touching the code. For example, we change the state within the `REPL` and watch our application change / re-render based on the changes. Or we can re-implement a specific function that handles our data and watch our application change based on that. So basically, it is like having `redux-tools` but for the whole application rather than just `state`.

`Clojure` has a beautiful way of concurrent programming.

It offers us a great `Clojure - JavaScript interoperability`.

And this all just makes development much, much easier and quicker.

Also, because it follows a functional programming paradigm, we tend to create tiny re-usable and easily testable functions. This also speeds up future refactoring time.

# When?
We can use `Clojure` to build `SPAs` on Front End for example. Or to build a micro service / API on Back End.
We can even create a full - blown LEMP-like application with SSR (server side rendering). Oh and there's a one-liner scaffolding template for each one of these choices.

So basically, for web development, `Clojure` is very similar to `JavaScript` in terms of being useful on Front End and Back End as well (`node.js` replacement).

Back End and Front End can share common functions and we can even have separate function bodies, depending on whether it is Front End accessing it or Back End. For example, if we have a `logger` function and if we call this function from Front End, it will use `console.log` to output the message and if we call the same function from Back End, it will write the message to the log file.

And finally, we can use `Clojure` outside of Web Development too.

# How?
Below you'll see a few basic, as well as 'cool stuff' examples of `Clojure` vs `JavaScript`.
> Note: `Cljs` is for `ClojureScript`

### Basics
Just a few basic data structures
```clojure
;; a comment

;; null
nil

;; symbol
'hello

;; vector
[1 2 3 4]

;; list
(1 2 3 4)

;; keyword
:hello-world

;; map
{:first-name "John"
 :last-name "Smith"}

;; set
#{1 2 3 4}
```

As you might notice, `Clojure` does not care about commas. They are considered as whitespaces. They are completely optional. Common practice is to just strip them out.

`maps` are like `objects` in `JavaScript`. Their keys can be of almost any data structure, so we are not restricted to just one type like in `JavaScript`, where we are only allowed to have `Strings`.

The following `maps` are also valid:
```clojure
;; using strings as keys
{"a string key" "and it's value"}

;; using vectors as keys
{[1 2] "hello"
 [3 4] "world"}

;; using maps as keys
{{"name" "John"} "some value"}
```

Most common data type we use as keys is a `keyword` (`:this-is-an-example`).

We can also use almost all the special characters when naming things. And this allows us to write more descriptive code. For example, imagine having the following function names:
```clojure
is-valid?
shout!
transform-dog->cat
```

### Namespaces
JS
```js
// not applicable
```
Cljs
```clojure
(ns my-application.core
  (:require [my-application.utils :as utils])
```

### Definitions
JS
```js
const person = "John Smith";
const add = (a, b) => a + b;

// anonymous function
() => { ... } 
```
Cljs
```clojure
(def person "John Smith")
(defn add [a b] (+ a b))

;; anonymous functions
#( ... )
(fn [] ...)
```
Before we move any further, if you pay a closer attention to the syntax we've used here
```clojure
(def person "John Smith")
```
It is actually just a list (data structure) we've created here. And that's the whole idea of `Lisp` in general. The code is just data. So `Clojure` code is just a big list, with all the other nested lists. 

In fact, we can write the above code like so
```clojure
(def, person, "John Smith")
```
and it would still work.

An equivalent in `JavaScript` (if `JavaScript` was a `Lisp-like` language) would be something along the lines of:
```js
[const, person, =, "John Smith"]
// and then without commas
[const person = "John Smith"]
```

### Functions
JS
```js
const data = [1, 2, 3, 4, 5];

const add = (a, b) => a + b;
console.log(data.reduce(add));

const isOdd = x => !!(x & 1); // using bitwise
console.log(data.filter(isOdd).reduce(add));
```

Cljs
```clojure
(def data [1 2 3 4 5])

(print (reduce + data))

(print (reduce + (filter odd? data)))
```
Notice how we use `+` as a function here? Because it IS just a function. In fact, in `Clojure`, everything is a function (even `if`).
Also, we didn't define `odd?` anywhere. And this is what "missing functionality" meant in my *healthy ranting* section. In `Clojure` we just have these tiny functions built in.

Let's look at some mind - blowing examples. `Clojure` has a few building blocks behind the scenes, that can be extended and built upon. It feels like you can change the language and extend the actual language yourself. One of these types of 'building blocks' is `Protocol`. In short, a protocol is what an interface would be in OOP languages, but with a bit of different thinking - in OOP, an interface says that classes have to have these methods. But a protocol says that this and that type needs to know what to do when this function calls it.

Protocols are like sets of functionalities, but they don't determine an implementation, but rather just a specification. And the implementation happens on a data structure / data type. 

A bit abstract, I know. But let's look at the example here.

If I wanted to filter out numbers in a vector / an array that are less than number 4, I could do this:
```js
const isLessThan4 = x => x < 4;
[1, 2, 3, 4, 5].filter(isLessThan4);
```
and `Clojure` equivalent would be
```clojure
(defn less-than-4?
  [x]
  (< x 4))

(filter less-than-4? [1 2 3 4 5])
```

But I could also teach a `number` how to be a function.
```clojure
(extend-type js/Number
  IFn
    (-invoke
      ([self x] (> self x))))
```
So here I'm telling a Number data type (ignore `js/` part of it for now) - if you're used as a function, then take that `x` argument and return whether you are more than `x`.

And then I can just use it like so
```clojure
(filter 4 [1 2 3 4 5])
```
In this example, `IFn` is a protocol and it is a specification of `being executable`. So any data type can implement / extend this protocol and become an executable. But they need to have an implementation of `-invoke` method / function which is what will be called when this data type is used as an executable.

### JS interop
As seen in the example above, we used this `js/Number` syntax to extend it. `ClojureScript` exposes a global `js` namespace. It is almost as an alternate reality that we have full access to. Here are a few examples of using `js` realm in `ClojureScript`.
```clojure
;; calling console.log in javascript
(.log js/console "Hello world")

;; calling an alert
(.alert js/window "Hi there")

;; accessing document title
(.-title (.-document js/window))
```
So for calling methods, we use `.method` syntax and to access a property, we use `.-property` syntax. If the above is not that readable, we can pipe them through the thread operator like so
```clojure
(-> js/window
    .-document
    .-title)
```

And here's how we can use ANY `js` library loaded in our page:
```clojure
;; good ol' jQuery
(.height (js/jQuery "#some-div"))

;; some moment.js
(.fromNow (js/moment))
```

### Reagent
Now, you might be wondering - is this how we use `React` too? Well, we could. But that would be a pain. We would just be writing `JavaScript` in `Clojure`, instead of just writing things `Clojure` way.

So there are a few `React` bindings for `Clojure`. They are sort of like robust wrappers around `React.js`. There are a few, but my personal favourite is [reagent](https://reagent-project.github.io/). With `reagent`, we create components using `hiccup`. `hiccup` is sort of like `jsx` but for `Clojure`. And the best thing about `hiccup` is that we don't have to add any extra convertors like `hiccup to Clojure`, because they are just `Clojure` vectors.

Here's an example component:

JS
```js
const PersonView = person => <div>{person.name}</div>;
```
Cljs
```clojure
(defn person-view [person]
  [:div (:name person)])
```
It's just a vector, that returns a `keyword` as a first element and a person's name as a second.

A few more pseudo examples
```clojure
(defn toolbar-item []
  [:div {:id :some-id} "Hello world"])

(defn ok-button [is-active?]
  [:button {:class (when is-active? "active")
            :on-click #(.log js/console "This was clicked")}
    "Click here"])
```
Again, each element in the vector is just a simple data structure that's already there, in `Clojure`.

So we can already see how flexible this can be. To compare, in a typical `React` component that returns some `jsx`, that point is the end of the computation. In this pseudo code we have a `Button` component and a function that wants to add `!` to the text within that button:
```js
const Button = ...
const addExclamationMarkToButtonText = button => ...
```
In `JSX` we can't do `<addExclamantionMarkToButtonText(Button) />`, because the `JSX` returned by that `Button` is not extendible.

In `Clojure` we can, because again, it's just a simple vector that is returned from a component function.

### Re-frame
So `reagent` allows us to create components and it is all about UI / Views. And then, there is [re-frame](https://github.com/Day8/re-frame), which is built on top of `reagent` and it offers the whole application architecture. It gives us a nice way to manage side effects (ajax calls, local storage, dealing with things like intervals, timeouts and etc.), it gives us a way to manage state efficiently and capture user intents. It offers these things through a few building blocks it comes with and it makes the code re-usable, testable and easily maintainable.

`re-frame` is like `redux` but on steroids. `Redux` gives us a way to manage the state, but `re-frame` gives us the way to manage the whole application.

Let's imagine that we have an application that needs to call an api endpoint and fetch users on an initial load. And at the same time, it needs to set `loading` state in our application and once the users are fetched, it needs to update the database with newly fetched users + update that `loading` state.

Let's look at the thinking / logic of `React + Redux` and then `re-frame`.

In `React + Redux`, we already know a few things:
1. We need a module like `api/users` that exports `getUsers` function for example
2. We need an action function, somewhere, that receives a `dispatch` function through args (assuming we're using redux-thunk) and it will do something along the lines of 
```js
// actions
const getUsers = () => async dispatch => {
  const users = await api.getUsers();
  dispatch(updateUsers(users));
};
```
3. Then we need a few `ActionType` constants defined somewhere to use them inside the reducer and inside that `updateUsers` action + to test these later.
4. We also need our root component (or whatever component needs that data) to be connected to redux, for us to use `mapStateToProps`, `mapDispatchToProps` or `mergeProps` and then to call `this.props.getUsers()`. So we need to use `connect` function from `react-redux` and etc.
5. Finally we need this root component to make that call in `componentWillMount` lifecycle probably.

So we're thinking about the technical bits here and we have a lot of moving pieces to achieve something simple and we have to do it this way so that our code is scalable later on. 

Another thing is that our `getUsers` action is responsible for two things - handling an api call and dispatching events based on the result. 

And to test that action, we have to mock `api.getUsers` and notice, we're not capturing the intent anywhere. We're just capturing what happens when.

Now let's look at the `re-frame` way.

In `re-frame` we define a database / state in its own file (and we can split it into different files). And this is our initial application state. And it is just a simple `map` that looks like this

```clojure
(def default-db
 {:loading false
  :users []}
```
There is no `createStore` or `combineReducers` anywhere.

Then we register an event. For example, `initialise-application`:
```clojure
(re-frame/reg-event-fx
  :initialise-application
  (fn [cofx [_ _]]
    {:db   (assoc default-db :loading true)
     :http {:method      :get
            :url         "/api/users"
            :on-success  [:update-users]
            :on-failure  [:display-error]}}))
```
Ignore the `(fn [cofx [_ _]]` and if we look at the body of that function, we can see that it just returns a simple map. And this is the intent. The keys in this map correspond to effects that will happen in our application (`:db` and `:http`) in this case. And our `initialise-application` event doesn't care what these effects do. It just returns an intent what it wants to do.

So our `initialise-application` event wants to produce 2 effects in our application:
1. create a database out of `default-db`, but with an updated `loading` state.
2. fire `http GET` request to `/api/users`

And notice how testable this is. Instead of mocking the api calls, we can simply check if the intent `map` includes an `http` (or whatever effect is going to take place in the application).

Then we can register just a database effect here:

```clojure

(re-frame/reg-event-db
  :update-users
  (fn [db [_ users]]
    (assoc db :users users)))
```

Notice `reg-event-db` instead of `reg-event-fx`. This is there in re-frame, because most times we will be registering events that update the database (state). We could do it the other way as well, but this one just has a shorter syntax as it specifically targets `:db` effect.

And then we 'teach' our application how to handle the actual effect like so:
```clojure
(re-frame/reg-fx
  :db
  (fn [value]
    (reset! app-db value)))
```
> Note: re-frame exposes app-db, which is where our application state is stored.

So as you can see, effects are very tiny building blocks that do just 1 thing. `:db` effect does its own thing, `:http` would do its own things and etc.

`:db` and `:http-xhrio` effects actually come with `re-frame`, but we just re-implemented it in the example above. And we can have as many different types of effects as we want.

And now that we have `:initialise-application` event, we can dispatch it from our application right before we mount our root component like so

```clojure
(defn init []
  (re-frame/dispatch [:initialise-application])
  (mount-root-component))
```
And this is just a function call. It's not within any component lifecycles. A component doesn't need to initialise an application. It needs to be as dummy as possible. It just needs to do its job - display things.

Let's look at some other types of event dispatch examples:

```clojure
(defn list-item [product]
  [:div
    [:button {:on-click #(re-frame/dispatch [:add-to-basket product])}]
    (:label product)])

(defn some-link [link]
  [:a {:on-click #(do
                    (.preventDefault %)
                    (re-frame/dispatch [:make-clojure-default]))}
    "Click to make Clojure your default programming language"])
```

And then we also have a way to create subscriptions to our database / state.

```clojure
(re-frame/reg-sub
  :users
  (fn [db]
    (:users db)))
```

And then in our component, we can subscribe to that like so
```clojure
(defn root-component []
  (let [users (re-frame/subscribe [:users])]
    [:div
      [users-view users]])) ;; btw, a nested component here
```

Subscriptions in `re-frame` are reactive. So we can chain them and our components will only ever update when that particular subscription's value changes.

```clojure
(re-frame/reg-sub
  :users
  (fn [db]
    (:users db)))

(re-frame/reg-sub
  :active-users
  (fn [_]
    (re-frame/subscribe [:users]))
  (fn [users]
    (filter active? users)))
```

This way, the component that is subscribed to `active-users`, will only ever re-render when the output of this specific subscription changes.

# Where?
- [Clojure eBook worth every second of reading](https://www.braveclojure.com/)
- [Clojure Docs](http://clojuredocs.org/)
- [Clojure Build Tool](https://leiningen.org/)
- [Clojure Slack channel](http://clojurians.net/)
- [Clojure project templates](https://clojars.org/)

# Conclusion
It is of course impossible to fit every cool bit of a language + a library / framework in a post like this. There is much, much more to not only `re-frame`, but to `Clojure` in general.

It is definitely a tool that will look cool in your tool belt. :)
