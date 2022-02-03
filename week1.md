# Week 1
# Rock, Paper, Scissors

## Week Resources and Instructions

Complete this week's multiple choice questions and quiz by following the links below:

[Multiple Choice submission form](https://forms.gle/EfGpbTnVR68gR8iV9)

[Week 1 Quiz](https://forms.gle/TmP9MVQLsqEAPq7u8)

Complete the "Code It" portions in a [secret gist](https://gist.github.com) and share the link with your mentor.

The Independent Project can be completed in a GitHub repository. Share the repo link with your mentor.

**Plagiarism Warning** 

Plagiarism weakens the community and does nothing to improve your web3 development skills. 
Students that plagiarize are subject to being removed from the bootcamp and may not receive a certificate of completion. 
If you are stuck, lost, or confused, reach out to your mentor(s) or instructor for assistance.

## Knowledge Reinforcement

This section will help reinforce your knowledge of creating a Reach boilerplate. 
At the end of this section you should feel comfortable creating participants and participant interact interfaces.

### Definitions & Concepts

You should be comfortable with the following terms:

Reach.App

Consensus Network

only()

publish()

declassify()

.interact

### Initialize Reach Apps

**Reach.App**
Reach.App occurs in which part of a Reach application?

--Consensus Step

--App Init

--Step

--Local Step

---
#### _Code it_

_In an `.rsh` file Create a rudimentary Reach Application with two participants, "Pat" and "Vanna"_

_In an `.rsh` file Create a rudimentary Reach Application with three participants, "Creator", "Bidder", and "Buyer"_

---

### Build participant interact interfaces 

**Participant Interact interface**

Where in the Reach Application are Participants defined?

--Local Step

--Step

--App Init

--Consensus Step

---
#### _Code it_
_Define a participant interact interface that will be shared between Pat and Vanna. 
Provide two methods: "getChallenge", which takes no input and outputs an integer and "seeResult", which takes an integer input and has no output._

_Define a participant interact interface that will be shared between Creator, Bidder, and Buyer. 
Provide a method "seePrice", which takes no input and outputs an integer and "getDescription" which takes no input and outputs a byte._

---

### Participant Blocks
**only**
Recall that the template for writing a block of code that _only_ one specific Participant performs is:

``` rsh
Participant.only(() => {
    // Do something here
});
// Move to the consensus step
```

---
#### _Code it_
_Create PARTICIPANT.only code blocks for Pat and Vanna._

_Create PARTICIPANT.only code blocks for Bidder and Buyer._

---

### Interact & declassify constants within local steps
**interact**

_An interact expression may only occur in what part of a Reach program?_

--App Init

--Step

--Local Step

--Consensus Step

---
#### _Code it_
_Write an interact expression that binds the value to the result of interacting with Pat through the `getChallenge` method. Assign the expression to a `const` "challengePat"_

_Write an interact expression that binds the resulting value of interacting with Vanna through the `getChallenge` method. Assign the expression to a `const` "challengeVanna"_

_Write an interact expression that binds the value of `seePrice` for the `Bidder` participant. Assign the expression to a `const` "price"_

_Write an interact expression that binds the value of `seeDescription` for the `Buyer` participant. Assign the expression to a `const` "description"_

---

**declassify**

In which mode of a Reach Application is `declassify` accessible?

--Consensus step

--Local step

--Step

--App Init

---
#### _Code it_
_Add the declassify primitive to all the `interact` expressions above._

---

### Gaining Consensus
**publish**

_Publish allows the Reach app to move between which two steps?_

--Consensus Step ==> Local Step

--Local Step ==> Step

--Step ==> Local Step

--Step ==> Consensus Step

_`.publish`_ _joins a Participant to the application by publishing the value to the_ __________ __________ _and moves the code into the_ ____________ _______ _where all participants act together._

#### _Code it_
_Add `.publish` statements to the `only` code blocks. 
`.publish` statements should follow the format of:_
``` rsh
PART.publish(const);
```
where PART is the participant and const is the constant assigned to the declassified interact value.

## Independent Project: Morra

Over the course of this bootcamp, you will create an independent project. The game you'll create is known as [Morra](https://en.wikipedia.org/wiki/Morra_(game)). 
Your goal will be to create a game that allows two participants to choose a number of fingers to show and make a guess of how much the total amount will be. 
Participants will share their fingers and their guesses and a winner or draw will be determined.

Before you begin coding, follow the link above to understand the rules of the game. 
Identify the participants and how they'll interact with one another and the contract.
Write down on paper the order of operations of each participant.
Match every action to the appropriate step in the DApp. 
This will help you create a map of your program and will become a reference tool as you build.

You may use the resource as a guide, however, do not copy and paste, and don't type your code while looking at the resource. 
Only use the resource as a last resort. When you use it, flip between the resource and your text editor to help build your Reach programming muscles.

For this module, in the backend, build the skeleton of the participant interact interface and the skeleton of the Reach App initialization. In the frontend, create a starting balance, mirror the participant interact interface, and initialize the contract.

[Resource](https://github.com/algorand-devrel/Reach-Morra-Game/tree/main/morra0)

## Quiz

1. What method is used to initialize a Reach DApp?

1. One method that can be used to move from the "Step" mode to the "Consensus Step" mode is __________.

1. What method is used to share data with participants?

1. In cryptography, two of the most common placeholder names for participants are: _______ and ______.

1. True or False: functions in a `.only()` code block are available to all participants.