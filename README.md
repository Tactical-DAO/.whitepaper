<p align="center">
  <img src="/assets/tactical.png" width="300" alt="Tactical DAO Banner">
</p>

# Tactical DAO

# About
We are a decentralized autonomous organization (DAO) that empowers holders with governance rights to propose treasury investments while incentivizing participation through dual staking pools.



## Table of Contents

1. [Tokenomics](#tokenomics)
2. [Contract Overview](#contract-overview)
3. [Testing](#testing)

## Tokenomics

- Token Name: Tactical DAO
- Ticker: $TACTICAL
- Total Supply: 2 Billion
- Total Supply Minted at Launch: 1 Billion

#### Uniswap Liquidity Pool: 500M TACTICAL (50% at launch, 75% dilutted over time)

- This forms the largest allocation to demonstrate clear alignment with community interests
- Large community ownership helps prevent concentration of voting power
- Shows commitment to decentralized governance from day one
- Size enables meaningful participation from various investor tiers

#### Treasury Management: 300M TACTICAL (30% at launch, 22.5% dilutted over time)

- Provides substantial liquidity for initial investment operations


#### Operations: 150M TACTICAL (15% at launch, 11.25% dilutted over time)

- Dedicated to sustainable operational runway
- Covers development, security audits, and platform maintenance
- Enables hiring of key personnel (analysts, risk managers, etc.)
- Provides buffer for market-making and liquidity management
- Funds ongoing protocol improvements and upgrades

#### Developer: 50M TACTICAL (5% at launch, 3.75% dilutted over time)

- Conservative founder allocation shows commitment to community ownership
- 3-year vesting period aligns with long-term success
- Size is sufficient to incentivize founding team without being excessive
- Comparable to traditional hedge fund management carry structures
- Gradual unlock prevents market pressure from founder selling

#### Summary

- Long-term sustainability over short-term gains
- Community ownership while maintaining operational efficiency
- Clear separation between investment capital and operational funds
- Professional management incentives similar to traditional finance
- Protection against concentration of power or market manipulation




## Tactical DAO (TACTICAL) Token and Staking System

A comprehensive DeFi system featuring dual staking pools with NFT-based governance capabilities.

## Contract Overview

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
- `MAX_SUPPLY`: 1,000,000,000 tokens (with 18 decimals)
- `INITIAL_SUPPLY`: 500,000,000 tokens (with 18 decimals)

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
- `REGULAR_STAKE_PERIOD`: 365 days

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

## Testing

1. **Test Environment Setup**
   - Get test ETH from a Sepolia faucet
   - Get test tokens (if using existing ERC20)
   - Connect MetaMask to Sepolia testnet

2. **Contract Testing in Remix**
   - Use the Remix interface to test each function
   - Click on deployed contract functions to test them
   - Monitor transactions in MetaMask
   - Check function results in Remix console

3. **Frontend Testing Steps**
   - Install Python 3, make sure install to path is enabled.
   - cd C:\Users\lbrya\Documents\Tactical DAO\root
   - python -m http.server 8000
   - http://localhost:8000/index.html

   
   Test the following flows:
   - Connect wallet
   - Fetch live coin prices
   - Fetch user wallet balances
   - Fetch treasury wallet balances
   - Submit proposal [in development]
   - Stake tokens [in development]
   - Unstake tokens [in development]
   - Claim rewards [in development]

4. **Common Test Cases**
   - Try to stake with insufficient balance
   - Submit proposal without minimum tokens
   - Test all read functions
   - Verify transaction success/failure handling

5. **Debugging in Remix**
   - Use Remix's Debug button on failed transactions
   - Check transaction details in the Remix console
   - Verify event emissions
   - Test contract state changes

## Troubleshooting Common Issues

1. **MetaMask Connection**
   - Ensure MetaMask is on the correct network
   - Reset MetaMask account if transactions are stuck
   - Clear transaction history if needed

2. **Contract Interaction**
   - Verify contract address is correct
   - Check ABI matches deployed contract
   - Ensure sufficient gas for transactions
   - Verify input parameters match function requirements

3. **Frontend Issues**
   - Check browser console for errors
   - Verify Web3 provider connection
   - Confirm contract instance creation
   - Validate transaction parameter formatting

## Next Steps

1. **Additional Features**
   - Add voting functionality
   - Implement token distribution
   - Add proposal execution
   - Enhance treasury management

2. **Production Deployment**
   - Deploy to mainnet
   - Set up monitoring
   - Implement security measures
   - Add transaction notifications


