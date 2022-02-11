# Week 3
# Trust and Commitments

## Resources

[Deck](https://docs.google.com/presentation/d/1SaVCTIzWaxrabHt1Pib3CjrcUBXuZEPM4ieeTpKmYxs/edit?usp=sharing)

[Repo](https://github.com/TheChronicMonster/rps-bootcamp)

[Week 3 Multiple Choice](https://forms.gle/ixJeWWGgEqvmiogPA)

[Week 3 Quiz](https://forms.gle/Tj5YqX5e1FfYZEKx8)

Complete the "Code It" portion in a [secret gist](https://gist.github.com) and share the link with your mentor.

The Independent Project can be completed in a GitHub repository. Share the repo link with your mentor.

## Knowledge Reinforcement

In Trust and Commitments we explore attack vectors in web3 apps and learn how Reach uses mathematical verification to protect honest participants. 

We attack the DApp and then explore how `assert` and `salt`s are used to deter attack vectors.

### Definitions & Concepts

Terms for this unit include:

assert

verification engine

forall

hasRandom

makeCommitment()

checkCommitment()

salt

**assert**

Your goal, as a Reach programmer, is to write every assumption you have into the program in the form of `assert` statements.

```cmd
assert( claim, [msg] )
```

[`assert`](https://docs.reach.sh/rsh/compute/#assert) is a static assertion that is only valid if `claim` resolves to true. `[msg]` refers to an optional bytes argument, which is included in violation reports. Look at this example from Token minting for a better understanding. 

```rsh
require(supply >= 2 * amt);
const tok = new Token({ name, symbol, url, metadata, supply, decimals });
transfer(amt, tok).to(who);
tok.burn(amt);
assert(tok.supply() == supply - amt);
tok.burn();
assert(tok.destroyed() == false);
tok.destroy();
```

In this example we use a [`require`](https://docs.reach.sh/rsh/consensus/#rsh_require) expression to confirm that the supply is greater than or equal to two times of the available amount, `amt`. 
Create a token constant named `tok` and then transfer an amount of a token to a participant.
The token, `tok` is then burned and it's `assert`ed that the supply of the token is equal to the difference in the amount and the total supply available. 
Thanks to the logic of an `assert` expression, if `tok.supply() == supply - amt` evaluates to `false` then the contract will self-destruct. 
Only if the expression is `true` will the contract move forward to the next command, and then immediately thereafter, the contract will run another `assert` to mathematically verify that the token `tok` has not been destroyed.

There are five types of [assertions](https://docs.reach.sh/model/#p_30):

1. A knowledge assertion: claims that one honest participant **cannot know** something that another honest participant does know.

1. A static assertion: an "always-true" formula. 

1. An assumption (`assume`) of a true formula if frontends behave honestly. 

1. A `require`ment; a true formula if participants behave honestly.

1. A possibility `assert`ion; a truthful formula derived from values in a formula provided by honest participants and frontends.

Honest participants are defined as those who only execute steps specified by the DApp.

An honest frontend only returns values that ensure all assumptions evaluate to the boolean `true`.

#### _Understanding the Compiler_

Understanding the compiler allows you to understand the underpinning philosophy of Reach. "There are things that are not permissible, all of which, you will not be allowed to do. There are things that are permissible, which you may do, however, there are some permissible things that cannot be proven to be `true`. You will not be allowed to do those things."

At compile time, the Reach compiler will attempt to produce counter-examples that falsify the `claim` of an assert statement. When the compiler successfully shows a `claim` to be `false` it will fail with a detailed message explaining the evaluation. It is possible to write a `claim` that always evaluates to `true`, without the compiler being able to prove that fact. When this happens, Reach will fail to compile and report its analysis as incomplete.

_Reach never produces erroneous counter-examples._ (If the compiler states that an `assert`ion is `false`, it is mathematically proven to be so).

### The Reach Verification Engine

As stated above, your goal is to write every assumption you have about your program in the form of `assert` statements. The intention is not to help the verification engine prove the truth of such statements, but to provide assumptions that the verification engine can use to attempt to falsify the program.

Unit tests for functions that would normally be created in a test suite should be written as a series of assertions directly in the block that defines a given function.

>_Assigned Reading_
>
>Read more about Reach's Verification Engine in the guide, _[How and What to Verify](https://docs.reach.sh/guide/assert/#guide-assert)_.


#### _Verification Engine Messages_

Let's look at a failed verification message. 

This is the failing code block:

```
  Bob.only(() => {
    interact.acceptWager(wager);
    const handBob = declassify(interact.getHand());
  });
  Bob.publish(handBob)
    .pay(wager);
  const outcome = (handAlice + (4 - handBob)) % 3;
  const            [forAlice, forBob] =
    outcome == 2 ? [       1,      0] : // <- Oops. was: [2, 0]
    outcome == 0 ? [       0,      2] :
    /* tie      */ [       1,      1];
  transfer(forAlice * wager).to(Alice);
  transfer(forBob * wager).to(Bob);
  commit();
```

Here is the message that results from a failed attempt to compile.

```
Verification failed:
  when ALL participants are honest
  of theorem: assert
  msg: "balance zero at application exit"
  at ./index-bad.rsh:8:30:compileDApp
  // Violation Witness
  const v75 = "Alice".interact.wager;
  //    ^ could = 1
  //      from: ./index-bad.rsh:11:10:property binding
  const v78 = protect<UInt>("Alice".interact.getHand());
  //    ^ could = 8
  //      from: ./index-bad.rsh:21:50:application
  const v88 = protect<UInt>("Bob".interact.getHand());
  //    ^ could = 4
  //      from: ./index-bad.rsh:29:48:application
  // Theorem Formalization
  const v97 = (v78 + (4 - v88)) % 3;
  //    ^ would be 2
  const v104 = ((v97 == 2) ? [1, 0 ] : ((v97 == 0) ? [0, 2 ] : [1, 1 ]));
  //    ^ would be [1, 0 ]
  const v118 = 0 == (((v75 + v75) - (v104[0] * v75)) - (v104[1] * v75));
  //    ^ would be false
  assert(v118);
  ```

"Verification failed: when ALL participants are honest of theorem: assert" informs us that the compiler failed an assertion. You'll notice in the code block that there are no `assert`s, `assume`s, or `require`s, and yet, the VEngine failed due to a `false` assert with a message of "balance zero at application exit." 

How and why did that happen? 

Reach's verification engine will always check
* That the contract does not "overspend"
* That there are no tokens left in a contract when/if it exits
* That formulas are true with honest participants 
* That formulas are true with dishonest participants

The message, "balance zero at application exit" indicates that the theorem that verifies that a contract has a zero balance at exit failed.

The message under "Violation Witness" detail values that will cause the contract to fail and where.

The message under "Theorem Formalization" outline the theorem(s) that failed.

As web3 developers using Reach, it is our job to to assist the verification engine to catch errors beyond the default categories by using `assert`ions, as outlined above.

Let's look at one more example. 

**forall**

In our first iteration, we trusted that our formula codifying 'Rock, Paper, Scissors' results was correct.

```
'reach 0.1'

const [ isHand, ROCK, PAPER, SCISSORS ] = makeEnum(3);
const [ isOutcome, B_WINS, DRAW, A_WINS ] = makeEnum(3);

const winner = (handAlice, handBob) => 
  ((handAlice + (4 - handBob)) % 3);
```

We can do better than blindly trusting this formula is correct. We are now equipped with the ability to verify the correctness of the expression. 
In web2, we might implement a frontend that returned testing scenario values and check the output. That's possible with Reach, as well, however, we have the ability to verify test cases directly in a Reach program as verification assertions.

```
assert(winner(ROCK, PAPER) == B_WINS);
assert(winner(PAPER, ROCK) == A_WINS);
assert(winner(ROCK, ROCK) == DRAW);
```

This is a great place to start with assertions, but we can utilize a `forall` to create a more robust statement about how our DApp should behave. 

```
forall(UInt, handAlice =>
  forall(UInt, handBob =>
    assert(isOutcome(winner(handAlice, handBob)))));
```

[`forall`](https://docs.reach.sh/rsh/compute/#rsh_forall) is a computation that may only be referenced inside of assertions. Using `forall` in this manner ensures that `winner` will always provide an outcome that will be true, no matter the values provided. 

We can also express that when the input values are the same, the outcome will always be a `DRAW`.

```
forall(UInt, (hand) =>
  assert(winner(hand, hand) == DRAW));
```

`forall` allows you to quantify over all the possible inputs that might be provided. 
You'd think proving billions of possibilities would require more time than the heat death of our sun will require, but thanks to Reach's [symbolic execution engine](https://docs.reach.sh/guide/reach/#guide-reach) utilizing [satisfiability modulo theories](https://en.wikipedia.org/wiki/Satisfiability_modulo_theories), it takes less than half of a second to verify.

**hasRandom**

[`hasRandom`](https://docs.reach.sh/rsh/compute/#hasrandom) is a participant interact interface which specifies random as a function that takes no arguments and returns an unsigned integer of bit width bits. 
Reach does not natively support randomness. 
For the convenience of developers, Reach provides a default frontend implementation via `hasRandom`. 
Random number generation is left to the frontend implementation. It's not required to use `hasRandom` in a Reach program. 
Any random number generator may be implemented in the frontend.

When using `hasRandom` in the backend interface, the backend will expect the frontend to provide the functionality. 
Adding `hasRandom` to the frontend interact object enables the frontend to provide `hasRandom` functionality to the backend.

>index.rsh
```rsh
const Player = {
  ...hasRandom, // backend expects frontend to provide `hasRandom`
  getHand: Fun([], UInt),
  seeOutcome: Fun([UInt], Null),
};
```

>index.mjs
```mjs
const Player = (Who) => ({
  ...stdlib.hasRandom, // Creates random number generator functionality
  getHand: () => {
    const hand = Math.floor(Math.random() * 3);
    console.log(`${Who} played ${HAND[hand]}`);
    return hand;
  },
  seeOutcome: (outcome) => {
    console.log(`${Who} saw outcome ${OUTCOME[outcome]}`);
  },
});
```

**makeCommitment()**

`makeCommitment( interact, x )`

`makeCommitment()` is a function provided by the standard library to help developers create [cryptographic commitment schemes](https://en.wikipedia.org/wiki/Commitment_scheme). `makeCommitment()` returns two values, `[ commitment, salt ]`, where `salt` is the result of calling `interact.random()`, and `commitment` is the digest of `salt` and `x`. This is used in a local step before `checkCommitment` is used in a consensus step.

>In computer terms, a [digest](https://ell.stackexchange.com/questions/166833/what-is-mean-digest-in-computer-terminology#166837) is made by condensing a document, message, keyword or other data item into a very short fixed-length summary, for example CRC (typically 32 bits) MD5 (128 bits) or SHA-1 (160 bits). 
>In cryptography, a [digest](https://www.webopedia.com/definitions/message-digest/) can be used to verify the integrity of a transmitted document and to generate a unique identifier for an item.
>In this case, digesting `salt` and `x` refers to the process of condensing their information into a single `commitment` argument.

```
  Alice.only(() => {
    const wager = declassify(interact.wager);
    const _handAlice = interact.getHand();
    const [_commitAlice, _saltAlice] = makeCommitment(interact, _handAlice);
    const commitAlice = declassify(_commitAlice);
  });
  Alice.publish(wager, commitAlice)
    .pay(wager);
  commit();
```

**checkCommitment()**

`checkCommitment( commitment, salt, x )`

`checkCommitment()` makes a requirement that `commitment` is the digest of `salt` and `x`. This is used in a consensus step after `makeCommitment` is used in a local step.

`checkCommitment()` is used to check that published values match the original values. This will always be the case with honest participants, but dishonest participants may violate this assumption.

```
  Alice.only(() => {
    const saltAlice = declassify(_saltAlice);
    const handAlice = declassify(_handAlice);
  });
  Alice.publish(saltAlice, handAlice);
  checkCommitment(commitAlice, saltAlice, handAlice);
```

Let's look at another example of `checkCommitment()` in use.

```
  Alice.only(() => {
    const wager = declassify(interact.wager);
    const _handAlice = interact.getHand();
    const [_commitAlice, _saltAlice] = makeCommitment(interact, _handAlice);
    const commitAlice = declassify(_commitAlice);
  });
  Alice.publish(wager, commitAlice)
    .pay(wager);
  commit();
```

In the line `const [_commitAlice, _saltAlice] = makeCommitment(interact, _handAlice);`, Alice computes a commitment to her hand. 
This computation is stored in an array that holds the secret commitment and a salt. 
The secret "salt" value must be revealed later, but not yet. 
If we reveal a salt too soon then a dishonest Bob or an Eve (eavesdropper) could use this information against Alice. 
One final thing to note here, is that this "salt" was generated by the `random` function inside of `hasRandom` and it's why we pass `interact` to this function.
For your convenience, the anatomy of `makeCommitment()` looks like: `makeCommitment( interact, x )`.

**salt**

According to [Wikipedia](https://en.wikipedia.org/wiki/Salt_(cryptography)), a salt is random data that is used as an additional input to a one-way function that hashes data, a password or passphrase. Salts are used to safeguard secret information.
In Reach, if you choose to use the standard `hasRandom` functionality, then a "salt" is generated by the random function inside of hasRandom. This is why we pass interact to the `makeCommitment()` function in the snippet below.

```
Alice.only(() => {
  const wager = declassify(interact.wager);
  const _handAlice = interact.getHand();
  const [_commitAlice, _saltAlice] = makeCommitment(interact, _handAlice);
  const commitAlice = declassify(_commitAlice);
});
Alice.publish(wager, commitAlice)
  .pay(wager);
commit();
```

It is important to include the salt in the commitment, so that multiple commitments to the same value are not identical. It's also important not to share the salt until needed, because if an attacker knows the set of possible values, they can enumerate them and compare with the result of the commitment and learn the value. This is another reason for using a randomly generated salt.

To reveal salts we simply use `declassify` in a local step, similarly to how we'd declassify any other information. In the snippet below, we can see how Alice declassifies her salt and publishes it along with her hand.

```
Alice.only(() => {
  const saltAlice = declassify(_saltAlice);
  const handAlice = declassify(_handAlice);
});
Alice.publish(saltAlice, handAlice);
checkCommitment(commitAlice, saltAlice, handAlice);
```

## Independent Project

This week, we return to our Moora game. You may need to review your work from the first week and consult your notes to refresh yourself on how the game works and how you'll continue to codify the game. 

This week, your creating the logic of the game. You'll populate the participant interface with methods and define their functionality in the frontend interact object.

You may use the resource as a guide, however, do not copy and paste, and don't type your code while looking at the resource. 
Only use the resource as a last resort. When you use it, flip between the resource and your text editor to help build your Reach programming muscles.

For this module, in the backend, build the skeleton of the participant interact interface and the skeleton of the Reach App initialization. In the frontend, create a starting balance, mirror the participant interact interface, and initialize the contract.

### Moora
Resource [link](https://github.com/algorand-devrel/Reach-Morra-Game/tree/main/morra1)

## Quiz

1. True or False: Using `forall` can be dangerous because of how long it takes the Reach program to compile.

1. How are salts revealed to other participants?

1. In what step can a makeCommitment() be used?

1. In what step can a checkCommitment() be used?

1. True or False: To create random numbers, a Reach programmer must use `hasRandom`.