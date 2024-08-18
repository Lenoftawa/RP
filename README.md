# ETH-only LEB8s 
## Motivation
Rocketpool is losing minipools.  This is believed to be because RPL is not attractive due to it's volatility and the need for 10% collateral in order to enter and receive rewards.  This original tokenomics required a minimum 10% of borrowed ETH in RPL in order to 

- spin up a new minipool 
- receive RPL rewards from inflation and 
- incentivise 'topping-up' to induce buy-pressure when the token price falls. 

Unfortunately, the volatility of RPL with respect to ETH has made this ineffective.  That's not just due to the low value of RPL but also the appreciation of ETH. 

Additionally, the prospective arrival of Lido's CSM has changed the competitive landscape by offering significantly higher APR without the need for staking an additional token.  This is achieved by requiring much lower ETH collateral. The prospect of CSM attracting Node Operators away from Rocketpool prompted the recent rework of the Rocketpool tokenomics, to to be delivered in Saturn 1 and 2.

Finally, the development of the tokenomics re-work in Saturn 1 and 2 is significant and cannot compete with Lido's CSM delivery timeframe.  This means that there will be a significant period when the incentive will be for existing Node Operators to migrate to CSM for the considerable ETH APR advantage.  Moreover, CSM will likely be preferred by new Node Operators that have been on the sidelines because they are reluctant to be exposed to RPL volatility. While Saturn will turn the tables with respect to competitive returns, the interim loss of NOs will mean having to claw them back rather than build on Rocketpool's current position. 

In sum, these concerns suggest that an interim measure must be given serious consideration, and the obvious solution is to loosen the requirement for RPL staking. The changes required must also:

- be minimal so that they can be implemented safely and expeditiously 
- not interfere with the timeline for Saturn 1
- maintain the advantage of staking RPL
- not adversely affect the viability of related protocols that will also aim to solve the 'RPL requirement' problem (namely Nodeset and RocketLend).

## Considerations

### Nodeset and RocketLend

It is important that any solution does not remove the incentive to stake RPL as these protocols are designed to allow RPL holders to receive income from locking RPL in Rocketpool.

### Saturn

If the rewards are too high, then there will be no motivation to move to Saturn minipools when they become available.

### RPL value

The reward system cannot encourage exiting and dumping RPL.

### Exit and re-enter churn

Ideally the reward system should not favour closing existing minipools and spinning up new LEB8s. Although this is neutral in terms of ETH and rETH.

## A Dynamic Commission Proposal

