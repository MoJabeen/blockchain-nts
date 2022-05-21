Description: Overview of the terra crash and how the different elements of the system work. 

## When was terra founded ?

- Founded in 2018 by an economist and CS (Daniel Shin and Do Kwon)

- Built by Terraform labs (a singaporian software company)

## What is terra ?

-  Terra is an open source blockchain protocol, a platform for stable coins and dapps.

- Luna is terra's native coin. Used for staking, governance and mining.

### How do you mine on terra?

- Terra uses a POS system, the miners are called **validators** and validate blocks on full nodes.

- The **validators** reward is the transaction fee, based on the staked luna. They also have voting influnece on the governance of the blockchain.

- **Delegators** stake thier luna on behalf of a **validator** to earn staking rewards. The reward is then split to all delegators, with the **validator** taking a commission. 

- The process starts with a **Proposer** submit a new block to be validated by the validators, the proposer is rewarded for this role. 

- If a **validator** missbehaves a portion of their stake and the **delagtor** stake is **Slashed**.

*Terra has around 130 validators currently (you need to stake more luna than the bottom validator to be one) meaning it is currently quite centralised.* 

### How does Terra governance work?

- Each vote is counted by the amount of staked Luna.

- Validators vote with thier entire stake, unless explictly specified by the delegators.

- The proposer of the proposal must deposit luna, as the collateral to back the proposal. _Prevents spam_ !

- Propsal will pass if >40% of staked luna voted and the number of yes votes is a majority (>50%). The deposit is then refunded or burned.

## How does terra stable coin pegging work?

Context: Stable coins are assets pegged to the value of of another asset (normally a fiat currency).

- Terra uses an algorathm to mantain its peg, instead of the sole use of large cash reserves and debt. The benefit being it enables greater transparency and decentralisation.

- TerraUSD (UST) primaraly maintains its peg through arbitrageurs buying and selling terra's token LUNA. 

*Arbitrage: Buying and selling assets in different markets or forms to benefit from a different price.*

****

### How does arbitrage keep UST price pegged?

- One dollars worth of LUNA can be burned (removed from the blockchain) to mint 1 UST. And vice versa as shown in figure 1 below. 

![](UST-LUNA%20minting.png)

*Figure 1: UST<->LUNA minting*

- If the price of UST drops below its peg, this creates buying pressure on UST to mint into LUNA and sell for the profit. This buying pressure will increase its price as the supply drops.

- If the price of UST increases above its peg this creates buying pressure for Luna to be minted into UST and then sold for the profit. Increasing the supply of UST dropping the price.

### What is the death spiral?

- During a bear market when the price of LUNA is speculative, UST dropping below its beg will create sell pressure on LUNA (as people mint it to claim profit). This will further decrease the price of Luna.

- As Luna drops, UST holders fear increases creating sell demand, further incentivasing LUNA minting and selling. Causing further deacrease as supply increase, causing a spiral of death :(.

- Recently due to a number of unfortunatley timed events caused the luna market cap to drop below the UST market cap. Triggering a death spiral to 0.