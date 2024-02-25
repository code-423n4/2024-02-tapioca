
# Tapioca DAO Invitational audit details
- Total Prize Pool: $138,000 in USDC
  - HM awards: $119,450 in USDC
  - QA awards: $2,775 in USDC
  - Gas awards: $2,775 in USDC
  - Judge awards: $12,500 in USDC
  - Scout awards: $500 in USDC
 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2024-02-tapioca-invitational/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts February 26, 2024 20:00 UTC
- Ends March 18, 2024 20:00 UTC

## This is a Private audit

This audit repo and its Discord channel are accessible to **certified wardens only.** Participation in private audits is bound by:

1. Code4rena's [Certified Contributor Terms and Conditions](https://github.com/code-423n4/code423n4.com/blob/main/_data/pages/certified-contributor-terms-and-conditions.md)
2. C4's [Certified Contributor Code of Professional Conduct](https://code4rena.notion.site/Code-of-Professional-Conduct-657c7d80d34045f19eee510ae06fef55)

*All discussions regarding private audits should be considered private and confidential, unless otherwise indicated.*

Please review the following confidentiality requirements carefully, and if anything is unclear, ask questions in the private audit channel in the C4 Discord.


## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-02-tapioca-dao/blob/main/4naly3er-report.md).


Prior audits can be viewed [here](https://docs.tapioca.xyz/tapioca/information/audits-and-partners), and the contents of these are also considered known issues and ineligible for awards. It is recommended that wardens read both Certora reports for helpful context. 


_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

[ ⭐️ SPONSORS: Are there any known issues or risks deemed acceptable that shouldn't lead to a valid finding? If so, list them here. ]


# Overview

The Tapioca protocol is built with a lot of different smart contracts, scattered across 5 repositories.
It's an _Omnichain_ protocol working the LayerZero messaging layer. At its core, Tapioca ERC20/ERC721 contracts uses the LayerZero contracts and infrastructure.


The other repos are here to support the ecosystem as well as to create a synergy between the tokenemics and the protocol features.

* [TapiocaBar](https://github.com/Tapioca-DAO/Tapioca-bar) Core logic of the protocol. This is where `USDO` gets minted and lent.
* [tapiocaz](https://github.com/Tapioca-DAO/TapiocaZ) Contracts that contains a wrapper named `TOFT`, which is used to wrap gas tokens and transfer allow their usage through the LayerZero network.
* [YieldBox](https://github.com/Tapioca-DAO/tap-yieldbox) A "BentoBox v2". Acts as a vault, that allow for yield strategies to be applied on the asset.
* [yieldbox-strategies](https://github.com/Tapioca-DAO/tapioca-yieldbox-strategies) Yield strategies that will be used by a YieldBox asset.



# Notes
- The docs provide a lot of information about the protocol and the user flow, given the size of the protocol, we encourage checking it at https://docs.tapioca.xyz/tapioca/.



## Links

- **Previous audits: https://docs.tapioca.xyz/tapioca/information/security-and-integrations#audits** 
- **Documentation: https://docs.tapioca.xyz/tapioca**
- **Website: https://www.tapioca.xyz**
- **Twitter: https://twitter.com/tapioca_dao** 
- **Discord: https://discord.com/invite/tapiocadao** 


# Scope

[ ⭐️ SPONSORS: add scoping and technical details here ]

- [ ] In the table format shown below, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each *For line of code counts, we recommend running prettier with a 100-character line length, and using [cloc](https://github.com/AlDanial/cloc).* 
  - [ ] external contracts called in each
  - [ ] libraries used in each


### tap-token
| Type     | File                                                    | Description                                                                                           | nSLOC    | Complex. Score | Capabilities                                                                                                                                                                                                                                                 |
|----------|---------------------------------------------------------|-------------------------------------------------------------------------------------------------------|----------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 📝       | ./contracts/erc721NftLoader/ERC721NftLoader.sol         | Load URIs for ERC721 contracts.                                                                       | 19       | 19             | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/governance/twTAP.sol                        | ERC721 governance token. Users can lock `$TAP` and receive rewards.                                   | 324      | 210            | **<abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='Uses Hash-Functions'>🧮</abbr><abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                   |
| 📝       | ./contracts/option-airdrop/AirdropBroker.sol            | `aoTAP` minter. Users can participate on 4 different phases to receive and exercise `aoTAP`.          | 287      | 251            | **<abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='Uses Hash-Functions'>🧮</abbr><abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                   |
| 📝       | ./contracts/option-airdrop/LTap.sol                     | 1:1 `$TAP` swapper. Used for the Tapioca LBP.                                                         | 31       | 30             | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/option-airdrop/aoTAP.sol                    | Airdrop TAP. Used only by `AirdropBroker`.                                                            | 65       | 56             | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/options/TapiocaOptionBroker.sol             | `oTAP` minter. Users can participate by locking `Singularity` positions, receive and exercise `oTAP`. | 356      | 254            | **<abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                                                                  |
| 📝       | ./contracts/options/TapiocaOptionLiquidityProvision.sol | Middleman contract for TapiocaOptionBroker. Users can create Singularity lock.                        | 230      | 185            | **<abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                              |
| 📝       | ./contracts/options/oTAP.sol                            | Option TAP.                                                                                           | 54       | 45             | ****                                                                                                                                                                                                                                                         |
| 🎨       | ./contracts/options/twAML.sol                           | Math logic for `AirdropBroker` and `twTAP`.	                                                          | 79       | 109            | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                             |
| 🎨       | ./contracts/tokens/BaseTapToken.sol                     |                                                                                                       | 19       | 16             | ****                                                                                                                                                                                                                                                         |
| 🎨       | ./contracts/tokens/BaseTapTokenMsgType.sol              |                                                                                                       | 6        | 4              | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/tokens/TapToken.sol                         | `$TAP` contract. Inherit from `TapiocaOmnichainEngine`.                                               | 186      | 163            | **<abbr title='Payable Functions'>💰</abbr><abbr title='Uses Hash-Functions'>🧮</abbr>**                                                                                                                                                                     |
| 📚       | ./contracts/tokens/TapTokenCodec.sol                    | Encode/decode `$TAP` LayerZero calls.                                                                 | 50       | 19             | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/tokens/TapTokenReceiver.sol                 | Receive LZ calls.                                                                                     | 93       | 47             | **<abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                              |
| 📝       | ./contracts/tokens/TapTokenSender.sol                   | Send LZ calls.                                                                                        | 12       | 6              | ****                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/tokens/Vesting.sol                          | Vesting contract.                                                                                     | 134      | 100            | **<abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                                                                  |
| 📝       | ./contracts/tokens/extensions/TapTokenHelper.sol        | Helps create `$TAP` calls.                                                                            | 27       | 15             | ****                                                                                                                                                                                                                                                         |
| 🎨       | ./contracts/tokens/module/ModuleManager.sol             | Modularize `TapToken` contract into multiple contracts, called by delegate calls.                     | 29       | 19             | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='DelegateCall'>👥</abbr>**                                                                                                                                                                                |
| 📝📚🔍🎨 | **Totals**                                              |                                                                                                       | **2052** | **1500**       | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='Payable Functions'>💰</abbr><abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='DelegateCall'>👥</abbr><abbr title='Uses Hash-Functions'>🧮</abbr><abbr title='Unchecked Blocks'>Σ</abbr>** |


### tapioca-periph
| Type     | File                                                                          | Description                                                                                                                                          | nSLOC    | Complex. Score | Capabilities                                                                                                                                                                                                                                                                                       |
|----------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 📝       | ./contracts/Cluster/Cluster.sol                                               | Whiteliste addresses from different LayerZero supported chains.                                                                                      | 55       | 38             | ****                                                                                                                                                                                                                                                                                               |
| 📚🔍     | ./contracts/libraries/SafeApprove.sol                                         | Safe approval util.                                                                                                                                  | 15       | 14             | ****                                                                                                                                                                                                                                                                                               |
| 📝       | ./contracts/Magnetar/BaseMagnetar.sol                                         |                                                                                                                                                      | 36       | 35             | ****                                                                                                                                                                                                                                                                                               |
| 📝       | ./contracts/Magnetar/Magnetar.sol                                             | Batching contract that initiate calls to multiple external core contracts such as `YieldBox`, `Singularity`, `BigBang`, `tOFT/mtOFT`, `USDO`, `TAP`. | 182      | 99             | **<abbr title='Payable Functions'>💰</abbr><abbr title='DelegateCall'>👥</abbr>**                                                                                                                                                                                                                  |
| 📝       | ./contracts/Magnetar/MagnetarHelper.sol                                       | View helper for `Magnetar`.                                                                                                                          | 194      | 141            | ****                                                                                                                                                                                                                                                                                               |
| 📝       | ./contracts/Magnetar/MagnetarStorage.sol                                      |                                                                                                                                                      | 58       | 46             | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='Payable Functions'>💰</abbr><abbr title='DelegateCall'>👥</abbr>**                                                                                                                                                                             |
| 📝       | ./contracts/Magnetar/modules/MagnetarAssetCommonModule.sol                    | `Magnetar` module.                                                                                                                                   | 60       | 36             | ****                                                                                                                                                                                                                                                                                               |
| 📝       | ./contracts/Magnetar/modules/MagnetarAssetModule.sol                          | `Magnetar` module.                                                                                                                                   | 73       | 60             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 📝       | ./contracts/Magnetar/modules/MagnetarAssetXChainModule.sol                    | `Magnetar` module.                                                                                                                                   | 55       | 21             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 🎨       | ./contracts/Magnetar/modules/MagnetarBaseModule.sol                           | `Magnetar` module.                                                                                                                                   | 138      | 94             | **<abbr title='create/create2'>🌀</abbr>**                                                                                                                                                                                                                                                         |
| 📝       | ./contracts/Magnetar/modules/MagnetarCollateralModule.sol                     | `Magnetar` module.                                                                                                                                   | 51       | 49             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 🎨       | ./contracts/Magnetar/modules/MagnetarMintCommonModule.sol                     | `Magnetar` module.                                                                                                                                   | 138      | 105            | ****                                                                                                                                                                                                                                                                                               |
| 📝       | ./contracts/Magnetar/modules/MagnetarMintModule.sol                           | `Magnetar` module.                                                                                                                                   | 30       | 18             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 📝       | ./contracts/Magnetar/modules/MagnetarMintXChainModule.sol                     | `Magnetar` module.                                                                                                                                   | 59       | 34             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 📝       | ./contracts/Magnetar/modules/MagnetarOptionModule.sol                         | `Magnetar` module.                                                                                                                                   | 130      | 102            | **<abbr title='Payable Functions'>💰</abbr><abbr title='Initiates ETH Value Transfer'>📤</abbr>**                                                                                                                                                                                                  |
| 📝       | ./contracts/Magnetar/modules/MagnetarYieldBoxModule.sol                       | `Magnetar` module.                                                                                                                                   | 34       | 31             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 🎨       | ./contracts/tapiocaOmnichainEngine/BaseTapiocaOmnichainEngine.sol             |                                                                                                                                                      | 65       | 41             | ****                                                                                                                                                                                                                                                                                               |
| 🎨       | ./contracts/tapiocaOmnichainEngine/BaseToeMsgType.sol                         | `TapiocaOmnichainEngine` message types.                                                                                                              | 11       | 9              | ****                                                                                                                                                                                                                                                                                               |
| 📚       | ./contracts/tapiocaOmnichainEngine/TapiocaOmnichainEngineCodec.sol            | Encode/decode `TOE` LayerZero calls.                                                                                                                 | 246      | 139            | **<abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                                                                                                        |
| 🎨       | ./contracts/tapiocaOmnichainEngine/TapiocaOmnichainReceiver.sol               | LZ call receiver.                                                                                                                                    | 164      | 101            | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='Payable Functions'>💰</abbr><abbr title='DelegateCall'>👥</abbr>**                                                                                                                                                                             |
| 🎨       | ./contracts/tapiocaOmnichainEngine/TapiocaOmnichainSender.sol                 | LZ call sender.                                                                                                                                      | 18       | 13             | **<abbr title='Payable Functions'>💰</abbr>**                                                                                                                                                                                                                                                      |
| 📝       | ./contracts/tapiocaOmnichainEngine/extension/TapiocaOmnichainEngineHelper.sol | Helps encode `TOE` LZ calls.                                                                                                                         | 200      | 70             | **<abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                                                                                                        |
| 📝       | ./contracts/tapiocaOmnichainEngine/extension/TapiocaOmnichainExtExec.sol      | Approvals/Permits caller.                                                                                                                            | 161      | 75             | **<abbr title='Unchecked Blocks'>Σ</abbr>**                                                                                                                                                                                                                                                        |
| 🎨       | ./contracts/utils/ERC721Permit.sol                                            | EIP2612 on ERC721.                                                                                                                                   | 33       | 30             | **<abbr title='Uses Hash-Functions'>🧮</abbr>**                                                                                                                                                                                                                                                    |
| 📝📚🔍🎨 | **Totals**                                                                    |                                                                                                                                                      | **2206** | **1401**       | **<abbr title='Uses Assembly'>🖥</abbr><abbr title='Payable Functions'>💰</abbr><abbr title='Initiates ETH Value Transfer'>📤</abbr><abbr title='DelegateCall'>👥</abbr><abbr title='Uses Hash-Functions'>🧮</abbr><abbr title='create/create2'>🌀</abbr><abbr title='Unchecked Blocks'>Σ</abbr>** |


## Out of scope

Contract marked by `// External`, which all are `node_modules/` external pacakages.

# Additional Context

Our implementation of TapiocaDAO's tw, otherwise known as Time Weighted Average Magnitude Lock. 
tw is a mechanism proposed as a solution for promoting sustainable economic growth for a decentralized finance ecosystem, while maintaining economic alignment among its participants. 

tw addresses issues created by the prevailing practice of liquidity mining, which results in small groups of opportunistic capital providers motivated solely by profit capturing all benefits at the expense of all other actors. 
tw was designed with game theory concepts to reach subgame perfect Nash equilibria, in contrast with liquidity mining where Nash equilibrium cannot be reached due to the presence of a dominant strategy

Read more about twAML at: https://www.tapioca.xyz/docs/twAML.pdf

Miscellaneous: 
- An EIP2612 integration for an ERC721 was used.
- `transferFrom` should be assumed to always be used with the `Pearlmit` (Uniswap's `permit2` alike) contract, instead of the actual token's `transferFrom`.
- LayerZero composed calls are supposed to be called on independent transactions given sequential indexes. However for security purposed, all composed calls will be executed in a single transaction.
- The blockchains that should be assumed for this contest are: `Arbitrum, Ethereum, Optimism, Avalanche`.
- In the event of a DoS, the duration would be 8 hours.

- [ ] Please list specific ERC20 that your protocol is anticipated to interact with. Could be "any" (literally anything, fee on transfer tokens, ERC777 tokens and so forth) or a list of tokens you envision using on launch.
- [ ] Please list specific ERC721 that your protocol is anticipated to interact with.
- [ ] Which blockchains will this code be deployed to, and are considered in scope for this audit?
- [ ] Please list all trusted roles (e.g. operators, slashers, pausers, etc.), the privileges they hold, and any conditions under which privilege escalation is expected/allowable
- [ ] In the event of a DOS, could you outline a minimum duration after which you would consider a finding to be valid? This question is asked in the context of most systems' capacity to handle DoS attacks gracefully for a certain period.
- [ ] Is any part of your implementation intended to conform to any EIP's? If yes, please list the contracts in this format: 
  - `Contract1`: Should comply with `ERC/EIPX`
  - `Contract2`: Should comply with `ERC/EIPY`

## Attack ideas (Where to look for bugs)
*List specific areas to address - see [this blog post](https://medium.com/code4rena/the-security-council-elections-within-the-arbitrum-dao-a-comprehensive-guide-aa6d001aae60#9adb) for an example*

## Main invariants
*Describe the project's main invariants (properties that should NEVER EVER be broken).*

*twAML Invariants*
- No lock can be created and unlocked in the same block
- No lock can be created and unlocked before its (original, uint256) duration has expired.
- Given a small amount of dust, I can never reach X multiple.
- I can never lock for longer than X (intended soft max cap enforced by math if not added via require).
- The sum of all balances at a given epoch, is the sum of all active locks.
- If I could have unlocked at epoch X, then the totalSum of Balances at epoch X reflects that.

## Scoping Details 
[ ⭐️ SPONSORS: please confirm/edit the information below. ]

```
- If you have a public code repo, please share it here:  N/A
- How many contracts are in scope?: 42
- Total nSLoC for these contracts?:  4207
- How many external imports are there?: N/A 
- How many separate interfaces and struct definitions are there for the contracts within scope?:
- Does most of your code generally use composition or inheritance?: Inheritance.
- How many external calls?: 
- What is the overall line coverage percentage provided by your tests?: ~70%.
- Is this an upgrade of an existing system?: False.
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.): Timelock, NFT, ERC20.
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?: Yes
- Please describe required context: Some functions in `tapioca-periph/Magnetar` contract targets another repository, which is `tapioca-bar`.
- Does it use an oracle?: Yes. Oracles can be found in `tapioca-periph/oracle`.
- Describe any novel or unique curve logic or mathematical models your code uses: twAMl. Explained above.
- Is this either a fork of or an alternate implementation of another project?: False.
- Does it use a side-chain?: N/A
- Describe any specific areas you would like addressed: N/A
```

# Tests

*Provide every step required to build the project from a fresh git clone, as well as steps to run the tests with a gas report.* 

*Note: Many wardens run Slither as a first pass for testing.  Please document any known errors with no workaround.* 

## Miscellaneous

Employees of Tapioca and employees' family members are ineligible to participate in this audit.
