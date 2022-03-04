# Week 4: Timeouts & Participation

## Resources

[Deck](https://docs.google.com/presentation/d/1Seuf9eCp4C2OmnF6v7haIKxSbbtQv-H20bd0RJcmoA0/edit?usp=sharing)

[Repo](https://github.com/TheChronicMonster/rps-bootcamp)

[Week 4 Multiple Choice](https://forms.gle/2NMNA7nfSmyiy3n89)

[Week 4 Quiz](https://forms.gle/vUPAkRHkFHpg6wW68)

Complete the "Code It" portion in a [secret gist](https://gist.github.com) and share the link with your mentor.

The Independent Project can be completed in a GitHub repository. Share the repo link with your mentor.

## Knowledge Reinforcement

This section focuses on the timeout mechanism and the `closeTo` expression. 

### Definitions & Concepts

Terms for this unit include:

timeout

deadline

relativeTime()

closeTo()

**timeout**

The `timeout` method protects participants against non-participation attacks. Non-participation is often a trivial problem in web2, however, in web3, a single non-participating actor could stall a contract and lock funds indefinitely. 

There are two ways that non-participation can be dealt with: Punishment and Disincentivizing. 

Punishment involves taking assets from non-participating actors. However, the timeout threshold must be judiciously set or honest, but delayed, participants could be punished, unjustly. 

Disincentivizing can be an effective strategy when participants are asked to stake 10x, or some significant amount, more than their wager in order to ensure timely participation. 

Read more about timeouts in our [guide](https://docs.reach.sh/guide/timeout/#guide-timeout).

> How would you incentive Alice and Bob to complete a game of Rock, Paper, Scissors?

**deadline**

In the lesson this week, we created a field, `deadline`, which represents some number of time delta units. 
Many networks measure time in the number of blocks. 

`deadline` is a user made field. It's not required to name this field, "deadline". We simply need to track the passage of time in order to trigger the `timeout` mechanism. 
In _Rock, Paper, Scissors_, we program Alice to establish the deadline and then declassify that information in her local step. Later, Bob and Alice use the `deadline`'s time delta when measuring `relativeTime` in the event of a `timeout`.

**relativeTime**

`relativeTime()` is a computation that is introduced in this lesson, but is not given a detailed explanation in the tutorial. 
`relativeTime()` is a time argument that measures the relative passage of time, such as the change in the number of blocks.
The absolute passage of time, such as the passage of milliseconds, can be measured with `absoluteTime()`. 
The passage of seconds can be measured with `relativeSecs()` and `absoluteSecs()`

**closeTo**

`closeTo( Who, after, nonNetPayAmt )`

`closeTo` operates in the step mode and is a helpful standard library function that allows a participant to make a publication and transfer the `balance()` to `Who` and then end the DApp after executing the function `after` in a step.

Without `closeTo`, developers would be required to complete the following template:

```
// Has participant 'Anybody' make a publication
Anybody.publish();
// Transfer the balance() and the non-network payment amount to 'Who'
transfer([ balance(), ...nonNetPayAmt ]).to(Who);
commit();
// End the DApp after executing the function 'after' in a step
after();
exit();
```

---
### _Code it_

Write a code block that instructs a participant, Eve, to publish her `callLog` and attach a `timeout()` method that accepts a `relativeTime`, which accepts an argument, `deadline`. If the `deadline` is triggered, the contract should perform a `closeTo` for "Alice" and trigger the function `timeExpiration`.
---

## Independent Project

This week, we return to our Morra game to add a timeout mechanism.

You may use the resource as a guide, however, do not copy and paste, and don't type your code while looking at the resource. 
Only use the resource as a last resort. When you use it, flip between the resource and your text editor to help build your Reach programming muscles.

In this unit, give Alice and Bob timeout capabilities where appropriate. Use `closeTo` to transfer funds in the event of a timeout trigger and notify the frontend that a timeout has occurred.

### Morra
Resource [link](https://github.com/algorand-devrel/Reach-Morra-Game/tree/main/morra2)

## Quiz

[Week 4 Quiz](https://forms.gle/vUPAkRHkFHpg6wW68)