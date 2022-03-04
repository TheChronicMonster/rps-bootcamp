# Week 5
# Play and Play Again

## Resources

[Deck](https://docs.google.com/presentation/d/1p8KX5mfrmfL4Snw3FfAtq5aq6TCOyoQre_rUJV9QvHM/edit?usp=sharing)

[Repo](https://github.com/TheChronicMonster/rps-bootcamp)

[Week 5 Multiple Choice](https://forms.gle/nBeWVJ6DGQCGCDSR8)

[Week 5 Quiz](https://forms.gle/hL6D8b9bCKUwgnvK7)

Complete the "Code It" portion in a [secret gist](https://gist.github.com) and share the link with your mentor.

The Independent Project can be completed in a GitHub repository. Share the repo link with your mentor.

### Definitions & Concepts

Terms for this unit include:

invariant

while loops

continue

## Knowledge Reinforcement

Loops are important in any program for a number of reasons. 
In the case of _Rock, Paper, Scissors_, players will become annoyed by needing to deploy and attach the contract after a draw. 
In any given program, there are often certain operations that need to iterate multiple times to achieve the participant's desires. 

Due to the static nature of Reach's identifier bindings, 
while loops will either never begin or never end because static loop conditions never change. 
To resolve this, Reach has the ability to introduce a variable binding, known as a `loop invariant`.

A [loop invariant](https://en.wikipedia.org/wiki/Loop_invariant) is a property of a program loop that is true before and after each loop. 
Loop invariants are only concerned with values that can change.  

There are only two kinds of mutable bindings in Reach programs: loop variables and the contract balance. For this reason, you'll often utilize `pay` and `transfer` within a while loop in Reach. 

### Designing a Loop Invariant

It's important to understand the equation for the contract balance before the loop is begun. 
Transfers within the loop must be tracked with precision. 
Ideally, you'll express the balance as an equation over loop variables. 
After tracking the balance, add additional clauses that track the other necessary properties that are needed at the end of the loop.

For more complicated programs, such as when nested loops are required, the inner loop's invariant will include clauses related to the outer loop's invariant. 

### Loops and the Verification Engine

So that the verification engine doesn't trigger a false alarm, it is important to state what properties are invariant before the `while` loop's execution. 

### Reach Mode of a Loop

Loops are only allowed inside of consensus steps. Attempting a loop inside of a step or a local step will create an error, so it's important to refrain from committing data before utilizing a loop. 

The reason for this constraint is because all participants must agree on control flow direction in the DApp, and they can only come to a unified understanding in the consensus step.

Additionally, loop invariants reference the contract balance, and this consensus must be reached in the consensus step.

### Invariant Loop Example

Let's look at a prototype of a while loop with loop invariants. Recall that a loop invariant is a property that is true before the loop begins and after the loop ends. 

We will name the loop invariant `INV`

``` rsh
... before ...
var V = INIT;
invariant( INV );
while ( COND ) {
  ... body ...
  V = NEXT;
  continue; }
... after ...
assert(P);
}
```

The first two lines imply that an earlier part of the program must have established the invariant, `INV`. 
This `INV` must be true before entering the loop and after exiting.
`var V` is subject to change in the course of the loop's execution.

The body of the loop will utilize the truthfulness of the condition, `COND`, and the invariant, `INV` after the variable `V` has mutated to `NEXT`.

`P` is asserted in the latter part of the program when `COND` is no longer true. The invariant `INV` can be used to establish future assertions. 

Now that we have an understanding of the anatomy of a while loop with loop invariants, let's examine a portion of the loop created in this lesson. I'll drop in our prototype as comments so you can see how the prototype relates to a real code sample:

``` rsh
/* 
// ... before ...
// Note that we're in the consensus step
// var v = INIT;
*/

var outcome = DRAW;
// invariant( INV );
invariant( balance() == 2 * wager && isOutcome(outcome) );
// while ( COND ) {
while ( outcome == DRAW ) {
  // ... body ...
    commit();
    /*
    // closed consensus step inside of while loop body
    // participants interact as needed inside the loop
    */

    Alice.only(() => {
      // do something in local step
    });
    Alice.publish(commitAlice)
      // something in consensus step
    commit();
    // participants continue their interactions...

    // V = NEXT;
    outcome = winner(handAlice, handBob);
    continue;
}
// ... after ...
// assert(P);
```

Here's the full block without the additional comments:

``` rsh
var outcome = DRAW;
invariant( balance() == 2 * wager && isOutcome(outcome) );
while ( outcome == DRAW ) {
    commit();

    Alice.only(() => {
        const _handAlice = interact.getHand();
        const [_commitAlice, _saltAlice] = makeCommitment(interact, _handAlice);
        const commitAlice = declassify(_commitAlice);
    });
    Alice.publish(commitAlice)
      .timeout(relativeTime(deadline), () => closeTo(Bob, informTimeout));
    commit();
    
    unknowable(Bob, Alice(_handAlice, _saltAlice));
    Bob.only(() => {
        const handBob = declassify(interact.getHand());
    });
    Bob.publish(handBob)
      .timeout(relativeTime(deadline), () => closeTo(Alice, informTimeout));
    commit();

    Alice.only(() => {
        const saltAlice = declassify(_saltAlice);
        const handAlice = declassify(_handAlice);
    });
    Alice.publish(saltAlice, handAlice)
      .timeout(relativeTime(deadline), () => closeTo(Bob, informTimeout));
    checkCommitment(commitAlice, saltAlice, handAlice);

    outcome = winner(handAlice, handBob);
    continue;
}
```

Learn more about [loops in Reach](https://docs.reach.sh/guide/loop-invs/#guide-loop-invs) by reviewing the linked guide. 

**invariant**

An invariant is a statement that a property or an expression will remain the same before and after the while loop body's execution. 
For example, a loop invariant may state that the contract balance will remain the same, or that an variable will remain immutable.

**while**

While loops in Reach occur within a consensus step. It is proceeded by an invariant expression and ends with a `continue` statement.

**continue**

Reach requires that `continue` be explicitly written into the loop body. `continue` is a terminator statement.


---
### _Code it_

Review your gist for Pat and Vanna from lessons 1 and 2. Use the starter code you created to create a while loop for Pat and Vanna's contract. 

The while loop should allow Pat and Vanna to each get two challenges and see the result of the challenge in their local step.
---

## Independent Project

This week, we return to our Morra game to add a while loop to the game. Be sure to add the invariant, track the mutable variable, and end the loop with a `continue` statement.

You may use the resource as a guide, however, do not copy and paste, and don't type your code while looking at the resource. 
Only use the resource as a last resort. When you use it, flip between the resource and your text editor to help build your Reach programming muscles.

### Morra
Resource [link](https://github.com/algorand-devrel/Reach-Morra-Game/tree/main/morra2)

## Quiz

