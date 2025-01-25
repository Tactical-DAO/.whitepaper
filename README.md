<p align="center">
  <img src="/assets/tactical.png" width="300" alt="Tactical DAO Banner">
</p>

<div align="center">

# Tactical DAO

</div>

## Table of Contents
1. [About](#about)
2. [Contract Overview](#contract-overview)


<br>
<br>
<br>
<br>


# About
Tactical DAO is building the future of esports ownership, enabling community governance and shared success in professional Counter-Strike.
Our vision is to establish the first successful community-owned professional Counter-Strike team, setting new standards for esports organization governance and ownership.


<br>
<br>
<br>
<br>







## Contract Overview

 A comprehensive DeFi system featuring dual staking pools with NFT-based governance capabilities.
 
The system consists of four main contracts:
1. TacticalToken (ERC20)
2. FoundersNFT (ERC721)
3. TacticalStaking
4. TacticalGovernance

## Detailed Contract Documentation

### TacticalToken Contract
```solidity
contract TacticalToken is ERC20, Ownable
```
Standard ERC20 implementation with maximum supply control.

#### Constants
- `MAX_SUPPLY`: 2,000,000,000 tokens (with 18 decimals)
- `INITIAL_SUPPLY`: 1,000,000,000 tokens (with 18 decimals)

#### Functions
- `constructor()`: Initializes the token with name "Tactical DAO" and symbol "TACTICAL", mints initial supply
- `mint(address to, uint256 amount)`: Allows owner to mint additional tokens up to MAX_SUPPLY

### FoundersNFT Contract
```solidity
contract FoundersNFT is ERC721, Ownable
```
NFT contract representing founder stake participation.

#### State Variables
- `_tokenIds`: Counter for NFT IDs
- `tokenStaker`: Mapping of NFT ID to staker address

#### Functions
- `constructor()`: Initializes the NFT with name "Tactical DAO Founder NFT" and symbol "fTACTICAL"
- `mintNFT(address staker)`: Mints new NFT to staker (only callable by staking contract)

### TacticalStaking Contract
```solidity
contract TacticalStaking is Ownable, ReentrancyGuard
```
Core staking functionality with dual pool system.

#### Constants
- `REGULAR_APR`: 20% (2000 basis points)
- `FOUNDER_APR`: 100% (10000 basis points)
- `REGULAR_STAKE_PERIOD`: 30 days

#### Structures
```solidity
struct RegularStake {
    uint256 amount;
    uint256 timestamp;
    uint256 lastClaimTimestamp;
}

struct FounderStake {
    uint256 amount;
    uint256 timestamp;
    uint256 lastClaimTimestamp;
    uint256 nftId;
}
```

#### State Variables
- `regularStakes`: Mapping of address to regular stake info
- `founderStakes`: Mapping of address to founder stake info

#### Regular Staking Functions
- `stakeRegular(uint256 amount)`: 
  - Stakes tokens in regular pool
  - Requires minimum 100 tokens
  - 365-day lock period
  - 20% APR

- `unstakeRegular()`:
  - Withdraws regular stake + rewards
  - Only after lock period
  - Combines principal and rewards

- `claimRegularRewards()`:
  - Claims pending regular stake rewards
  - Resets reward timer
  - Transfers rewards to user

- `reinvestRegularRewards()`:
  - Compounds regular stake rewards
  - Adds rewards to principal
  - Resets reward timer

#### Founder Staking Functions
- `stakeFounder(uint256 amount)`:
  - Stakes tokens in founder pool
  - Mints governance NFT on first stake
  - Permanent lock
  - 100% APY

- `claimFounderRewards()`:
  - Claims pending founder stake rewards
  - Resets reward timer
  - Transfers rewards to user

- `reinvestFounderRewards()`:
  - Compounds founder stake rewards
  - Adds rewards to principal
  - Resets reward timer

#### View Functions
- `calculateRegularRewards(address staker)`: Calculates pending regular rewards
- `calculateFounderRewards(address staker)`: Calculates pending founder rewards
- `getRegularStakeInfo(address staker)`: Returns regular stake details
- `getFounderStakeInfo(address staker)`: Returns founder stake details

#### Events
- `RegularStaked(address indexed user, uint256 amount)`
- `FounderStaked(address indexed user, uint256 amount, uint256 nftId)`
- `RegularUnstaked(address indexed user, uint256 amount)`
- `RegularRewardsClaimed(address indexed user, uint256 amount)`
- `FounderRewardsClaimed(address indexed user, uint256 amount)`
- `RegularRewardsReinvested(address indexed user, uint256 amount)`
- `FounderRewardsReinvested(address indexed user, uint256 amount)`

### TacticalGovernance Contract
```solidity
contract TacticalGovernance
```
Basic governance system for founder NFT holders.

#### Structures
```solidity
struct Proposal {
    string description;
    uint256 forVotes;
    uint256 againstVotes;
    mapping(address => bool) hasVoted;
    bool executed;
    uint256 deadline;
}
```

#### Functions
- `createProposal(string memory description, uint256 votingPeriod)`:
  - Creates new governance proposal
  - Only founder NFT holders
  - Sets voting deadline

- `vote(uint256 proposalId, bool support)`:
  - Casts vote on proposal
  - Only founder NFT holders
  - One vote per proposal per holder
  - Must vote before deadline

## Security Features

1. ReentrancyGuard implementation
2. SafeERC20 usage
3. Ownership controls
4. Clear access control
5. Event emission for tracking
6. Deadline enforcement
7. Duplicate vote prevention

## Deployment Steps

1. Deploy TacticalToken
2. Deploy FoundersNFT
3. Deploy TacticalStaking with token and NFT addresses
4. Deploy TacticalGovernance with NFT address
5. Transfer FoundersNFT ownership to TacticalStaking
6. Transfer initial token supply to deployment wallet

## Notes

- All rewards are calculated in real-time
- Regular stakes have 365-day lock period
- Founder stakes are permanently locked
- Governance voting is restricted to founder NFT holders
- Rewards can be claimed or reinvested at any time
- Separate reward tracking for each pool


## Next Steps

1. **Additional Features**
   - Add voting functionality
   - Add staking functionality
   - Add proposal functionality
   - Enhance website functionality




