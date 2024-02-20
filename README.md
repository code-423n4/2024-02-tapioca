# ✨ So you want to run an audit

This `README.md` contains a set of checklists for our audit collaboration.

Your audit will use two repos: 
- **an _audit_ repo** (this one), which is used for scoping your audit and for providing information to wardens
- **a _findings_ repo**, where issues are submitted (shared with you after the audit) 

Ultimately, when we launch the audit, this repo will be made public and will contain the smart contracts to be reviewed and all the information needed for audit participants. The findings repo will be made public after the audit report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the audit sponsor (⭐️)**.

---

# Audit setup

## 🐺 C4: Set up repos
- [ ] Create a new private repo named `YYYY-MM-sponsorname` using this repo as a template.
- [ ] Rename this repo to reflect audit date (if applicable)
- [ ] Rename auditt H1 below
- [ ] Update pot sizes
- [ ] Fill in start and end times in audit bullets below
- [ ] Add link to submission form in audit details below
- [ ] Add the information from the scoping form to the "Scoping Details" section at the bottom of this readme.
- [ ] Add matching info to the Code4rena site
- [ ] Add sponsor to this private repo with 'maintain' level access.
- [ ] Send the sponsor contact the url for this repo to follow the instructions below and add contracts here. 
- [ ] Delete this checklist.

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 48 business hours prior to audit start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the audit — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the audit. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this `README.md` file

- [ ] Modify the contents of this `README.md` file. Describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. (Here are two well-constructed examples: [Ajna Protocol](https://github.com/code-423n4/2023-05-ajna) and [Maia DAO Ecosystem](https://github.com/code-423n4/2023-05-maia))
- [ ] Review the Gas award pool amount. This can be adjusted up or down, based on your preference - just flag it for Code4rena staff so we can update the pool totals across all comms channels.
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] [This checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746) provides some best practices for Code4rena audits.

## ⭐️ Sponsor: Final touches
- [ ] Review and confirm the details in the section titled "Scoping details" and alert Code4rena staff of any changes.
- [ ] Review and confirm the list of in-scope files in the `scope.txt` file in this directory.  Any files not listed as "in scope" will be considered out of scope for the purposes of judging, even if the file will be part of the deployed contracts.
- [ ] Check that images and other files used in this README have been uploaded to the repo as a file and then linked in the README using absolute path (e.g. `https://github.com/code-423n4/yourrepo-url/filepath.png`)
- [ ] Ensure that *all* links and image/file paths in this README use absolute paths, not relative paths
- [ ] Check that all README information is in markdown format (HTML does not render on Code4rena.com)
- [ ] Remove any part of this template that's not relevant to the final version of the README (e.g. instructions in brackets and italic)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

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
- Starts February 21, 2024 20:00 UTC
- Ends March 13, 2024 20:00 UTC

## This is a Private audit

This audit repo and its Discord channel are accessible to **certified wardens only.** Participation in private audits is bound by:

1. Code4rena's [Certified Contributor Terms and Conditions](https://github.com/code-423n4/code423n4.com/blob/main/_data/pages/certified-contributor-terms-and-conditions.md)
2. C4's [Certified Contributor Code of Professional Conduct](https://code4rena.notion.site/Code-of-Professional-Conduct-657c7d80d34045f19eee510ae06fef55)

*All discussions regarding private audits should be considered private and confidential, unless otherwise indicated.*

Please review the following confidentiality requirements carefully, and if anything is unclear, ask questions in the private audit channel in the C4 Discord.


## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2024-02-tapioca-dao/blob/main/4naly3er-report.md).



_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

[ ⭐️ SPONSORS: Are there any known issues or risks deemed acceptable that shouldn't lead to a valid finding? If so, list them here. ]


# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:** 
- **Documentation:**
- **Website:**
- **Twitter:** 
- **Discord:** 


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

*List any files/contracts that are out of scope for this audit.*

# Additional Context

- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
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
- If you have a public code repo, please share it here:  
- How many contracts are in scope?:   16
- Total SLoC for these contracts?:  750
- How many external imports are there?: 4 
- How many separate interfaces and struct definitions are there for the contracts within scope?:  4
- Does most of your code generally use composition or inheritance?:   Inheritance
- How many external calls?:   1
- What is the overall line coverage percentage provided by your tests?: 100
- Is this an upgrade of an existing system?: False
- Check all that apply (e.g. timelock, NFT, AMM, ERC20, rollups, etc.): 
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:   Yes
- Please describe required context:  Fees are collected from individual uniswap v3 pools via a function on one of the new contracts. That function calls the 'collectProtocol' function on pool contracts.
- Does it use an oracle?:  No
- Describe any novel or unique curve logic or mathematical models your code uses: N/A
- Is this either a fork of or an alternate implementation of another project?:   True
- Does it use a side-chain?:
- Describe any specific areas you would like addressed:
```

# Tests

*Provide every step required to build the project from a fresh git clone, as well as steps to run the tests with a gas report.* 

*Note: Many wardens run Slither as a first pass for testing.  Please document any known errors with no workaround.* 

## Miscellaneous

Employees of [SPONSOR NAME] and employees' family members are ineligible to participate in this audit.
