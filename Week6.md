# Week 6
# Interaction

## Resources

[Deck](https://docs.google.com/presentation/d/1SwWl8sU7-2dwTbW6xYWbwAXisPw0ByRZXRZU5-TjSiU/edit?usp=sharing)

[Repo](https://github.com/TheChronicMonster/rps-bootcamp)

[Week 6 Quiz](https://reachsh.typeform.com/to/r5mv4xTW)

### Definitions & Concepts

Terms for this unit include:

ask.ask

ask.yesno

ask.done

JSON.stringify && JSON.parse

stdlib.connector

## Lesson Summary

Command prompt applications are very 1970s. 
They're great for developers trying out new things, 
but the first mainstream DApp is going to feature a beautiful user interface that's easy to interact with on a mobile device. 

In this lesson we first explore how we can alter our frontend to be interactive between multiple terminals. 
Previously, we operated the entire DApp from a single terminal, 
but this is a poor simulation for the real world. 
In `mainnet`, users will operate contracts from their own terminals, and may interact with other participants from across the globe. 
Users will not want their decisions to be made randomly by a machine; 
they will want to make their own decisions.
 
Once we're satisfied with our interactive version, 
we'll build a web-based user interface with React.

### Knowledge Reinforcement

**ask.ask**

`ask`, or more specifically, `ask.ask` is an asynchronous function that asks a question on the console and returns a Promise. 
`ask.ask` is useful when the contract needs to obtain information from the user. 
A common question that is asked of the user is, 
"Are you the deployer of the contract, or are you attaching?" 
However, this question is most often asked as a yes/no question. 
i.e. "Are you deploying the contract?"

That's because the most common method to use after `ask.ask` is `ask.yesno`.

However, as we see in this lesson, 
`ask.ask` is used to obtain the answer to more open ended questions, such as, 
"What hand do you wish to play?"

**ask.yesno**

`ask.yesno` parses "Yes"/"No" answers. 
`ask.yesno` is essentially a boolean that only accepts "yes" or "y" and "no" or "n" as inputs. 

A common use of this method is to determine if the user is deploying the contract or attaching to it. 
However, `ask.yesno` can be used to secure any boolean responses.

In _Rock, Paper, Scissors_, `ask.yesno` is used to ask the user if they'd like to create a `devnet` account. 
If the user answers negatively, then the user is asked to supply an account number to connect to.

Of course, what happens after the user answers is determined by a conditional statement. 

**ask.done**

`ask.done` simply informs the program that `ask.ask` and `ask.yesno` will no longer be used.

**JSON.stringify** && **JSON.parse**

`JSON.stringify` is the stringified result of the Reach [contract handle](https://docs.reach.sh/frontend/#p_65) 
and `JSON.parse` is the parsed version of `JSON.stringify`.

The contract handles can be used after calling `ctc.getInfo()` to allow a participant to attach to the contract.

**stdlib.connector**

A [connector](https://docs.reach.sh/frontend/#p_16) is the abbreviated name of the consensus network that the DApp is connecting to. Reach can currently connect to Ethereum: `'ETH'`, Algorand: `'ALGO'`, and Conflux: `'CFX'`.

---
### _Code it_

Add interactivity to last week's Fortune Telling DApp by utilizing the `ask.ask` functionality. 

What happens if you refuse to answer a `ask.yesno` with a "y" or "n"?
How does the Reach program respond if you omit `ask.done`?

---

### Web Interaction with React

Now that _Rock, Paper, Scissors_ is secure, fully interactive, and otherwise complete, 
it's time to make your game shine with a new web-based user interface.

Reach makes this easy to do with React, 
but you can use any frontend of your choice by utilizing Reach's RPC Server. 
We won't cover that in this bootcamp, 
but if you'd like to learn more about RPC and how you can use it to build frontends using languages such as Python, C#, and Go, then check out _Rock, Paper, Scissors_ [Addendum](https://docs.reach.sh/tut/rps/7-rpc/). 

Returning to React, here are a few things you should know about ReactJS:

* React programs are JavaScript programs that use a special library that allows you to mix HTML inside JavaScript.

* React has a special compiler that packs a bundle of JavaScript programs, 
and dependencies, 
into one large file that can be deployed on a static Web server.

* When developing and testing with React, a special development Web server updates the packed file every time the source file is modifies so you don't have to constantly run the compiler.

* Reach automates the process of starting this development server for you when you run `./reach react` and gives you access to it at `http://localhost:3000/`.

**Wallets**

Creating a web-based DApp requires the use of a "crypto wallet". On Ethereum networks, the most common wallet is "[MetaMask](https://metamask.io/)". There are three popular options on Algorand, including [My Algo Wallet](https://wallet.myalgo.com/) and [Pera Wallet](https://perawallet.app/).

If this is your first time setting up a crypto wallet, 
you'll want to **write** the mnemonic phrase that is given to you with pen and paper in a notebook that will not be lost. 
It's best to avoid taking screenshots of the phrase.
Never share your phrase with anyone or your funds can be stolen. 

Think of the phrase and private key as the keys to your home. 
You are solely responsible for the phrase and key. 
A central authority cannot bail you out if these assets are lost.

**Views**

[Views](https://reactnative.dev/docs/view) are essential components in a React UI. 
They are containers that support multiple layouts, 
and can be nested within other views and have many children.

Each view contains a bit of code that determines how the view will be displayed on the web page. 
This is syntax that's common to React. 
There's no code in this lesson that's specific to the Reach programming language.

**Components**

[Components](https://reactjs.org/docs/glossary.html#components) are the building blocks of a React app. 
They are reusable and return elements that can be rendered in the app. 
Components are nestable and their names begin with capital letters. 

**Extends**

React component classes are defined by [extend](https://reactjs.org/docs/react-component.html)ing `React.Component`. 
[`extends`](https://carlmungazi.com/class-extends-react-lesson/) is a keyword that creates subclasses of the `React.Component`.

Close inspection of the _Rock, Paper, Scissors_ tutorial will show that interact interfaces are `extended` in the React frontend where interact object functionality is defined.

**Render DOM**

It's a small line, but remember to end the React DApp with `renderDOM(<App />);`. Without that line the DOM will not load!

---
### _Code it_

Enhance the Fortune Telling DApp with a React GUI.
---

## Independent Project

This week, we finalize Morra with a front-end interface. Use React to create a working user interface for the Morra game.

You may use the resource as a guide, however, do not copy and paste, and don't type your code while looking at the resource. 
Only use the resource as a last resort. When you use it, flip between the resource and your text editor to help build your Reach programming muscles.

### Morra
Resource [link](https://github.com/algorand-devrel/Reach-Morra-Game/tree/main/morra2)

## Quiz

https://reachsh.typeform.com/to/r5mv4xTW