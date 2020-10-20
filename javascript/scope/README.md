# Scope

Time for the scope chain 🕺🏼 In this post I assume you know the basics of execution contexts: I’ll soon write a post on that too though 😃

Let's take a look at the following code:  


```text
const name = "Lydia"
const age = 21
const city = "San Francisco"


function getPersonInfo() {
  const name = "Sarah"
  const age = 22

  return `${name} is ${age} and lives in ${city}`
}

console.log(getPersonInfo())
```

We're invoking the `getPersonInfo` function, which returns a string containing the values of the `name`, `age` and `city` variables:  
`Sarah is 22 and lives in San Francisco`. But, the `getPersonInfo` function doesn't contain a variable named `city` 🤨? How did it know the value of `city`?

First, memory space is set up for the different contexts. We have the default **global context** \(`window` in a browser, `global` in Node\), and a **local context** for the `getPersonInfo` function which has been invoked. Each context also has a **scope chain**.

For the `getPersonInfo` function, the scope chain looks something like this \(don't worry, it doesn't have to make sense just yet\):

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--57Tstr0c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/89b9buizhevs0jf6djyn.png)

The scope chain is basically a "chain of references" to objects that contain references to values \(and other scopes\) that are referencable in that execution context. \(⛓: "Hey, these are all the values you can reference from within this context".\) The scope chain gets created when the execution context is created, meaning it's created at runtime!

However, I won't talk about the _activation object_ or the execution contexts in general in this post, let's just focus on scope! In the following examples, the key/value pairs in the execution contexts represent the references that the scope chain has to the variables.

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--3L9mEScA--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/iala2et7bg9bgdj4c2lg.png)

The scope chain of the global execution context has a reference to 3 variables: `name` with the value `Lydia`, `age` with the value `21`, and `city` with the value `San Francisco`. In the local context, we have a reference to 2 variables: `name` with the value `Sarah`, and `age` with the value `22`.

When we try to access the variables in the `getPersonInfo` function, the engine first checks the local scope chain.

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--e_017_sT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/xn17f0t54acz8tiq7122.gif)

The local scope chain has a reference to `name` and `age`! `name` has the value of `Sarah` and `age` has the value of `22`. But now, what happens when it tries to access `city`?

In order to find the value for `city` the engine "goes down the scope chain". This basically just means that the engine doesn't give up that easily: it works hard for you to see if it can find a value for the variable `city` in the outer scope that the local scope has a reference to, the **global object** in this case.

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--uE5KEpLT--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/z9iclg23rmbpts7meoq6.gif)

In the global context, we declared the variable `city` with the value of `San Francisco`, thus has a reference to the variable `city`. Now that we have a value for the variable, the function `getPersonInfo` can return the string `Sarah is 22 and lives in San Francisco` 🎉

We can go _down_ the scope chain, but we can't go _up_ the scope chain. \(Okay this may be confusing because some people say _up_ instead of _down_, so I'll just rephrase: You can go to **outer** scopes, but not to more inner... \(innerer..?\) scopes. I like to visualize this as a sort of waterfall:

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--mJn1OY2F--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/doq46yc6nuiam51evy44.png)

Or even deeper:

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--ymRwRgkB--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/rece2zj4pb4w1fn56q5k.png)

Let's take this code as an example.

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--HQ8cmkNI--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/0z6342b72f3n6v6ufafk.png)

It's almost the same, however there's one big difference: we _only_ declared `city` in the `getPersonInfo` function now, and _not_ in the global scope. We didn't invoke the `getPersonInfo` function, so no local context is created either. Yet, we try to access the values of `name`, `age` and `city` in the global context.

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--bqIqLoUF--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/f3wvlo4c3gqf3mve1g0n.gif)

It throws a `ReferenceError`! It couldn't find a reference to a variable called `city` in the global scope, and there were no outer scopes to look for, and it **cannot** go _up_ the scope chain.

This way, you can use scope as a way to "protect" your variables and re-use variable names.

Besides global and local scopes, there is also a **block scope**. Variables declared with the `let` or `const` keyword are scoped to the nearest curly brackets \(`{}`\).  


```text
const age = 21

function checkAge() {
  if (age < 21) {
    const message = "You cannot drink!"
    return message
  } else {
    const message = "You can drink!"
    return message
  }
} 
```

You can visualize the scopes as:

![Alt Text](https://res.cloudinary.com/practicaldev/image/fetch/s--t-azbfm3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/75n1vpm7z4d8924cnvje.png)

We have a global scope, a function scope, and two block scopes. We were able to declare the variable `message` twice, since the variables were scoped to the curly brackets.

To quickly recap:

* You can see "scope chain" as a chain of references to values that we can access in the current context.
* Scopes also make it possible to re-use variable names that were defined further down the scope chain, since it can only go _down_ the scope chain, not _up_.

That was it for scope \(chains\)! There's tons more to say about this so I may add extra info when I have some free time. Feel free to ask questions if you're struggling with anything, I love to help! 💕

[https://dev.to/lydiahallie/javascript-visualized-scope-chain-13pd](https://dev.to/lydiahallie/javascript-visualized-scope-chain-13pd)

{% embed url="https://developer.mozilla.org/ru/docs/Web/JavaScript/Closures\#%D0%9B%D0%B5%D0%BA%D1%81%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F\_%D0%BE%D0%B1%D0%BB%D0%B0%D1%81%D1%82%D1%8C\_%D0%B2%D0%B8%D0%B4%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B8" %}