After considerable discussion, a proposal has emerged around 'ETH-only LEB8s', where the RPL requirement for creating LEB8s and receiving ETH commission is removed. See [Discord thread](https://discord.com/channels/405159462932971535/1267421248288198677) for the full discussion.
### ETH-only LEB8s

The proposal would allow the creation of new LEB8s without the requirement for additional RPL.

LEB8s already exist, so little if any Smart Contract work would be required. Removing the requirement for RPL allows:

- new Node Operators to create minipools without RPL stake, 
- existing Node Operators with idle ETH to add minipools and 
- Node Operators with EB16s to bond-reduce.

ETH-only LEB8s should attract more staked ETH. However, unless the commission rate is lower than 14%, there would be an incentive for under-collateralised Node Operators to exist and re-enter resulting in a huge churn and no incentive for new Node Operators to stake RPL at all. To remedy this, the commission rate for new operators would be set lower and a bonus in ETH available in proportion to RPL held.  For implementation reasons, the base commission will be distributed directly from consensus rewards, while the ETH bonus will be distributed from the Smoothing Pool via the rewards tree calculation. A positive side effect of the reduced commission is that rETH holders may pay a slightly lower overall commission.

While the commision distribution should help reduce the imbalance between Node operator supply and rETH demand, it does not solve the RPL value concern. The incentive to 'top-up' has hit a tipping point where few Node Operators are able or inclined to do so. Some Node Operators have chosen to switch to EB16s while others have exited minipools in order to receive RPL rewards. In extrme cases Node Operators have exited their nodes all-together

To reverse this trend, the proposal recommends that the current 10% minimum RPL for earning RPL rewards (the cliff) should be modified to:

- give some relief to under-collateralised nodes and prevent them from exiting minipools or migrating to EB16s.
- give ETH-only LEB8s an incentive to buy RPL.
- limit the reward dilution for fully collateralised nodes as much as practical.

The exact shape of this change is **TBD**.

NOTE: The RPL reward adjustment should not be confused with removal of the RPL requirement for creation of LEB8s and receiving bonus ETH rewards.  

### ETH Commission

The commission for ETH-only LEB8s must be less than that of fully collateralised LEB8s and must also encourage the holding of RPL.

The proposal suggests a setting of between 5% and 10% ETH commission; this figure still needs to be fully determined. The advantage of a high ETH commission is to encourage RPL-reluctant NOs to join and erode the advantage of CSM.  On the other hand it could compete with Saturn 1 and the lost comparative advantage of staked RPL may cause unproductive churn where Node Operators exit and re-enter some of their minipools.

To encourage ETH-only LEB8s to stake, a bonus in ETH would be paid based on RPL holding. Clearly, this bonus cannot exceed the 14% commission earned by a fully collateralised LEB8. While new node operators may not have a limited appetite for RPL, it will incentivise Bond-reduction of existing EB16s and possibly create a market for RocketLend. 

### RPL Rewards

RPL rewards are determined node-wide, in contrast to commission, which is paid per minipool. So any change to distribution would need to work with old and new LEB8 minipools.

The RPL cliff has been identified as an incentive to exit minipoools rather than top-up, so it makes sense to review this. This is a more contentious feature in that there is a tension between creating:
- incentive to top-up, with resulting in RPL buy pressure 
- incentive to exit minipools due to to-up exhaustion, with resulting loss of ETH
- incentive for new NOs to buy RPL
- incentive for under-collaterlised existing NOs to add RPL
- opportunity for NOs to use RocketLend


### Implementation Constraints

To avoid the risks associated with smart contract changes, the reward system should be handled by offchain reward tree generation. The commission paid to minipools is currently calculated onchain and is a constant value (14%).  To distribure dynamic commission without changes to the smart contract, ETH-only LEB8s will have a lower percentage distributed onchain and an ETH top-up based on RPL staked will be taken from the smoothing pool as part of reward tree generation. RPL rewards are currently computed offchain and therefore are not as constrained. 

## Specification

### LEB8 minipool Creation

All LEB8s created after this change, will inherit ETH-only LEB8 characteristics.

If a Node Operator under-collaterlaised and has not opted into the smoothing pool, then when they create an LEB8 minipool, Smartnode will warn them that they not receive bonus ETH for holding RPL.

Documentation will to warn the user of the protential loss of income from a decision to opt out of the smoothing pool when creating an LEB8 minipool when under-collateralised. 

### Commission calculation

Commission calculation will remain unchanged.  The commission will be distributed according to the rate assigned to each minipool at creation time.

![image](https://github.com/user-attachments/assets/9faa5491-2ba7-46f1-b8ec-2fddd9f5e82b)


- The commission for LEB8s will be A%

### Smoothing pool distribution

The smoothing pool balance will only be distributed as part of the reward tree calculation process to those that have opted in.

The bonus will be available to Node Operators using the existing claim interface. 

The smoothing pool will be distributed based on borrowed-ETH/RPL ratio (`ratio`) of the whole node according to the rules:

- The minimum eligibility level will be B% RPL collateral ratio, where B is greater than or equal to 0 and less than C.
- The maximum eligibility level will be C% RPL collateral ratio where C cannot be greater than 15.
- The minimum smoothing pool bonus percentage will be D%-A% where D is a value in the range A to 14.
- The maximum smoothing pool bonus percentage will be E%-A% where E must be less than or equal to 14
- the smoothing pool bonus percentage will scale linearly from D% to E%
- the smoothing pool bonus for a specific minipool will be where the nodes RPL collateral ratio intersects the line from D% to 14%

  ...... etc .....
