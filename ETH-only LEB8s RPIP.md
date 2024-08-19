
# RPIP-XX: ETH-only LEB8 Minipools

## Abstract

This proposal introduces ETH-only LEB8 minipools, allowing Node Operators to create minipools without staking RPL. The aim is to address the challenges posed by RPL's volatility and maintain Rocket Pool's competitiveness in the face of Lido's CSM.

## Motivation

Rocket Pool has been experiencing a decline in minipools, primarily due to RPL's volatility and the requirement for a 10% RPL collateral to receive rewards and create new minipools. Additionally, Lido's forthcoming CSM offers higher APR without needing to stake an additional token, potentially drawing Node Operators away from Rocket Pool. Given the extended timeline for Rocket Pool's Saturn 1 and 2 updates, there is a risk of losing Node Operators to Lido CSM during this interim period.

To maintain competitiveness, it is crucial to consider some form of interim measure that loosens the RPL staking requirement.

## Considerations
The solution must not interfere with the timeline for Saturn 1, must be simple to implement, must not undermine the value of staking RPL, and should avoid negatively impacting layer 2 RocketPool protocols like Nodeset and RocketLend.

### Nodeset and RocketLend  
Layer 2 RocketPool protocols have been designed to function within the current tokenomics, so any changes should maintain demand for rETH and preserve the incentive for Node Operators to stake RPL.

### Saturn  

The rewards for ETH-only LEB8s cannot be overly generous, as this will diminish the appeal of transitioning to Saturn Megapools once they become available.

The delivery schedule for Saturn 1 is already challenging, in fact it is the primary motivation for this proposal. Delivering this interim measure cannot be allowed to intefere with Saturn development.

### RPL Value  
The reward system should avoid encouraging the selling of RPL or unnecessary churn in minipool creation.

### Exit and Re-enter Churn  
The proposal needs to minimize incentives for Node Operators to close existing minipools only to create new ETH-only LEB8s, as this would be distruptive and not be beneficial to the Rocketpool protocol.

## Dynamic Commission Proposal

A key feature of this proposal is the introduction of ETH-only LEB8s, where Node Operators can create minipools without the requirement for additional RPL. The commission rate for new ETH-only LEB8s would be set lower than the current 14%, with an additional ETH bonus tied to the amount of RPL held. The total ETH reward (comission plus bonus) would be capped at 14% to avoid competing with existing LEB8s. This bonus would be distributed from the Smoothing Pool via the rewards tree calculation.

While this approach should attract more staked ETH, it also considers the potential negative impact on RPL value. The proposal recommends revisiting the current 10% minimum RPL requirement for earning RPL rewards. This will provide relief to under-collateralized nodes and incentivize the purchase of RPL for ETH-only LEB8s. The exact parameters of this change are yet to be determined.

### ETH Commission

The commission for ETH-only LEB8s should be set between 5% and 10%, encouraging Node Operators who are hesitant to stake RPL to participate while maintaining some incentive to stake RPL. A higher commission could erode the advantage of Lido's CSM, but it may also lead to churn where Node Operators exit and re-enter minipools without staking RPL.

To further incentivize RPL staking, an ETH bonus would be paid based on the amount of RPL held, ensuring it does not exceed the 14% commission earned by fully collateralized LEB8s. This structure may also encourage bond reduction in existing EB16s and create opportunities for RocketLend.

### RPL Rewards

RPL rewards are distributed on a node-wide basis, unlike commission, which is paid per minipool. The current RPL cliff, which incentivizes Node Operators to exit minipools rather than top up their RPL, should be reassessed. The goal is to balance the incentives for topping up RPL, minimizing exits due to RPL exhaustion, and encouraging new Node Operators to purchase RPL.


## Implementation Constraints

To avoid the risks associated with smart contract modifications, the reward system should be managed off-chain via the reward tree generation process. Dynamic commission distribution can be achieved by setting a lower on-chain commission rate for ETH-only LEB8s and providing an ETH top-up based on RPL collateral ratio through the Smoothing Pool.

## Specification
The ETH reward distribution for ETH-only LEB8s can be described in terms of a number of parameters (A to E) as shown in the chart below. 
![image](https://github.com/user-attachments/assets/cb007835-4c50-4703-9f12-3103d0c0b9ea)

_(not to scale)_

These parameters can be set to define the `commission rate` as well as the Smoothing Pool `distribution rule`. 

1. ### LEB8 Minipool Creation
   - All LEB8s created post-implementation will follow the ETH-only LEB8 structure.
   - If a Node Operator creates an LEB8 minipool and has not opted into the Smoothing Pool, Smartnode will issue a warning about the potential loss of bonus ETH.
   - Documentation will highlight the risks of opting out of the Smoothing Pool when creating LEB8 minipools.

2. ### Commission Calculation

   - The commission rate for new LEB8s will be set to A%.
   - The commission rate for existing minipools will remain unchanged.
   - The commission calculation remains unchanged, but the rate assigned to each minipool at creation time will differ.

3. ### Smoothing Pool Distribution
   - The Smoothing Pool balance will be distributed during the reward tree calculation process.
   - The Smoothing Pool balance will be only be distributed to nodes that have opted into the Smoothing Pool.
   - The bonus will be available to Node Operators using the existing claim interface.
   - The proportion of ETH distributed to each minipool will be based on the node's borrowed ETH/RPL ratio (`ratio`) by applying the following `distribution rule`:
     - The minimum eligibility level will be B% RPL collateral `ratio`.
     - The maximum eligibility level will be C% RPL collateral `ratio`.
     - The minimum Smoothing Pool ETH bonus percentage will be D%-A%.
     - The maximum Smoothing Pool ETH bonus percentage will be E%-A%.
     - The Smoothing Pool ETH bonus percentage will scale linearly from D% to E%.
     - The Smoothing Pool bonus for a specific minipool will be calculated where the node's RPL collateral `ratio` intersects the line from D% to 14%.
  - If the Smoothing pool balance is insufficient to distribute the full top-up according to the `distribution rule`, a `distribution adjustment` will be applied as follows:
     - Something simple.
  - The parameter settings for 'commission rate' and Smoothing Pool `distribution rule` will be:
      - A = 7
      - B = 0
      - C = 10
      - D = 7
      - E = 14
   
      These parameter settings can be expressed diagramatically as:
   
   ![image](https://github.com/user-attachments/assets/8c75ea5c-c3a6-420c-b177-565d4b81880b)


_(not to scale)_
