# Week 2
# Bets and Wagers

## Knowledge Reinforcement

In this unit, you'll become acquainted with creating currency for test accounts and using the pay and transfer methods.
 
These methods are critical to move between the `step` and `Consensus step` modes of a Reach Application.

### Definitions & Concepts

Terms for this unit include:

formatCurrency()

parseCurrency()

pay()

transfer()

### Currency

**formatCurrency**

formatCurrency() displays amounts in ___A_____ units. These units are ____B____.

A: standard or atomic

B: divisible or indivisible

**parseCurrency**

parseCurrency() displays amounts in ____A____ units, which are ____B____.

A: standard or atomic

B: divisible or indivisible

In Ethereum, a ______ unit is called an ETH and an _______ unit is called a WEI.

**pay()**

_`pay()` enables the Reach App to move from the_ _____ _mode to the_ ______ _mode._

--Local Step ==> Step

--App init ==> Step

--Consensus Step ==> Local Step

--Step ==> Consensus Step

---
#### _Code it_
Review the Buyer `only()` block you created last week. Add a parameter, `payment`, to the `publish` method and attach a `pay` method to it.

---

**transfer**

_`transfer()` is a method within which mode of a Reach app?_

--Step

--Local Step

--Consensus Step

What's wrong with `transfer()` in this block:

``` rsh
  const outcome = (handAlice + (4 - handBob)) % 3;
  const            [forAlice, forBob] =
    outcome == 2 ? [       2,      0] :
    outcome == 0 ? [       0,      2] :
    /* tie      */ [       1,      1];
  transfer(forBob * wager).to(Alice);
  transfer(forBob * wager).to(Alice);
  commit();
```

## Independent Project
Type out the starter code below and then complete the steps under the "[Explore units and balances](https://github.com/TheChronicMonster/reach-developer-portal/blob/master/en/books/essentials/getting-started/wisdom-for-sale/index.md#explore-units-and-balances)" section to better understand atomic and standard units.

`index.rsh`
``` rsh
'reach 0.1';

const commonInteract = {};
const sellerInteract = { ...commonInteract };
const buyerInteract = { ...commonInteract };

export const main = Reach.App(() => {
  const S = Participant('Seller', sellerInteract);
  const B = Participant('Buyer', buyerInteract);
  init();

  exit();
})
```

`index.mjs`
``` mjs
import {loadStdlib } from '@reach-sh/stdlib';
import * as backend from './build/index.main.mjs';

const role = 'seller';
console.log(`Your role is ${role}.`);

const stdlib = loadStdlib(process.env);
console.log(`The consensus network is ${stdlib.connector}.`);

(async () => {
  const commonInteract = {};

  // SELLER
  if (role === 'seller') {
    const sellerInteract = { ...commonInteract };

    const acc = await stdlib.newTestAccount(stdlib.parseCurrency(1000));
    const ctc = acc.contract(backend);
    await backend.Seller(ctc, sellerInteract);
  }

  // BUYER
  else {
    const buyerInteract = { ...commonInteract };
  }
})();
```

## Quiz

1. What new method is introduced in this unit that moves from the "Step" mode to the "Consensus Step" mode?

1. What method is used to move funds from one participant to another?

1. In a Reach program, currency should be passed as ________ units.

1. What method is used to move from the "Consensus Step" mode to the "Step" mode?

1. Up to how many decimals will be displayed per this statement:
`const fmt = (x) => stdlib.formatCurrency(x, 8);`
