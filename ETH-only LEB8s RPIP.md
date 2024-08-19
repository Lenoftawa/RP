
# Proposal: ETH-only LEB8 Minipools

---

## Abstract

This proposal introduces ETH-only LEB8 minipools, allowing Node Operators to create minipools without staking RPL. The aim is to address the challenges posed by RPL's volatility and maintain Rocket Pool's competitiveness in the face of Lido's CSM.

---

## Motivation

Rocket Pool has been experiencing a decline in minipool creation, primarily due to RPL's volatility and the requirement for a 10% RPL collateral to receive rewards. This has become less attractive due to both RPL's fluctuating value and the appreciation of ETH. Additionally, Lido's forthcoming CSM offers higher APR without needing to stake an additional token, potentially drawing Node Operators away from Rocket Pool. Given the extended timeline for Rocket Pool's Saturn 1 and 2 updates, there is a risk of losing Node Operators to Lido during this interim period.

To mitigate these risks and maintain competitiveness, it is crucial to consider an interim measure that loosens the RPL staking requirement without disrupting the development timeline for Saturn 1. The proposed solution should be simple to implement, not undermine the value of staking RPL, and avoid negatively impacting related protocols like Nodeset and RocketLend.

---

## Considerations

1. ### Nodeset and RocketLend  
   The proposed changes should preserve the incentive for Node Operators to stake RPL, as these protocols rely on RPL staking for income generation.

2. ### Saturn  
   Overly generous rewards for ETH-only LEB8s might diminish the appeal of transitioning to Saturn minipools once they become available.

3. ### RPL Value  
   The reward system should avoid encouraging the selling of RPL or unnecessary churn in minipool creation.

4. ### Exit and Re-enter Churn  
   The proposal aims to minimize incentives for Node Operators to close existing minipools only to create new ETH-only LEB8s, which could lead to inefficiencies.

---

## Dynamic Commission Proposal

A key feature of this proposal is the introduction of ETH-only LEB8s, where Node Operators can create minipools without the requirement for additional RPL. To address concerns about under-collateralized Node Operators and the incentive structure, the commission rate for new ETH-only LEB8s would be set lower than the standard 14%, with an additional ETH bonus tied to the amount of RPL held. This bonus would be distributed from the Smoothing Pool via the rewards tree calculation.

While this approach should attract more staked ETH, it also considers the potential negative impact on RPL value. The proposal recommends revisiting the current 10% minimum RPL requirement for earning RPL rewards to provide relief to under-collateralized nodes and incentivize the purchase of RPL for ETH-only LEB8s. The exact parameters of this change are yet to be determined.

---

### ETH Commission

The commission for ETH-only LEB8s should be set between 5% and 10%, encouraging Node Operators who are hesitant to stake RPL to participate while maintaining some incentive to stake RPL. A higher commission could erode the advantage of Lido's CSM, but it may also lead to churn where Node Operators exit and re-enter minipools without staking RPL.

To further incentivize RPL staking, an ETH bonus would be paid based on the amount of RPL held, ensuring it does not exceed the 14% commission earned by fully collateralized LEB8s. This structure may also encourage bond reduction in existing EB16s and create opportunities for RocketLend.

---

### RPL Rewards

RPL rewards are distributed on a node-wide basis, unlike commission, which is paid per minipool. The current RPL cliff, which incentivizes Node Operators to exit minipools rather than top up their RPL, should be reassessed. The goal is to balance the incentives for topping up RPL, minimizing exits due to RPL exhaustion, and encouraging new Node Operators to purchase RPL.

---

## Implementation Constraints

To avoid the risks associated with smart contract modifications, the reward system should be managed off-chain via the reward tree generation process. Dynamic commission distribution can be achieved by setting a lower on-chain commission rate for ETH-only LEB8s and providing an ETH top-up based on RPL holdings through the Smoothing Pool.

---

## Specification

1. ### LEB8 Minipool Creation
   - All LEB8s created post-implementation will follow the ETH-only LEB8 structure.
   - If a Node Operator is under-collateralized and not part of the Smoothing Pool, Smartnode will issue a warning about the potential loss of bonus ETH.
   - Documentation will highlight the risks of opting out of the Smoothing Pool when creating under-collateralized LEB8 minipools.

2. ### Commission Calculation
   - The commission calculation remains unchanged, but the rate assigned to each minipool at creation time will differ.
   - The commission for LEB8s will be set to A%.

3. ### Smoothing Pool Distribution
   - The Smoothing Pool balance will be distributed during the reward tree calculation process to those who have opted in.
   - The bonus will be available to Node Operators using the existing claim interface.
   - The Smoothing Pool will distribute based on the node's borrowed-ETH/RPL ratio (ratio) with the following rules:
     - The minimum eligibility level will be B% RPL collateral ratio.
     - The maximum eligibility level will be C% RPL collateral ratio.
     - The minimum Smoothing Pool bonus percentage will be D%-A%.
     - The maximum Smoothing Pool bonus percentage will be E%-A%.
     - The Smoothing Pool bonus percentage will scale linearly from D% to E%.
     - The Smoothing Pool bonus for a specific minipool will be calculated where the node's RPL collateral ratio intersects the line from D% to 14%.

