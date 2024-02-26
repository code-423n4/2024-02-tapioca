# Report

## Gas Optimizations

| |Issue|Instances|
|-|:-|:-:|
| [GAS-1](#GAS-1) | Use ERC721A instead ERC721 | 6 |
| [GAS-2](#GAS-2) | `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings) | 24 |
| [GAS-3](#GAS-3) | Use assembly to check for `address(0)` | 18 |
| [GAS-4](#GAS-4) | Using bools for storage incurs overhead | 1 |
| [GAS-5](#GAS-5) | Cache array length outside of loop | 2 |
| [GAS-6](#GAS-6) | State variables should be cached in stack variables rather than re-reading them from storage | 1 |
| [GAS-7](#GAS-7) | Use calldata instead of memory for function arguments that do not get mutated | 3 |
| [GAS-8](#GAS-8) | For Operations that will not overflow, you could use unchecked | 365 |
| [GAS-9](#GAS-9) | Use Custom Errors instead of Revert Strings to save Gas | 1 |
| [GAS-10](#GAS-10) | Avoid contract existence checks by using low level calls | 9 |
| [GAS-11](#GAS-11) | Stack variable used as a cheaper cache for a state variable is only used once | 1 |
| [GAS-12](#GAS-12) | State variables only set in the constructor should be declared `immutable` | 12 |
| [GAS-13](#GAS-13) | Functions guaranteed to revert when called by normal users can be marked `payable` | 34 |
| [GAS-14](#GAS-14) | `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`) | 9 |
| [GAS-15](#GAS-15) | Using `private` rather than `public` for constants, saves gas | 10 |
| [GAS-16](#GAS-16) | Use shift right/left instead of division/multiplication if possible | 6 |
| [GAS-17](#GAS-17) | Superfluous event fields | 1 |
| [GAS-18](#GAS-18) | `uint256` to `bool` `mapping`: Utilizing Bitmaps to dramatically save on Gas | 1 |
| [GAS-19](#GAS-19) | Increments/decrements can be unchecked in for-loops | 10 |
| [GAS-20](#GAS-20) | Use != 0 instead of > 0 for unsigned integer comparison | 14 |
| [GAS-21](#GAS-21) | `internal` functions not called by the contract should be removed | 9 |

### <a name="GAS-1"></a>[GAS-1] Use ERC721A instead ERC721

ERC721A standard, ERC721A is an improvement standard for ERC721 tokens. It was proposed by the Azuki team and used for developing their NFT collection. Compared with ERC721, ERC721A is a more gas-efficient standard to mint a lot of of NFTs simultaneously. It allows developers to mint multiple NFTs at the same gas price. This has been a great improvement due to Ethereum's sky-rocketing gas fee.

    Reference: https://nextrope.com/erc721-vs-erc721a-2/

*Instances (6)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

5: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

7: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

8: import {IERC721} from "@openzeppelin/contracts/token/ERC721/IERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

6: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

9: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

6: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

### <a name="GAS-2"></a>[GAS-2] `a = a + b` is more gas effective than `a += b` for state variables (excluding arrays and mappings)

This saves **16 gas per instance.**

*Instances (24)*:

```solidity
File: contracts/governance/twTAP.sol

329:                 pool.cumulative += pool.averageMagnitude;

340:             pool.totalDeposited += _amount;

377:         weekTotals[w0 + 1].netActiveVotes += int256(votes);

446:             next.netActiveVotes += prev.netActiveVotes;

448:                 next.totalDistPerVote[i] += prev.totalDistPerVote[i];

476:         totals.totalDistPerVote[_rewardTokenId] += (_amount * DIST_PRECISION) / uint256(totals.netActiveVotes);

565:                     claimed[_tokenId][i] += amount;

607:                 pool.cumulative += position.averageMagnitude;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

255:         aoTAPCalls[_aoTAPTokenID][cachedEpoch] += chosenAmount; // Adds up exercised amount to current epoch

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

274:                 pool.cumulative += pool.averageMagnitude;

284:             pool.totalDeposited += lock.ybShares;

293:         netDepositedForEpoch[epoch + 1][lock.sglAssetID] += int256(uint256(lock.ybShares));

337:                 pool.cumulative += participation.averageMagnitude;

397:         oTAPCalls[_oTAPTokenID][cachedEpoch] += chosenAmount; // Adds up exercised amount to current epoch

621:                 curr[sglAssetID] += prev[sglAssetID];

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

212:         activeSingularities[_singularity].totalDeposited += _ybShares;

386:                 total += sgl.poolWeight;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

360:         mintedInWeek[week] += _amount;

400:         emission += unclaimed;

406:             emission += boostedTAP; // Add TAP in the contract as boosted TAP

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

146:         __totalClaimed += _claimable;

147:         users[msg.sender].claimed += _claimable;

171:         totalRegisteredAmount += _amount;

202:             _totalAmount += _amounts[i];

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="GAS-3"></a>[GAS-3] Use assembly to check for `address(0)`

*Saves 6 gas per instance*

*Instances (18)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

49: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

156:     /// @param _paymentToken The payment token

157:     /// @param _tapAmount The amount of TAP to be exchanged. If 0 it will use the full amount of TAP eligible for the deal

306:         emit SetTapOracle(_tapOracle, _tapOracleData);

376:     /// Should occur after the end of the airdrop, which is 8 epochs, or 41 days long.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

54:         uint256 amount = balanceOf(msg.sender);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

122: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

500:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/oTAP.sol

112: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

50: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

151:             _mint(_data.airdrop, 1e18 * 2_500_000);

169:         override(BaseTapiocaOmnichainEngine, ERC20)

172:         return BaseTapiocaOmnichainEngine.transferFrom(from, to, value);

443:     function setPause(bool _pauseState) external onlyOwner {

457:      * @param timestamp The timestamp to use to compute the week

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

179:     /// @param _amounts user weights

210:         if (_cachedTotalAmount > _totalAmount) revert Overflow();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

57:     function _executeModule(uint8 _module, bytes memory _data, bool _forwardRevert)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="GAS-4"></a>[GAS-4] Using bools for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past. See [source](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/58f635312aa21f947cae5f8578638a85aa2519f5/contracts/security/ReentrancyGuard.sol#L23-L27).

*Instances (1)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

63:     mapping(address => mapping(uint256 => bool)) public userParticipation; // user address => phase => participated

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="GAS-5"></a>[GAS-5] Cache array length outside of loop

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

*Instances (2)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

329:             for (uint256 i; i < _users.length; i++) {

335:             for (uint256 i; i < _users.length; i++) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="GAS-6"></a>[GAS-6] State variables should be cached in stack variables rather than re-reading them from storage

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses.

*Saves 100 gas per instance*

*Instances (1)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

49: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

### <a name="GAS-7"></a>[GAS-7] Use calldata instead of memory for function arguments that do not get mutated

When a function with a `memory` array is called externally, the `abi.decode()` step has to use a for-loop to copy each index of the `calldata` to the `memory` index. Each iteration of this for-loop costs at least 60 gas (i.e. `60 * <mem_array>.length`). Using `calldata` directly bypasses this loop.

If the array is passed to an `internal` function which passes the array to another internal function where the array is modified and therefore `memory` is used in the `external` call, it's still more gas-efficient to use `calldata` when the `external` function uses modifiers, since the modifiers may prevent the internal functions from being called. Structs have the same overhead as an array of length one.

 *Saves 60 gas per instance*

*Instances (3)*:

```solidity
File: contracts/tokens/TapToken.sol

244:      * @dev Executes the send operation.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/extensions/TapTokenHelper.sol

64:      * @dev The amount field is trivial in this message as it'll be overwritten by the receiver contract.

85:         }

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/extensions/TapTokenHelper.sol)

### <a name="GAS-8"></a>[GAS-8] For Operations that will not overflow, you could use unchecked

*Instances (365)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

5: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

9: import {INFTLoader} from "../interfaces/INFTLoader.sol";

25:     INFTLoader public nftLoader; // NFT URI loader contract

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

5: import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

6: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

7: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

8: import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

9: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

10: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

13: import {IPearlmit, PearlmitHandler} from "tapioca-periph/Pearlmit/PearlmitHandler.sol";

14: import {ERC721NftLoader} from "tap-token/erc721NftLoader/ERC721NftLoader.sol";

15: import {ERC721Permit} from "tapioca-periph/utils/ERC721Permit.sol";

16: import {ERC721PermitStruct} from "tap-token/tokens/ITapToken.sol";

17: import {TapToken} from "tap-token/tokens/TapToken.sol";

18: import {TWAML} from "tap-token/options/twAML.sol";

43:     bool divergenceForce; // 0 negative, 1 positive

44:     bool tapReleased; // allow restaking while rewards may still accumulate

45:     uint56 expiry; // expiry timestamp. Big enough for over 2 billion years..

46:     uint88 tapAmount; // amount of TAP locked

47:     uint24 multiplier; // Votes = multiplier * tapAmount

48:     uint40 lastInactive; // One week BEFORE the staker gets a share of rewards

49:     uint40 lastActive; // Last week that the staker shares in rewards

75:     TWAMLPool public twAML; // sglAssetId => twAMLPool

77:     mapping(uint256 => Participation) public participants; // tokenId => part.

82:     uint256 MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

83:     uint256 constant dMAX = 1_000_000; // 100 * 1e4; 0% - 100% voting power multiplier

86:     uint256 public constant MAX_LOCK_DURATION = 100 * 365 days; // 100 years

97:     uint256 constant DIST_PRECISION = 2 ** 128; //2 ** 128;

100:     mapping(IERC20 => uint256) public rewardTokenIndex; // Index 0 is reserved with 0x0 address

111:     uint256 public creation; // Week 0 starts here

138:         rewardTokens.push(IERC20(address(0x0))); // 0 index is reserved

174:         return (block.timestamp - creation) / EPOCH_DURATION;

200:             votes = uint256(position.tapAmount) * uint256(position.multiplier);

259:             uint256 net = cur.totalDistPerVote[i] - prev.totalDistPerVote[i];

260:             result[i] = ((votes * net) / DIST_PRECISION) - claimed[_tokenId][i];

262:                 ++i;

315:         if (magnitude >= pool.cumulative * 4) revert NotValid();

320:         bool hasVotingPower = _amount >= computeMinWeight(pool.totalDeposited + VIRTUAL_TOTAL_AMOUNT, MIN_WEIGHT_FACTOR);

322:             pool.totalParticipants++; // Save participation

323:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

329:                 pool.cumulative += pool.averageMagnitude;

333:                     pool.cumulative -= pool.averageMagnitude;

340:             pool.totalDeposited += _amount;

342:             twAML = pool; // Save twAML participation

346:         uint256 expiry = block.timestamp + _duration;

356:         uint256 w1 = (expiry - creation) / EPOCH_DURATION;

360:         tokenId = ++mintedTWTap;

361:         uint256 votes = _amount * multiplier;

377:         weekTotals[w0 + 1].netActiveVotes += int256(votes);

378:         weekTotals[w1 + 1].netActiveVotes -= int256(votes);

437:             if (goal - week > _limit) {

438:                 goal = week + _limit;

444:             WeekTotals storage next = weekTotals[++week];

446:             next.netActiveVotes += prev.netActiveVotes;

448:                 next.totalDistPerVote[i] += prev.totalDistPerVote[i];

450:                     ++i;

466:         if (_rewardTokenId == 0) revert NotValid(); // @dev rewardTokens[0] is 0x0

476:         totals.totalDistPerVote[_rewardTokenId] += (_amount * DIST_PRECISION) / uint256(totals.netActiveVotes);

513:         if (rewardTokens.length + 1 > maxRewardTokens) {

518:         uint256 newTokenIndex = rewardTokens.length - 1;

541:         return ((timestamp - creation) / EPOCH_DURATION);

561:             for (uint256 i; i < len; ++i) {

565:                     claimed[_tokenId][i] += amount;

593:                 --pool.totalParticipants;

602:                     pool.cumulative -= position.averageMagnitude;

607:                 pool.cumulative += position.averageMagnitude;

611:             pool.totalDeposited -= position.tapAmount;

613:             twAML = pool; // Save twAML exit

614:             emit AMLDivergence(pool.cumulative, pool.averageMagnitude, pool.totalParticipants); // Register new voting power event

629:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

5: import {MerkleProof} from "@openzeppelin/contracts/utils/cryptography/MerkleProof.sol";

6: import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

7: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import {IERC721} from "@openzeppelin/contracts/token/ERC721/IERC721.sol";

9: import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

10: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

11: import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

12: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

15: import {IPearlmit, PearlmitHandler} from "tapioca-periph/pearlmit/PearlmitHandler.sol";

16: import {ITapiocaOracle} from "tapioca-periph/interfaces/periph/ITapiocaOracle.sol";

17: import {TWAML, FullMath} from "tap-token/options/twAML.sol"; // TODO Naming

18: import {TapToken} from "tap-token/tokens/TapToken.sol";

19: import {AOTAP, AirdropTapOption} from "./aoTAP.sol";

52:     uint128 public epochTAPValuation; // TAP price for the current epoch

53:     uint64 public lastEpochUpdate; // timestamp of the last epoch update

54:     uint64 public epoch; // Represents the number of weeks since the start of the contract

56:     mapping(ERC20 => PaymentTokenOracle) public paymentTokens; // Token address => PaymentTokenOracle

57:     address public paymentTokenBeneficiary; // Where to collect the payment tokens

59:     mapping(uint256 => mapping(uint256 => uint256)) public aoTAPCalls; // oTAPTokenID => epoch => amountExercised

63:     mapping(address => mapping(uint256 => bool)) public userParticipation; // user address => phase => participated

71:     uint256 public constant PHASE_1_DISCOUNT = 500_000; //50 * 1e4; 50%

78:     bytes32[4] public phase2MerkleRoots; // merkle root of phase 2 airdrop

87:     uint256 public constant PHASE_3_DISCOUNT = 500_000; //50 * 1e4; 50%

95:     uint256 public constant PHASE_4_DISCOUNT = 330_000; //33 * 1e4;

97:     uint256 public EPOCH_DURATION = 2 days; // Becomes 7 days at the start of the phase 4

98:     uint256 public constant LAST_EPOCH = 8; // 8 epochs, 41 days long

182:         eligibleTapAmount -= aoTAPCalls[_aoTAPTokenID][cachedEpoch]; // Subtract already exercised amount

188:         uint256 otcAmountInUSD = tapAmount * epochTAPValuation; // Divided by TAP decimals

212:             aoTAPTokenID = _participatePhase2(_data); // _data = (uint256 role, bytes32[] _merkleProof)

214:             aoTAPTokenID = _participatePhase3(_data); // _data = (uint256[] _tokenID)

250:         eligibleTapAmount -= aoTAPCalls[_aoTAPTokenID][cachedEpoch]; // Subtract already exercised amount

255:         aoTAPCalls[_aoTAPTokenID][cachedEpoch] += chosenAmount; // Adds up exercised amount to current epoch

265:         if (block.timestamp < lastEpochUpdate + EPOCH_DURATION) {

271:         epoch++;

329:             for (uint256 i; i < _users.length; i++) {

335:             for (uint256 i; i < _users.length; i++) {

368:             for (uint256 i; i < len; ++i) {

407:         uint128 expiry = uint128(lastEpochUpdate + EPOCH_DURATION); // Set expiry to the end of the epoch

422:         uint256 subPhase = 20 + _role;

430:         uint128 expiry = uint128(lastEpochUpdate + EPOCH_DURATION); // Set expiry to the end of the epoch

431:         uint256 eligibleAmount = uint256(PHASE_2_AMOUNT_PER_USER[_role]) * 1e18;

458:                 ++i;

462:         uint128 expiry = uint128(lastEpochUpdate + EPOCH_DURATION); // Set expiry to the end of the epoch

463:         uint256 eligibleAmount = arrLen * PHASE_3_AMOUNT_PER_USER * 1e18; // Phase 3 amount multiplied the number of PCNFTs

477:         uint128 expiry = uint128(lastEpochUpdate + EPOCH_DURATION); // Set expiry to the end of the epoch

495:         uint256 otcAmountInUSD = tapAmount * epochTAPValuation;

514:         if (balAfter - balBefore != discountedPaymentAmount) revert Failed();

533:         uint256 rawPaymentAmount = _otcAmountInUSD / _paymentTokenValuation;

534:         paymentAmount = rawPaymentAmount - muldiv(rawPaymentAmount, _discount, 100e4); // 1e4 is discount decimals, 100 is discount percentage

536:         paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

4: import {ERC20, ERC20Permit} from "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";

5: import {SafeERC20, IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

34:         _mint(_lbp, 5_000_000 * 1e18); // 5M LTAP for LBP

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

5: import {BaseBoringBatchable} from "@boringcrypto/boring-solidity/contracts/BoringBatchable.sol";

6: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

7: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

10: import {IPearlmit, PearlmitHandler} from "tapioca-periph/Pearlmit/PearlmitHandler.sol";

11: import {ERC721Permit} from "tapioca-periph/utils/ERC721Permit.sol";

25:     uint128 expiry; // timestamp, as once one wise man said, the sun will go dark before this overflows

26:     uint128 discount; // discount in basis points

27:     uint256 amount; // amount of eligible TAP

31:     uint256 public mintedAOTAP; // total number of AOTAP minted

32:     address public broker; // address of the onlyBroker

34:     mapping(uint256 => AirdropTapOption) public options; // tokenId => Option

35:     mapping(uint256 => string) public tokenURIs; // tokenId => tokenURI

97:         tokenId = ++mintedAOTAP;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

5: import {IERC721Receiver} from "@openzeppelin/contracts/token/ERC721/IERC721Receiver.sol";

6: import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

7: import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

8: import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

9: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

10: import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

11: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

14: import {TapiocaOptionLiquidityProvision, LockPosition, SingularityPool} from "./TapiocaOptionLiquidityProvision.sol";

15: import {IPearlmit, PearlmitHandler} from "tapioca-periph/pearlmit/PearlmitHandler.sol";

16: import {ITapiocaOracle} from "tapioca-periph/interfaces/periph/ITapiocaOracle.sol";

17: import {TapToken} from "tap-token/tokens/TapToken.sol";

18: import {OTAP, TapOption} from "./oTAP.sol";

19: import {TWAML} from "./twAML.sol";

34:     bool divergenceForce; // 0 negative, 1 positive

59:     uint256 public epochTAPValuation; // TAP price for the current epoch

60:     uint256 public epoch; // Represents the number of weeks since the start of the contract

62:     mapping(uint256 => Participation) public participants; // tOLPTokenID => Participation

63:     mapping(uint256 => mapping(uint256 => uint256)) public oTAPCalls; // oTAPTokenID => epoch => amountExercised

65:     mapping(uint256 => mapping(uint256 => uint256)) public singularityGauges; // epoch => sglAssetId => availableTAP

67:     mapping(ERC20 => PaymentTokenOracle) public paymentTokens; // Token address => PaymentTokenOracle

68:     address public paymentTokenBeneficiary; // Where to collect the payment tokens

71:     mapping(uint256 => TWAMLPool) public twAML; // sglAssetId => twAMLPool

76:     uint256 public MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

77:     uint256 constant dMAX = 500_000; // 50 * 1e4; 0% - 50% discount

79:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

194:         if (block.timestamp < tOLPLockPosition.lockTime + EPOCH_DURATION) {

196:         } // Can only exercise after 1 epoch duration

205:             eligibleTapAmount -= oTAPCalls[_oTAPTokenID][cachedEpoch]; // Subtract already exercised amount

212:         uint256 otcAmountInUSD = tapAmount * epochTAPValuation; // Divided by TAP decimals

231:         uint128 lockExpiry = lock.lockTime + lock.lockDuration;

261:         if (magnitude > pool.cumulative * 4) revert TooLong();

266:             lock.ybShares >= computeMinWeight(pool.totalDeposited + VIRTUAL_TOTAL_AMOUNT, MIN_WEIGHT_FACTOR);

268:             pool.totalParticipants++; // Save participation

269:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

274:                 pool.cumulative += pool.averageMagnitude;

277:                     pool.cumulative -= pool.averageMagnitude;

284:             pool.totalDeposited += lock.ybShares;

286:             twAML[lock.sglAssetID] = pool; // Save twAML participation

287:             emit AMLDivergence(epoch, pool.cumulative, pool.averageMagnitude, pool.totalParticipants); // Register new voting power event

293:         netDepositedForEpoch[epoch + 1][lock.sglAssetID] += int256(uint256(lock.ybShares));

298:         netDepositedForEpoch[lastEpoch + 1][lock.sglAssetID] -= int256(uint256(lock.ybShares));

318:             if (block.timestamp < lock.lockTime + lock.lockDuration) {

332:                     pool.cumulative -= participation.averageMagnitude;

337:                 pool.cumulative += participation.averageMagnitude;

340:             pool.totalDeposited -= lock.ybShares;

343:                 --pool.totalParticipants;

346:             twAML[lock.sglAssetID] = pool; // Save twAML exit

347:             emit AMLDivergence(epoch, pool.cumulative, pool.averageMagnitude, pool.totalParticipants); // Register new voting power event

383:         if (block.timestamp < oTAPPosition.entry + EPOCH_DURATION) {

385:         } // Can only exercise after 1 epoch duration

392:         eligibleTapAmount -= oTAPCalls[_oTAPTokenID][cachedEpoch]; // Subtract already exercised amount

397:         oTAPCalls[_oTAPTokenID][cachedEpoch] += chosenAmount; // Adds up exercised amount to current epoch

414:         epoch++;

490:             for (uint256 i; i < len; ++i) {

513:         return ((timestamp - emissionsStartTime) / EPOCH_DURATION);

531:         uint256 expiryWeek = _timestampToWeek(_lock.lockTime + _lock.lockDuration);

548:         uint256 otcAmountInUSD = tapAmount * epochTAPValuation;

566:         if (balAfter - balBefore != discountedPaymentAmount) {

587:         uint256 discountedOTCAmountInUSD = _otcAmountInUSD - muldiv(_otcAmountInUSD, _discount, 100e4); // 1e4 is discount decimals, 100 is discount percentage

590:         paymentAmount = discountedOTCAmountInUSD / _paymentTokenValuation;

593:             paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

595:             paymentAmount = paymentAmount * (10 ** (_paymentTokenDecimals - 18));

608:             for (uint256 i; i < len; ++i) {

616:                 mapping(uint256 sglAssetID => int256 netAmount) storage prev = netDepositedForEpoch[epoch - 1];

621:                 curr[sglAssetID] += prev[sglAssetID];

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

5: import {BaseBoringBatchable} from "@boringcrypto/boring-solidity/contracts/BoringBatchable.sol";

6: import {IERC1155Receiver} from "@openzeppelin/contracts/token/ERC1155/IERC1155Receiver.sol";

7: import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

8: import {IERC165} from "@openzeppelin/contracts/utils/introspection/IERC165.sol";

9: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

10: import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

11: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

12: import {ERC721Permit} from "tapioca-periph/utils/ERC721Permit.sol";

13: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

16: import {IPearlmit, PearlmitHandler} from "tapioca-periph/pearlmit/PearlmitHandler.sol";

17: import {IYieldBox} from "tap-token/interfaces/IYieldBox.sol";

31:     uint128 sglAssetID; // Singularity market YieldBox asset ID

32:     uint128 ybShares; // amount of YieldBox shares locked.

33:     uint128 lockTime; // time when the tokens were locked

34:     uint128 lockDuration; // duration of the lock

38:     uint256 sglAssetID; // Singularity market YieldBox asset ID

39:     uint256 totalDeposited; // total amount of YieldBox shares deposited, used for pool share calculation

40:     uint256 poolWeight; // Pool weight to calculate emission

41:     bool rescue; // If true, the pool will be used to rescue funds in case of emergency

54:     uint256 public tokenCounter; // Counter for token IDs

55:     mapping(uint256 => LockPosition) public lockPositions; // TokenID => LockPosition

61:     mapping(uint256 => IERC20) public sglAssetIDToAddress; // Singularity market YieldBox asset ID => Singularity market address

62:     uint256[] public singularities; // Array of active singularity asset IDs

64:     uint256 public rescueCooldown = 2 days; // Cooldown before a singularity pool can be put in rescue mode

65:     mapping(uint256 sglId => uint256 rescueTime) public sglRescueRequest; // Time when the pool was put in rescue mode

67:     uint256 public totalSingularityPoolWeights; // Total weight of all active singularity pools

68:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

69:     uint256 public constant MAX_LOCK_DURATION = 100 * 365 days; // 100 years

144:             for (uint256 i; i < len; ++i) {

212:         activeSingularities[_singularity].totalDeposited += _ybShares;

215:         tokenId = ++tokenCounter;

241:             if (block.timestamp < lockPosition.lockTime + lockPosition.lockDuration) revert LockNotExpired();

254:         activeSingularities[_singularity].totalDeposited -= lockPosition.ybShares;

300:         if (block.timestamp < sglRescueRequest[sgl.sglAssetID] + rescueCooldown) revert RescueCooldownNotReached();

342:             uint256 sglLastIndex = sglLength - 1;

344:             for (uint256 i; i < sglLength; i++) {

383:         for (uint256 i; i < len; i++) {

386:                 total += sgl.poolWeight;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

5: import {BaseBoringBatchable} from "@boringcrypto/boring-solidity/contracts/BoringBatchable.sol";

6: import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";

9: import {ERC721NftLoader} from "tap-token/erc721NftLoader/ERC721NftLoader.sol";

10: import {ERC721Permit} from "tapioca-periph/utils/ERC721Permit.sol"; // TODO audit

25:     uint128 entry; // time when the option position was created

26:     uint128 expiry; // timestamp, as once one wise man said, the sun will go dark before this overflows

27:     uint128 discount; // discount in basis points

28:     uint256 tOLP; // tOLP token ID

32:     uint256 public mintedOTAP; // total number of OTAP minted

33:     address public broker; // address of the onlyBroker

35:     mapping(uint256 => TapOption) public options; // tokenId => Option

86:         tokenId = ++mintedOTAP;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/options/twAML.sol

27:             uint256 prod0; // Least significant 256 bits of the product

28:             uint256 prod1; // Most significant 256 bits of the product

70:             uint256 twos = denominator & (~denominator + 1);

86:             prod0 |= prod1 * twos;

94:             uint256 inv = (3 * denominator) ^ 2;

98:             inv *= 2 - denominator * inv; // inverse mod 2**8

99:             inv *= 2 - denominator * inv; // inverse mod 2**16

100:             inv *= 2 - denominator * inv; // inverse mod 2**32

101:             inv *= 2 - denominator * inv; // inverse mod 2**64

102:             inv *= 2 - denominator * inv; // inverse mod 2**128

103:             inv *= 2 - denominator * inv; // inverse mod 2**256

112:             result = prod0 * inv;

123:         uint256 mul = (_totalWeight * _minWeightFactor);

124:         return mul >= 1e4 ? mul / 1e4 : _totalWeight;

128:         return sqrt(_timeWeight * _timeWeight + _cumulative * _cumulative) - _cumulative;

139:         uint256 target = (_magnitude * _dMax) / _cumulative;

148:             uint256 x = y / 2 + 1;

151:                 x = (y / x + x) / 2;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

5: import {BaseTapiocaOmnichainEngine} from "tapioca-periph/tapiocaOmnichainEngine/BaseTapiocaOmnichainEngine.sol";

6: import {IPearlmit} from "tapioca-periph/interfaces/periph/IPearlmit.sol";

7: import {BaseTapTokenMsgType} from "./BaseTapTokenMsgType.sol";

8: import {TwTAP} from "tap-token/governance/twTAP.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

5: import {IMessagingChannel} from "@layerzerolabs/lz-evm-protocol-v2/contracts/interfaces/IMessagingChannel.sol";

6: import {MessagingReceipt, OFTReceipt} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/interfaces/IOFT.sol";

7: import {OAppReceiver} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oapp/OAppReceiver.sol";

8: import {Origin} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oapp/OApp.sol";

11: import {ERC20Permit, ERC20} from "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";

12: import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";

15: import {BaseTapiocaOmnichainEngine} from "tapioca-periph/tapiocaOmnichainEngine/BaseTapiocaOmnichainEngine.sol";

16: import {TapiocaOmnichainSender} from "tapioca-periph/tapiocaOmnichainEngine/TapiocaOmnichainSender.sol";

17: import {ERC20PermitStruct, ITapToken, LZSendParam} from "tap-token/tokens/ITapToken.sol";

18: import {ModuleManager} from "./module/ModuleManager.sol";

19: import {TapTokenReceiver} from "./TapTokenReceiver.sol";

20: import {TwTAP} from "tap-token/governance/twTAP.sol";

21: import {TapTokenSender} from "./TapTokenSender.sol";

22: import {BaseTapToken} from "./BaseTapToken.sol";

39:     uint256 public constant INITIAL_SUPPLY = 46_686_595 * 1e18; // Everything minus DSO

40:     uint256 public dso_supply = 53_313_405 * 1e18; // Emission supply for DSO

43:     uint256 constant decay_rate = 8800000000000000; // 0.88%

47:     uint256 public constant EPOCH_DURATION = 1 weeks; // 604800

86:     error NotValid(); // Generic error for simple functions

88:     error SupplyNotValid(); // Initial supply is not valid

146:             _mint(_data.contributors, 1e18 * 15_000_000);

147:             _mint(_data.earlySupporters, 1e18 * 3_686_595);

148:             _mint(_data.supporters, 1e18 * 12_500_000);

149:             _mint(_data.aoTap, 1e18 * 5_000_000);

150:             _mint(_data.dao, 1e18 * 8_000_000);

151:             _mint(_data.airdrop, 1e18 * 2_500_000);

206:         address _executor, // @dev unused in the default implementation.

207:         bytes calldata _extraData // @dev unused in the default implementation.

356:         if (emissionForWeek[week] < mintedInWeek[week] + _amount) {

360:         mintedInWeek[week] += _amount;

394:             dso_supply -= mintedInWeek[week - 1];

397:             unclaimed = emissionForWeek[week - 1] - mintedInWeek[week - 1];

400:         emission += unclaimed;

406:             emission += boostedTAP; // Add TAP in the contract as boosted TAP

460:         return ((timestamp - emissionsStartTime) / EPOCH_DURATION);

467:         result = (dso_supply * decay_rate) / DECAY_RATE_DECIMAL;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

6: import {OptionsBuilder} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oapp/libs/OptionsBuilder.sol";

7: import {OFTMsgCodec} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/libs/OFTMsgCodec.sol";

8: import {BytesLib} from "solidity-bytes-utils/contracts/BytesLib.sol";

20: } from "./ITapToken.sol";

22: import {TapiocaOmnichainEngineCodec} from "tapioca-periph/tapiocaOmnichainEngine/TapiocaOmnichainEngineCodec.sol";

77:         uint256 amount = BytesLib.toUint256(BytesLib.slice(_msg, durationOffset_, _msg.length - durationOffset_), 0);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

7: } from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/interfaces/IOFT.sol";

8: import {OFTMsgCodec} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/libs/OFTMsgCodec.sol";

9: import {OFTCore} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/OFTCore.sol";

10: import {Origin} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oapp/OApp.sol";

11: import {OFT} from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/OFT.sol";

14: import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";

25: } from "./ITapToken.sol";

26: import {TapiocaOmnichainReceiver} from "tapioca-periph/tapiocaOmnichainEngine/TapiocaOmnichainReceiver.sol";

27: import {IPearlmit} from "tapioca-periph/interfaces/periph/IPearlmit.sol";

28: import {TapTokenSender} from "./TapTokenSender.sol";

29: import {TapTokenCodec} from "./TapTokenCodec.sol";

30: import {BaseTapToken} from "./BaseTapToken.sol";

74:         address _executor, /*_executor*/ // @dev unused in the default implementation.

75:         bytes calldata _extraData /*_extraData*/ // @dev unused in the default implementation.

122:             address(this), 0, address(twTap), uint200(lockTwTapPositionMsg_.amount), uint48(block.timestamp + 1)

160:             (claimedAmount_.length - 1) // Remove 1 because the first index doesn't count.

175:             uint256 sendParamIndex = i - 1; // Remove 1 to account for the reserved 0 index.

181:             uint256 dust = claimedAmount_[i] - amountWithoutDust;

190:             claimTwTapRewardsMsg_.sendParam[sendParamIndex].sendParam.amountLD = amountWithoutDust; // Set the amount to send to the claimed amount

191:             claimTwTapRewardsMsg_.sendParam[sendParamIndex].sendParam.minAmountLD = amountWithoutDust; // Set the amount to send to the claimed amount

200:                 ++i;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/TapTokenSender.sol

7: } from "@layerzerolabs/lz-evm-oapp-v2/contracts/oft/interfaces/IOFT.sol";

9: import {TapiocaOmnichainSender} from "tapioca-periph/tapiocaOmnichainEngine/TapiocaOmnichainSender.sol";

10: import {IPearlmit} from "tapioca-periph/interfaces/periph/IPearlmit.sol";

11: import {BaseTapToken} from "./BaseTapToken.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenSender.sol)

```solidity
File: contracts/tokens/Vesting.sol

4: import {SafeERC20, IERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

5: import {ReentrancyGuard} from "@openzeppelin/contracts/security/ReentrancyGuard.sol";

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

98:         return _vested(seeded) - __totalClaimed;

104:         return _vested(users[_user].amount) - users[_user].claimed;

146:         __totalClaimed += _claimable;

147:         users[msg.sender].claimed += _claimable;

171:         totalRegisteredAmount += _amount;

202:             _totalAmount += _amounts[i];

205:                 ++i;

233:             uint256 initialUnlockAmount = (_seededAmount * _initialUnlock) / 10_000;

250:         return _start - (_start - ((_amount * _duration) / _totalAmount));

264:         if (_start == 0) return 0; // Not started

267:             _start = _start + _cliff; // Apply cliff offset

268:             if (block.timestamp < _start) return 0; // Cliff not reached

271:         if (block.timestamp >= _start + _duration) return _totalAmount; // Fully vested

273:         _start = _start - __initialUnlockTimeOffset; // Offset initial unlock so it's claimable immediately

274:         return (_totalAmount * (block.timestamp - _start)) / _duration; // Partially vested

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/extensions/TapTokenHelper.sol

10: } from "tapioca-periph/tapiocaOmnichainEngine/extension/TapiocaOmnichainEngineHelper.sol";

11: import {ITapToken, LockTwTapPositionMsg, UnlockTwTapPositionMsg, ClaimTwTapRewardsMsg} from "../ITapToken.sol";

12: import {BaseTapTokenMsgType} from "../BaseTapTokenMsgType.sol";

13: import {TapTokenCodec} from "../TapTokenCodec.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/extensions/TapTokenHelper.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

84:         return abi.decode(_returnData, (string)); // All that remains is the revert string

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="GAS-9"></a>[GAS-9] Use Custom Errors instead of Revert Strings to save Gas

Custom errors are available from solidity version 0.8.4. Custom errors save [**~50 gas**](https://gist.github.com/IllIllI000/ad1bd0d29a0101b25e57c293b4b0c746) each time they're hit by [avoiding having to allocate and store the revert string](https://blog.soliditylang.org/2021/04/21/custom-errors/#errors-in-depth). Not defining the strings also save deployment gas

Additionally, custom errors can be used inside and outside of contracts (including interfaces and libraries).

Source: <https://blog.soliditylang.org/2021/04/21/custom-errors/>:

> Starting from [Solidity v0.8.4](https://github.com/ethereum/solidity/releases/tag/v0.8.4), there is a convenient and gas-efficient way to explain to users why an operation failed through the use of custom errors. Until now, you could already use strings to give more information about failures (e.g., `revert("Insufficient funds.");`), but they are rather expensive, especially when it comes to deploy cost, and it is difficult to use dynamic information in them.

Consider replacing **all revert strings** with custom errors in the solution, and particularly those that have multiple occurrences:

*Instances (1)*:

```solidity
File: contracts/tokens/Vesting.sol

182:         if (_users.length != _amounts.length) revert("Lengths not equal");

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="GAS-10"></a>[GAS-10] Avoid contract existence checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including `EXTCODESIZE` (**100 gas**), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence

*Instances (9)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

370:                 paymentToken.safeTransfer(paymentTokenBeneficiary, paymentToken.balanceOf(address(this)));

380:         tapToken.transfer(msg.sender, tapToken.balanceOf(address(this)));

506:         uint256 balBefore = _paymentToken.balanceOf(address(this));

513:         uint256 balAfter = _paymentToken.balanceOf(address(this));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

492:                 paymentToken.safeTransfer(_paymentTokenBeneficiary, paymentToken.balanceOf(address(this)));

558:         uint256 balBefore = _paymentToken.balanceOf(address(this));

565:         uint256 balAfter = _paymentToken.balanceOf(address(this));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/tokens/Vesting.sol

225:         uint256 availableToken = _token.balanceOf(address(this));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

64:         (success, returnData) = module.delegatecall(_data);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="GAS-11"></a>[GAS-11] Stack variable used as a cheaper cache for a state variable is only used once

If the variable is only accessed once, it's cheaper to use the state variable directly that one time, and save the **3 gas** the extra stack assignment would spend

*Instances (1)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

185:         tapAmount = _tapAmount == 0 ? eligibleTapAmount : _tapAmount;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="GAS-12"></a>[GAS-12] State variables only set in the constructor should be declared `immutable`

Variables only set in the constructor and never edited afterwards should be marked as immutable, as it would avoid the expensive storage-writing operation in the constructor (around **20 000 gas** per variable) and replace the expensive storage-reading operations (around **2100 gas** per reading) to a less expensive value reading (**3 gas**)

*Instances (12)*:

```solidity
File: contracts/governance/twTAP.sol

151:         uint256 indexed cumulative, uint256 indexed averageMagnitude, uint256 indexed totalParticipants

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

139:     event SetPaymentToken(ERC20 paymentToken, ITapiocaOracle oracle, bytes oracleData);

140:     event SetTapOracle(ITapiocaOracle oracle, bytes oracleData);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

136:         uint256 indexed epoch, uint256 indexed sglAssetID, uint256 totalDeposited, uint256 tokenId, uint256 discount

138:     event AMLDivergence(uint256 indexed epoch, uint256 cumulative, uint256 averageMagnitude, uint256 totalParticipants);

139:     event ExerciseOption(

140:         uint256 indexed epoch, address indexed to, ERC20 indexed paymentToken, uint256 oTapTokenID, uint256 amount

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

107:     event RequestSglPoolRescue(uint256 sglAssetId, uint256 timestamp);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

152:             if (totalSupply() != INITIAL_SUPPLY) revert SupplyNotValid();

167:     function transferFrom(address from, address to, uint256 value)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

103:     function claimable(address _user) public view returns (uint256) {

104:         return _vested(users[_user].amount) - users[_user].claimed;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="GAS-13"></a>[GAS-13] Functions guaranteed to revert when called by normal users can be marked `payable`

If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (34)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

45:     function setNftLoader(address _nftLoader) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

489:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

497:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

501:     function setMaxRewardTokensLength(uint256 _length) external onlyOwner {

511:     function addRewardToken(IERC20 _token) external onlyOwner returns (uint256) {

527:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

302:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

309:     function setPhase2MerkleRoots(bytes32[4] calldata _merkleRoots) external onlyOwner {

355:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

361:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

377:     function daoRecoverTAP() external onlyOwner {

386:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

44:     function setTapToken(address _tapToken) external onlyOwner {

48:     function setOpenRedemption() external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

440:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

448:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

455:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

476:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

482:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

500:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

266:     function setSGLPoolWeight(IERC20 singularity, uint256 weight) external onlyOwner updateTotalSGLPoolWeights {

275:     function setRescueCooldown(uint256 _rescueCooldown) external onlyOwner {

283:     function requestSglPoolRescue(uint256 _sglAssetID) external onlyOwner {

294:     function activateSGLPoolRescue(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

334:     function unregisterSingularity(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

366:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

352:     function extractTAP(address _to, uint256 _amount) external onlyMinter whenNotPaused {

384:     function emitForWeek() external onlyMinter returns (uint256) {

424:     function setMinter(address _minter) external onlyOwner {

433:     function setTwTAP(address _twTap) external override onlyOwner onlyHostChain {

443:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

161:     function registerUser(address _user, uint256 _amount) external onlyOwner {

180:     function registerUsers(address[] calldata _users, uint256[] calldata _amounts) external onlyOwner {

219:     function init(IERC20 _token, uint256 _seededAmount, uint256 _initialUnlock) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="GAS-14"></a>[GAS-14] `++i` costs less gas compared to `i++` or `i += 1` (same for `--i` vs `i--` or `i -= 1`)

Pre-increments and pre-decrements are cheaper.

For a `uint256 i` variable, the following is true with the Optimizer enabled at 10k:

**Increment:**

- `i += 1` is the most expensive form
- `i++` costs 6 gas less than `i += 1`
- `++i` costs 5 gas less than `i++` (11 gas less than `i += 1`)

**Decrement:**

- `i -= 1` is the most expensive form
- `i--` costs 11 gas less than `i -= 1`
- `--i` costs 5 gas less than `i--` (16 gas less than `i -= 1`)

Note that post-increments (or post-decrements) return the old value before incrementing or decrementing, hence the name *post-increment*:

```solidity
uint i = 1;  
uint j = 2;
require(j == i++, "This will be false as i is incremented after the comparison");
```
  
However, pre-increments (or pre-decrements) return the new value:
  
```solidity
uint i = 1;  
uint j = 2;
require(j == ++i, "This will be true as i is incremented before the comparison");
```

In the pre-increment case, the compiler has to create a temporary variable (when used) for returning `1` instead of `2`.

Consider using pre-increments and pre-decrements where they are relevant (meaning: not where post-increments/decrements logic are relevant).

*Saves 5 gas per instance*

*Instances (9)*:

```solidity
File: contracts/governance/twTAP.sol

322:             pool.totalParticipants++; // Save participation

444:             WeekTotals storage next = weekTotals[++week];

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

271:         epoch++;

329:             for (uint256 i; i < _users.length; i++) {

335:             for (uint256 i; i < _users.length; i++) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

268:             pool.totalParticipants++; // Save participation

414:         epoch++;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

344:             for (uint256 i; i < sglLength; i++) {

383:         for (uint256 i; i < len; i++) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="GAS-15"></a>[GAS-15] Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that [returns a tuple](https://github.com/code-423n4/2022-08-frax/blob/90f55a9ce4e25bceed3a74290b854341d8de6afa/src/contracts/FraxlendPair.sol#L156-L178) of the values of all currently-public constants. Saves **3406-3606 gas** in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

*Instances (10)*:

```solidity
File: contracts/governance/twTAP.sol

85:     uint256 public constant EPOCH_DURATION = 7 days;

86:     uint256 public constant MAX_LOCK_DURATION = 100 * 365 days; // 100 years

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

71:     uint256 public constant PHASE_1_DISCOUNT = 500_000; //50 * 1e4; 50%

86:     uint256 public constant PHASE_3_AMOUNT_PER_USER = 714;

87:     uint256 public constant PHASE_3_DISCOUNT = 500_000; //50 * 1e4; 50%

95:     uint256 public constant PHASE_4_DISCOUNT = 330_000; //33 * 1e4;

98:     uint256 public constant LAST_EPOCH = 8; // 8 epochs, 41 days long

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

69:     uint256 public constant MAX_LOCK_DURATION = 100 * 365 days; // 100 years

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

39:     uint256 public constant INITIAL_SUPPLY = 46_686_595 * 1e18; // Everything minus DSO

47:     uint256 public constant EPOCH_DURATION = 1 weeks; // 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="GAS-16"></a>[GAS-16] Use shift right/left instead of division/multiplication if possible

While the `DIV` / `MUL` opcode uses 5 gas, the `SHR` / `SHL` opcode only uses 3 gas. Furthermore, beware that Solidity's division operation also includes a division-by-0 prevention which is bypassed using shifting. Eventually, overflow checks are never performed for shift operations as they are done for arithmetic operations. Instead, the result is always truncated, so the calculation can be unchecked in Solidity version `0.8+`

- Use `>> 1` instead of `/ 2`
- Use `>> 2` instead of `/ 4`
- Use `<< 3` instead of `* 8`
- ...
- Use `>> 5` instead of `/ 2^5 == / 32`
- Use `<< 6` instead of `* 2^6 == * 64`

TL;DR:

- Shifting left by N is like multiplying by 2^N (Each bits to the left is an increased power of 2)
- Shifting right by N is like dividing by 2^N (Each bits to the right is a decreased power of 2)

*Saves around 2 gas + 20 for unchecked per instance*

*Instances (6)*:

```solidity
File: contracts/governance/twTAP.sol

315:         if (magnitude >= pool.cumulative * 4) revert NotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

261:         if (magnitude > pool.cumulative * 4) revert TooLong();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/twAML.sol

148:             uint256 x = y / 2 + 1;

151:                 x = (y / x + x) / 2;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapToken.sol

150:             _mint(_data.dao, 1e18 * 8_000_000);

151:             _mint(_data.airdrop, 1e18 * 2_500_000);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="GAS-17"></a>[GAS-17] Superfluous event fields

`block.timestamp` and `block.number` are added to event information by default so adding them manually wastes gas

*Instances (1)*:

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

107:     event RequestSglPoolRescue(uint256 sglAssetId, uint256 timestamp);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="GAS-18"></a>[GAS-18] `uint256` to `bool` `mapping`: Utilizing Bitmaps to dramatically save on Gas

<https://soliditydeveloper.com/bitmaps>

<https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/structs/BitMaps.sol>

- [BitMaps.sol#L5-L16](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/structs/BitMaps.sol#L5-L16):

```solidity
/**
 * @dev Library for managing uint256 to bool mapping in a compact and efficient way, provided the keys are sequential.
 * Largely inspired by Uniswap's https://github.com/Uniswap/merkle-distributor/blob/master/contracts/MerkleDistributor.sol[merkle-distributor].
 *
 * BitMaps pack 256 booleans across each bit of a single 256-bit slot of `uint256` type.
 * Hence booleans corresponding to 256 _sequential_ indices would only consume a single slot,
 * unlike the regular `bool` which would consume an entire slot for a single value.
 *
 * This results in gas savings in two ways:
 *
 * - Setting a zero value to non-zero only once every 256 times
 * - Accessing the same warm slot for every 256 _sequential_ indices
 */
```

*Instances (1)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

63:     mapping(address => mapping(uint256 => bool)) public userParticipation; // user address => phase => participated

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="GAS-19"></a>[GAS-19] Increments/decrements can be unchecked in for-loops

In Solidity 0.8+, there's a default overflow check on unsigned integers. It's possible to uncheck this in for-loops and save some gas at each iteration, but at the cost of some code readability, as this uncheck cannot be made inline.

[ethereum/solidity#10695](https://github.com/ethereum/solidity/issues/10695)

The change would be:

```diff
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...  
+   unchecked { ++i; }
}  
```

These save around **25 gas saved** per instance.

The same can be applied with decrements (which should use `break` when `i == 0`).

The risk of overflow is non-existent for `uint256`.

*Instances (10)*:

```solidity
File: contracts/governance/twTAP.sol

561:             for (uint256 i; i < len; ++i) {

629:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

329:             for (uint256 i; i < _users.length; i++) {

335:             for (uint256 i; i < _users.length; i++) {

368:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

490:             for (uint256 i; i < len; ++i) {

608:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

144:             for (uint256 i; i < len; ++i) {

344:             for (uint256 i; i < sglLength; i++) {

383:         for (uint256 i; i < len; i++) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="GAS-20"></a>[GAS-20] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (14)*:

```solidity
File: contracts/governance/twTAP.sol

563:                 if (amount > 0) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

325:         activeSingularities[singularity].poolWeight = weight > 0 ? weight : 1;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/twAML.sol

20:             require(denominator > 0);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapToken.sol

388:         if (emissionForWeek[week] > 0) return 0;

392:         if (week > 0) {

404:         if (boostedTAP > 0) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

184:             if (dust > 0) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/Vesting.sol

162:         if (start > 0) revert Initialized();

165:         if (users[_user].amount > 0) revert AlreadyRegistered();

181:         if (start > 0) revert Initialized();

195:             if (users[_users[i]].amount > 0) revert AlreadyRegistered();

220:         if (start > 0) revert Initialized();

232:         if (_initialUnlock > 0) {

266:         if (_cliff > 0) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="GAS-21"></a>[GAS-21] `internal` functions not called by the contract should be removed

If the functions are required by an interface, the contract should inherit from that interface and use the `override` keyword

*Instances (9)*:

```solidity
File: contracts/governance/twTAP.sol

640:     function _getChainId() internal view virtual returns (uint256) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

57:      * @return lockTwTapPositionMsg_ The data of the lock.

77:         uint256 amount = BytesLib.toUint256(BytesLib.slice(_msg, durationOffset_, _msg.length - durationOffset_), 0);

97:      * @return unlockTwTapPositionMsg_ The needed data.

113:         unlockTwTapPositionMsg_ = UnlockTwTapPositionMsg(user_, tokenId_);

133:         return abi.decode(_msg, (RemoteTransferMsg));

140:      *        - lzSendParams::LZSendParam[]: The LZ send params to pass on the remote chain. (B->A)

154:      *        - lzSendParams::LZSendParam[]: The LZ send params to pass on the remote chain. (B->A)

164: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

## Non Critical Issues

| |Issue|Instances|
|-|:-|:-:|
| [NC-1](#NC-1) | Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe | 1 |
| [NC-2](#NC-2) | Missing checks for `address(0)` when assigning values to address state variables | 4 |
| [NC-3](#NC-3) | Array indices should be referenced via `enum`s rather than via numeric literals | 2 |
| [NC-4](#NC-4) | Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked` | 3 |
| [NC-5](#NC-5) | Constants should be in CONSTANT_CASE | 7 |
| [NC-6](#NC-6) | `constant`s should be defined rather than using magic numbers | 50 |
| [NC-7](#NC-7) | Control structures do not follow the Solidity Style Guide | 130 |
| [NC-8](#NC-8) | Default Visibility for constants | 7 |
| [NC-9](#NC-9) | Consider disabling `renounceOwnership()` | 7 |
| [NC-10](#NC-10) | Draft Dependencies | 2 |
| [NC-11](#NC-11) | Unused `error` definition | 2 |
| [NC-12](#NC-12) | Events should use parameters to convey information | 1 |
| [NC-13](#NC-13) | Event missing indexed field | 8 |
| [NC-14](#NC-14) | Events that mark critical parameter changes should contain both the old and the new value | 8 |
| [NC-15](#NC-15) | Function ordering does not follow the Solidity style guide | 8 |
| [NC-16](#NC-16) | Functions should not be longer than 50 lines | 142 |
| [NC-17](#NC-17) | Change int to int256 | 1 |
| [NC-18](#NC-18) | Lack of checks in setters | 15 |
| [NC-19](#NC-19) | Missing Event for critical parameters change | 17 |
| [NC-20](#NC-20) | NatSpec is completely non-existent on functions that should have them | 9 |
| [NC-21](#NC-21) | Incomplete NatSpec: `@param` is missing on actually documented functions | 12 |
| [NC-22](#NC-22) | Incomplete NatSpec: `@return` is missing on actually documented functions | 6 |
| [NC-23](#NC-23) | Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor | 12 |
| [NC-24](#NC-24) | Constant state variables defined more than once | 7 |
| [NC-25](#NC-25) | Consider using named mappings | 24 |
| [NC-26](#NC-26) | Owner can renounce while system is paused | 5 |
| [NC-27](#NC-27) | Adding a `return` statement when the function defines a named return variable, is redundant | 8 |
| [NC-28](#NC-28) | `require()` / `revert()` statements should have descriptive reason strings | 2 |
| [NC-29](#NC-29) | Take advantage of Custom Error's return value property | 138 |
| [NC-30](#NC-30) | Contract does not follow the Solidity style guide's suggested layout ordering | 11 |
| [NC-31](#NC-31) | TODO Left in the code | 2 |
| [NC-32](#NC-32) | Use Underscores for Number Literals (add an underscore every 3 digits) | 7 |
| [NC-33](#NC-33) | Internal and private variables and functions names should begin with an underscore | 18 |
| [NC-34](#NC-34) | Event is missing `indexed` fields | 22 |
| [NC-35](#NC-35) | Constants should be defined rather than using magic numbers | 2 |
| [NC-36](#NC-36) | `public` functions not called by the contract should be declared `external` instead | 7 |

### <a name="NC-1"></a>[NC-1] Replace `abi.encodeWithSignature` and `abi.encodeWithSelector` with `abi.encodeCall` which keeps the code typo/type safe

When using `abi.encodeWithSignature`, it is possible to include a typo for the correct function signature.
When using `abi.encodeWithSignature` or `abi.encodeWithSelector`, it is also possible to provide parameters that are not of the correct type for the function.

To avoid these pitfalls, it would be best to use [`abi.encodeCall`](https://solidity-by-example.org/abi-encode/) instead.

*Instances (1)*:

```solidity
File: contracts/tokens/TapToken.sol

212:             abi.encodeWithSelector(OAppReceiver.lzReceive.selector, _origin, _guid, _message, _executor, _extraData),

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-2"></a>[NC-2] Missing checks for `address(0)` when assigning values to address state variables

*Instances (4)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

138:     event NewEpoch(uint256 indexed epoch, uint256 epochTAPValuation);

368:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

136:         uint256 indexed epoch, uint256 indexed sglAssetID, uint256 totalDeposited, uint256 tokenId, uint256 discount

487:         uint256 len = _paymentTokens.length;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

### <a name="NC-3"></a>[NC-3] Array indices should be referenced via `enum`s rather than via numeric literals

*Instances (2)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

464:         uint128 discount = uint128(PHASE_3_DISCOUNT);

468:     /// @notice Participate in phase 4 of the Airdrop. twTAP and Cassava guild's role are given TAP pro-rata.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="NC-4"></a>[NC-4] Use `string.concat()` or `bytes.concat()` instead of `abi.encodePacked`

Solidity version 0.8.4 introduces `bytes.concat()` (vs `abi.encodePacked(<bytes>,<bytes>)`)

Solidity version 0.8.12 introduces `string.concat()` (vs `abi.encodePacked(<str>,<str>), which catches concatenation errors (in the event of a`bytes`data mixed in the concatenation)`)

*Instances (3)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

417:         bytes32 leaf = keccak256(abi.encodePacked(msg.sender));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

50:             abi.encodePacked(_lockTwTapPositionMsg.user, _lockTwTapPositionMsg.duration, _lockTwTapPositionMsg.amount);

88:         return abi.encodePacked(_msg.user, _msg.tokenId);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

### <a name="NC-5"></a>[NC-5] Constants should be in CONSTANT_CASE

For `constant` variable names, each word should use all capital letters, with underscores separating each word (CONSTANT_CASE)

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

83:     uint256 constant dMAX = 1_000_000; // 100 * 1e4; 0% - 100% voting power multiplier

84:     uint256 constant dMIN = 0;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

77:     uint256 constant dMAX = 500_000; // 50 * 1e4; 0% - 50% discount

78:     uint256 constant dMIN = 0;

79:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

68:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

43:     uint256 constant decay_rate = 8800000000000000; // 0.88%

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-6"></a>[NC-6] `constant`s should be defined rather than using magic numbers

Even [assembly](https://github.com/code-423n4/2022-05-opensea-seaport/blob/9d7ce4d08bf3c3010304a0476a785c70c0e90ae7/contracts/lib/TokenTransferrer.sol#L35-L39) can benefit from using readable constants instead of hex/numeric literals

*Instances (50)*:

```solidity
File: contracts/governance/twTAP.sol

80:     uint256 private VIRTUAL_TOTAL_AMOUNT = 10_000 ether;

82:     uint256 MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

140:         maxRewardTokens = 30;

315:         if (magnitude >= pool.cumulative * 4) revert NotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

79:     uint8[4] public PHASE_2_AMOUNT_PER_USER = [200, 200, 190, 190];

80:     uint24[4] public PHASE_2_DISCOUNT_PER_USER = [500_000, 400_000, 330_000, 250_000];

97:     uint256 public EPOCH_DURATION = 2 days; // Becomes 7 days at the start of the phase 4

211:         } else if (cachedEpoch == 2) {

213:         } else if (cachedEpoch == 3) {

215:         } else if (cachedEpoch >= 4) {

274:         if (epoch == 4) {

275:             EPOCH_DURATION = 7 days;

310:         if (epoch >= 2) revert NotValid();

334:         else if (_phase == 4) {

422:         uint256 subPhase = 20 + _role;

534:         paymentAmount = rawPaymentAmount - muldiv(rawPaymentAmount, _discount, 100e4); // 1e4 is discount decimals, 100 is discount percentage

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

34:         _mint(_lbp, 5_000_000 * 1e18); // 5M LTAP for LBP

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

74:     uint256 private VIRTUAL_TOTAL_AMOUNT = 10_000 ether;

76:     uint256 public MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

261:         if (magnitude > pool.cumulative * 4) revert TooLong();

587:         uint256 discountedOTCAmountInUSD = _otcAmountInUSD - muldiv(_otcAmountInUSD, _discount, 100e4); // 1e4 is discount decimals, 100 is discount percentage

592:         if (_paymentTokenDecimals <= 18) {

595:             paymentAmount = paymentAmount * (10 ** (_paymentTokenDecimals - 18));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

64:     uint256 public rescueCooldown = 2 days; // Cooldown before a singularity pool can be put in rescue mode

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/twAML.sol

94:             uint256 inv = (3 * denominator) ^ 2;

98:             inv *= 2 - denominator * inv; // inverse mod 2**8

99:             inv *= 2 - denominator * inv; // inverse mod 2**16

100:             inv *= 2 - denominator * inv; // inverse mod 2**32

101:             inv *= 2 - denominator * inv; // inverse mod 2**64

102:             inv *= 2 - denominator * inv; // inverse mod 2**128

103:             inv *= 2 - denominator * inv; // inverse mod 2**256

146:         if (y > 3) {

148:             uint256 x = y / 2 + 1;

151:                 x = (y / x + x) / 2;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapToken.sol

40:     uint256 public dso_supply = 53_313_405 * 1e18; // Emission supply for DSO

146:             _mint(_data.contributors, 1e18 * 15_000_000);

147:             _mint(_data.earlySupporters, 1e18 * 3_686_595);

148:             _mint(_data.supporters, 1e18 * 12_500_000);

149:             _mint(_data.aoTap, 1e18 * 5_000_000);

150:             _mint(_data.dao, 1e18 * 8_000_000);

151:             _mint(_data.airdrop, 1e18 * 2_500_000);

291:         return 18;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

69:         uint8 userOffset_ = 20;

70:         uint8 durationOffset_ = 32;

105:         uint8 userOffset_ = 20;

110:         uint256 tokenId_ = BytesLib.toUint256(BytesLib.slice(_msg, userOffset_, 32), 0);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

```solidity
File: contracts/tokens/Vesting.sol

231:         if (_initialUnlock > 10_000) revert AmountNotValid();

233:             uint256 initialUnlockAmount = (_seededAmount * _initialUnlock) / 10_000;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

75:         if (_returnData.length > 1000) return "Module: reason too long";

78:         if (_returnData.length < 68) return "Module: data";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-7"></a>[NC-7] Control structures do not follow the Solidity Style Guide

See the [control structures](https://docs.soliditylang.org/en/latest/style-guide.html#control-structures) section of the Solidity Style Guide

*Instances (130)*:

```solidity
File: contracts/governance/twTAP.sol

299:         if (_duration < EPOCH_DURATION) revert LockNotAWeek();

300:         if (_duration > MAX_LOCK_DURATION) revert LockTooLong();

301:         if (_timestampToWeek(block.timestamp) > currentWeek()) revert AdvanceEpochFirst();

307:             if (isErr) revert NotAuthorized();

315:         if (magnitude >= pool.cumulative * 4) revert NotValid();

464:         if (lastProcessedWeek != currentWeek()) revert AdvanceWeekFirst();

465:         if (_amount == 0) revert NotValid();

466:         if (_rewardTokenId == 0) revert NotValid(); // @dev rewardTokens[0] is 0x0

512:         if (rewardTokenIndex[_token] != 0) revert Registered();

582:         if (position.expiry > block.timestamp) revert LockNotExpired();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

144:         if (address(tapOracle) == address(0)) revert TapOracleNotSet();

145:         if (address(tapToken) == address(0)) revert TapNotSet();

170:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

183:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

186:         if (tapAmount < 1e18) revert TooLow();

205:         if (cachedEpoch == 0) revert NotStarted();

206:         if (cachedEpoch > LAST_EPOCH) revert Ended();

233:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

251:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

254:         if (chosenAmount < 1e18) revert TooLow();

280:         if (!success) revert Failed();

295:         if (address(tapToken) != address(0)) revert NotValid();

310:         if (epoch >= 2) revert NotValid();

325:         if (_users.length != _amounts.length) revert NotValid();

328:             if (epoch >= 1) revert NotValid();

378:         if (epoch <= LAST_EPOCH) revert TooSoon();

401:         if (_eligibleAmount == 0) revert NotEligible();

445:             if (PCNFT.ownerOf(_tokenIDs[i]) != msg.sender) revert NotEligible();

471:         if (_eligibleAmount == 0) revert NotEligible();

492:         if (tapAmount == 0) revert TapAmountNotValid();

499:         if (!success) revert Failed();

504:         if (discountedPaymentAmount == 0) revert PaymentAmountNotValid();

511:             if (isErr) revert Failed();

514:         if (balAfter - balBefore != discountedPaymentAmount) revert Failed();

531:         if (_paymentTokenValuation == 0) revert PaymentTokenValuationNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

38:         if (address(tapToken) == address(0)) revert TapNotSet();

53:         if (!openRedemption) revert RedemptionNotOpen();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

83:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

95:         if (msg.sender != broker) revert OnlyBroker();

110:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

118:         if (broker != address(0)) revert OnlyOnce();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

122:         if (_epochDuration != TapiocaOptionLiquidityProvision(_tOLP).EPOCH_DURATION()) revert NotEqualDurations();

155:         if (timestamp < emissionsStartTime) return 0;

183:             if (!_isPositionActive(tOLPLockPosition)) revert OptionExpired();

202:             if (netAmount == 0) revert NoLiquidity();

206:             if (eligibleTapAmount < _tapAmount) revert TooHigh();

210:         if (tapAmount < 1e18) revert TooLow();

233:         if (block.timestamp >= lockExpiry) revert LockExpired();

234:         if (_timestampToWeek(block.timestamp) > epoch) revert AdvanceEpochFirst();

237:         if (!isPositionActive) revert OptionExpired();

239:         if (lock.lockDuration < EPOCH_DURATION) revert DurationTooShort();

254:             if (isErr) revert TransferFailed();

261:         if (magnitude > pool.cumulative * 4) revert TooLong();

308:         if (!oTAP.exists(_oTAPTokenID)) revert PositionNotValid();

370:         if (!isPositionActive) revert OptionExpired();

390:         if (netAmount == 0) revert NoLiquidity();

393:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

396:         if (chosenAmount < 1e18) revert TooLow();

409:         if (_timestampToWeek(block.timestamp) <= epoch) revert TooSoon();

412:         if (singularities.length == 0) revert NoActiveSingularities();

423:         if (!success) revert Failed();

528:         if (_lock.lockTime == 0) revert PositionNotValid();

529:         if (_isSGLInRescueMode(_lock)) revert SingularityInRescueMode();

552:         if (!success) revert Failed();

563:             if (isErr) revert TransferFailed();

585:         if (_paymentTokenValuation == 0) revert PaymentTokenValuationNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

41:     bool rescue; // If true, the pool will be used to rescue funds in case of emergency

192:         if (_lockDuration < EPOCH_DURATION) revert DurationTooShort();

193:         if (_lockDuration > MAX_LOCK_DURATION) revert DurationTooLong();

195:         if (_ybShares == 0) revert SharesNotValid();

198:         if (sgl.rescue) revert SingularityInRescueMode();

201:         if (sglAssetID == 0) revert SingularityNotActive();

233:         if (!_exists(_tokenId)) revert PositionExpired();

241:             if (block.timestamp < lockPosition.lockTime + lockPosition.lockDuration) revert LockNotExpired();

247:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

284:         if (_sglAssetID == 0) revert NotRegistered();

285:         if (sglRescueRequest[_sglAssetID] != 0) revert AlreadyActive();

297:         if (sgl.sglAssetID == 0) revert NotRegistered();

298:         if (sgl.rescue) revert AlreadyActive();

299:         if (sglRescueRequest[sgl.sglAssetID] == 0) revert NotActive();

300:         if (block.timestamp < sglRescueRequest[sgl.sglAssetID] + rescueCooldown) revert RescueCooldownNotReached();

316:         if (assetID == 0) revert AssetIdNotValid();

336:         if (sglAssetID == 0) revert NotRegistered();

337:         if (!activeSingularities[singularity].rescue) revert NotInRescueMode();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

84:         if (msg.sender != broker) revert OnlyBroker();

100:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

108:         if (broker != address(0)) revert OnlyOnce();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/options/twAML.sol

27:             uint256 prod0; // Least significant 256 bits of the product

28:             uint256 prod1; // Most significant 256 bits of the product

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

41:         if (address(twTap) == address(0)) revert twTapNotSet();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

98:         if (msg.sender != minter) revert OnlyMinter();

103:         if (_getChainId() != governanceEid) revert OnlyHostChain();

141:         if (_data.endpoint == address(0)) revert AddressWrong();

152:             if (totalSupply() != INITIAL_SUPPLY) revert SupplyNotValid();

157:         if (_data.tapTokenSenderModule == address(0)) revert NotValid();

158:         if (_data.tapTokenReceiverModule == address(0)) revert NotValid();

316:         if (timestamp < emissionsStartTime) return 0;

353:         if (_amount == 0) revert NotValid();

385:         if (_getChainId() != governanceEid) revert NotValid();

388:         if (emissionForWeek[week] > 0) return 0;

425:         if (_minter == address(0)) revert NotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

159:         if (

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/Vesting.sol

32:     uint256 public immutable cliff;

85:         if (_duration == 0) revert VestingDurationNotValid();

87:         cliff = _cliff;

142:         if (start == 0) revert NotStarted();

144:         if (_claimable == 0) revert NothingToClaim();

162:         if (start > 0) revert Initialized();

163:         if (_user == address(0)) revert AddressNotValid();

164:         if (_amount == 0) revert AmountNotValid();

165:         if (users[_user].amount > 0) revert AlreadyRegistered();

181:         if (start > 0) revert Initialized();

182:         if (_users.length != _amounts.length) revert("Lengths not equal");

193:             if (_users[i] == address(0)) revert AddressNotValid();

194:             if (_amounts[i] == 0) revert AmountNotValid();

195:             if (users[_users[i]].amount > 0) revert AlreadyRegistered();

210:         if (_cachedTotalAmount > _totalAmount) revert Overflow();

220:         if (start > 0) revert Initialized();

221:         if (_seededAmount == 0) revert NoTokens();

222:         if (totalRegisteredAmount > _seededAmount) revert NotEnough();

226:         if (availableToken < _seededAmount) revert BalanceTooLow();

231:         if (_initialUnlock > 10_000) revert AmountNotValid();

260:         uint256 _cliff = cliff;

264:         if (_start == 0) return 0; // Not started

267:             _start = _start + _cliff; // Apply cliff offset

268:             if (block.timestamp < _start) return 0; // Cliff not reached

271:         if (block.timestamp >= _start + _duration) return _totalAmount; // Fully vested

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

43:         if (module == address(0)) revert ModuleManager__ModuleNotAuthorized();

75:         if (_returnData.length > 1000) return "Module: reason too long";

78:         if (_returnData.length < 68) return "Module: data";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-8"></a>[NC-8] Default Visibility for constants

Some constants are using the default visibility. For readability, consider explicitly declaring them as `internal`.

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

83:     uint256 constant dMAX = 1_000_000; // 100 * 1e4; 0% - 100% voting power multiplier

84:     uint256 constant dMIN = 0;

97:     uint256 constant DIST_PRECISION = 2 ** 128; //2 ** 128;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

77:     uint256 constant dMAX = 500_000; // 50 * 1e4; 0% - 50% discount

78:     uint256 constant dMIN = 0;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/tokens/TapToken.sol

43:     uint256 constant decay_rate = 8800000000000000; // 0.88%

44:     uint256 constant DECAY_RATE_DECIMAL = 1e18;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-9"></a>[NC-9] Consider disabling `renounceOwnership()`

If the plan for your project does not include eventually giving up all ownership control, consider overwriting OpenZeppelin's `Ownable`'s `renounceOwnership()` function in order to disable it.

*Instances (7)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

24: contract ERC721NftLoader is ERC721, Ownable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

69: contract TwTAP is TWAML, ERC721, ERC721Permit, Ownable, PearlmitHandler, ERC721NftLoader, ReentrancyGuard, Pausable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

43: contract AirdropBroker is Pausable, Ownable, PearlmitHandler, FullMath, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

21: contract LTap is Ownable, ERC20Permit {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

30: contract AOTAP is Ownable, PearlmitHandler, ERC721, ERC721Permit, BaseBoringBatchable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

50: contract TapiocaOptionBroker is Pausable, Ownable, PearlmitHandler, IERC721Receiver, TWAML, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/tokens/Vesting.sol

19: contract Vesting is Ownable, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="NC-10"></a>[NC-10] Draft Dependencies

Draft contracts have not received adequate security auditing or are liable to change with future developments.

*Instances (2)*:

```solidity
File: contracts/option-airdrop/LTap.sol

4: import {ERC20, ERC20Permit} from "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/tokens/TapToken.sol

11: import {ERC20Permit, ERC20} from "@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-11"></a>[NC-11] Unused `error` definition

Note that there may be cases where an error superficially appears to be used, but this is only because there are multiple definitions of the error in different files. In such cases, the error definition should be moved into a separate file. The instances below are the unused definitions.

*Instances (2)*:

```solidity
File: contracts/governance/twTAP.sol

138:         rewardTokens.push(IERC20(address(0x0))); // 0 index is reserved

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

109:      * @dev The initial supply of 100M is not minted here as we have the wrap method

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-12"></a>[NC-12] Events should use parameters to convey information

For example, rather than using `event Paused()` and `event Unpaused()`, use `event PauseState(address indexed whoChangedIt, bool wasPaused, bool isNowPaused)`

*Instances (1)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

155:     /// @param _aoTAPTokenID The aoTAP token ID

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="NC-13"></a>[NC-13] Event missing indexed field

Index event fields make the field more quickly accessible [to off-chain tools](https://ethereum.stackexchange.com/questions/40396/can-somebody-please-explain-the-concept-of-event-indexing) that parse events. This is especially useful when it comes to filtering based on an address. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Where applicable, each `event` should use three `indexed` fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three applicable fields, all of the applicable fields should be indexed.

*Instances (8)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

153:     /// @notice Returns the details of an OTC deal for a given oTAP token ID and a payment token.

154:     ///         The oracle uses the last peeked value, and not the latest one, so the payment amount may be different.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

115:     modifier updateTotalSGLPoolWeights() {

118:         emit UpdateTotalSingularityPoolWeights(totalSingularityPoolWeights);

124:     /// @notice Returns the lock position of a given tOLP NFT and if it's active

126:     function getLock(uint256 _tokenId) external view returns (LockPosition memory) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

84:     // *ERRORS*

98:         if (msg.sender != minter) revert OnlyMinter();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-14"></a>[NC-14] Events that mark critical parameter changes should contain both the old and the new value

This should especially be done if the new value is not required to be different from the old value

*Instances (8)*:

```solidity
File: contracts/governance/twTAP.sol

512:         if (rewardTokenIndex[_token] != 0) revert Registered();
             if (rewardTokens.length + 1 > maxRewardTokens) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

316:      * @notice Register users for a phase 1 or 4 with their eligible amount.
          * @param _phase The phase to register the users for
          * @param _users The users to register
          * @param _amounts The eligible amount of TAP for each user

321:     function registerUsersForPhase(uint256 _phase, address[] calldata _users, uint256[] calldata _amounts)
             external
             onlyOwner
         {
             if (_users.length != _amounts.length) revert NotValid();

355:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;
         }
     
         /// @notice Collect the payment tokens from the OTC deals
         /// @param _paymentTokens The payment tokens to collect
         function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

464:     function setPaymentToken(ERC20 _paymentToken, ITapiocaOracle _oracle, bytes calldata _oracleData)
             external
             onlyOwner
         {
             paymentTokens[_paymentToken].oracle = _oracle;
             paymentTokens[_paymentToken].oracleData = _oracleData;
     
             emit SetPaymentToken(_paymentToken, _oracle, _oracleData);

476:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;
         }
     
         /// @notice Collect the payment tokens from the OTC deals
         /// @param _paymentTokens The payment tokens to collect
         function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

280:      * @notice Requests a singularity market to be put in rescue mode. Needs to be activated later on in `activateSGLPoolRescue()`
          * @param _sglAssetID YieldBox asset ID of the singularity market
          */
         function requestSglPoolRescue(uint256 _sglAssetID) external onlyOwner {
             if (_sglAssetID == 0) revert NotRegistered();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

441:      * @notice Un/Pauses this contract.
          */
         function setPause(bool _pauseState) external onlyOwner {
             if (_pauseState) {
                 _pause();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-15"></a>[NC-15] Function ordering does not follow the Solidity style guide

According to the [Solidity style guide](https://docs.soliditylang.org/en/v0.8.17/style-guide.html#order-of-functions), functions should be laid out in the following order :`constructor()`, `receive()`, `fallback()`, `external`, `public`, `internal`, `private`, but the cases below do not follow this pattern

*Instances (8)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

1: 
   Current order:
   public tokenURI
   external setNftLoader
   
   Suggested order:
   external setNftLoader
   public tokenURI

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

1: 
   Current order:
   public tokenURI
   external getRewardTokens
   public currentWeek
   external getParticipation
   public claimable
   public getTypedDataHash
   external participate
   external claimRewards
   external exitPosition
   public advanceWeek
   external distributeReward
   external setVirtualTotalAmount
   external setMinWeightFactor
   external setMaxRewardTokensLength
   external addRewardToken
   external setPause
   internal _timestampToWeek
   internal _requireClaimPermission
   internal _claimRewards
   internal _releaseTap
   internal _existInArray
   internal _getChainId
   public supportsInterface
   
   Suggested order:
   external getRewardTokens
   external getParticipation
   external participate
   external claimRewards
   external exitPosition
   external distributeReward
   external setVirtualTotalAmount
   external setMinWeightFactor
   external setMaxRewardTokensLength
   external addRewardToken
   external setPause
   public tokenURI
   public currentWeek
   public claimable
   public getTypedDataHash
   public advanceWeek
   public supportsInterface
   internal _timestampToWeek
   internal _requireClaimPermission
   internal _claimRewards
   internal _releaseTap
   internal _existInArray
   internal _getChainId

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

1: 
   Current order:
   public tokenURI
   external isApprovedOrOwner
   external attributes
   external exists
   external setTokenURI
   external mint
   external burn
   external brokerClaim
   
   Suggested order:
   external isApprovedOrOwner
   external attributes
   external exists
   external setTokenURI
   external mint
   external burn
   external brokerClaim
   public tokenURI

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

1: 
   Current order:
   external timestampToWeek
   external getCurrentWeek
   external getOTCDealDetails
   external participate
   external exitPosition
   external exerciseOption
   external newEpoch
   external oTAPBrokerClaim
   external setVirtualTotalAmount
   external setMinWeightFactor
   external setTapOracle
   external setPaymentToken
   external setPaymentTokenBeneficiary
   external collectPaymentTokens
   external setPause
   internal _timestampToWeek
   internal _isSGLInRescueMode
   internal _isPositionActive
   internal _processOTCDeal
   internal _getDiscountedPaymentAmount
   internal _emitToGauges
   external onERC721Received
   
   Suggested order:
   external timestampToWeek
   external getCurrentWeek
   external getOTCDealDetails
   external participate
   external exitPosition
   external exerciseOption
   external newEpoch
   external oTAPBrokerClaim
   external setVirtualTotalAmount
   external setMinWeightFactor
   external setTapOracle
   external setPaymentToken
   external setPaymentTokenBeneficiary
   external collectPaymentTokens
   external setPause
   external onERC721Received
   internal _timestampToWeek
   internal _isSGLInRescueMode
   internal _isPositionActive
   internal _processOTCDeal
   internal _getDiscountedPaymentAmount
   internal _emitToGauges

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

1: 
   Current order:
   external getLock
   external getSingularities
   external getSingularityPools
   external getTotalPoolDeposited
   external isApprovedOrOwner
   internal _isApprovedOrOwner
   external lock
   external unlock
   external setSGLPoolWeight
   external setRescueCooldown
   external requestSglPoolRescue
   external activateSGLPoolRescue
   external registerSingularity
   external unregisterSingularity
   external setPause
   internal _computeSGLPoolWeights
   public supportsInterface
   external onERC1155Received
   external onERC1155BatchReceived
   
   Suggested order:
   external getLock
   external getSingularities
   external getSingularityPools
   external getTotalPoolDeposited
   external isApprovedOrOwner
   external lock
   external unlock
   external setSGLPoolWeight
   external setRescueCooldown
   external requestSglPoolRescue
   external activateSGLPoolRescue
   external registerSingularity
   external unregisterSingularity
   external setPause
   external onERC1155Received
   external onERC1155BatchReceived
   public supportsInterface
   internal _isApprovedOrOwner
   internal _computeSGLPoolWeights

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

1: 
   Current order:
   public tokenURI
   external isApprovedOrOwner
   external attributes
   external exists
   external mint
   external burn
   external brokerClaim
   
   Suggested order:
   external isApprovedOrOwner
   external attributes
   external exists
   external mint
   external burn
   external brokerClaim
   public tokenURI

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

1: 
   Current order:
   public transferFrom
   public lzReceive
   external executeModule
   public sendPacket
   public decimals
   external getCurrentWeek
   external getCurrentWeekEmission
   external timestampToWeek
   public getTypedDataHash
   external extractTAP
   external removeTAP
   external emitForWeek
   external setMinter
   external setTwTAP
   external setPause
   internal _timestampToWeek
   internal _computeEmission
   internal _getChainId
   
   Suggested order:
   external executeModule
   external getCurrentWeek
   external getCurrentWeekEmission
   external timestampToWeek
   external extractTAP
   external removeTAP
   external emitForWeek
   external setMinter
   external setTwTAP
   external setPause
   public transferFrom
   public lzReceive
   public sendPacket
   public decimals
   public getTypedDataHash
   internal _timestampToWeek
   internal _computeEmission
   internal _getChainId

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

1: 
   Current order:
   external claimable
   public claimable
   external vested
   external vested
   external totalClaimed
   external computeTimeFromAmount
   external claim
   external registerUser
   external registerUsers
   external init
   internal _computeTimeFromAmount
   internal _vested
   
   Suggested order:
   external claimable
   external vested
   external vested
   external totalClaimed
   external computeTimeFromAmount
   external claim
   external registerUser
   external registerUsers
   external init
   public claimable
   internal _computeTimeFromAmount
   internal _vested

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="NC-16"></a>[NC-16] Functions should not be longer than 50 lines

Overly complex code can make understanding functionality more difficult, try to further modularize your code to ensure readability

*Instances (142)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

35:     function tokenURI(uint256 tokenId) public view virtual override returns (string memory) {

45:     function setNftLoader(address _nftLoader) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

162:     function tokenURI(uint256 tokenId) public view override(ERC721, ERC721NftLoader) returns (string memory) {

169:     function getRewardTokens() external view returns (IERC20[] memory) {

173:     function currentWeek() public view returns (uint256) {

178:     function getParticipation(uint256 _tokenId) external view returns (Participation memory participant) {

192:     function claimable(uint256 _tokenId) public view returns (uint256[] memory) {

272:     function getTypedDataHash(ERC721PermitStruct calldata _permitData) public view returns (bytes32) {

293:     function participate(address _participant, uint256 _amount, uint256 _duration)

396:     function claimRewards(uint256 _tokenId, address _to)

414:     function exitPosition(uint256 _tokenId, address _to)

432:     function advanceWeek(uint256 _limit) public nonReentrant {

463:     function distributeReward(uint256 _rewardTokenId, uint256 _amount) external nonReentrant {

489:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

497:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

501:     function setMaxRewardTokensLength(uint256 _length) external onlyOwner {

511:     function addRewardToken(IERC20 _token) external onlyOwner returns (uint256) {

527:     function setPause(bool _pauseState) external onlyOwner {

540:     function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {

547:     function _requireClaimPermission(address _to, uint256 _tokenId) internal view {

557:     function _claimRewards(uint256 _tokenId, address _to) internal returns (uint256[] memory amounts_) {

580:     function _releaseTap(uint256 _tokenId, address _to) internal returns (uint256 releasedAmount) {

626:     function _existInArray(address _check, address[] memory _array) internal pure returns (bool) {

640:     function _getChainId() internal view virtual returns (uint256) {

647:     function supportsInterface(bytes4 interfaceId) public view virtual override(ERC721) returns (bool) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

162:     function getOTCDealDetails(uint256 _aoTAPTokenID, ERC20 _paymentToken, uint256 _tapAmount)

203:     function participate(bytes calldata _data) external whenNotPaused tapExists returns (uint256 aoTAPTokenID) {

226:     function exerciseOption(uint256 _aoTAPTokenID, ERC20 _paymentToken, uint256 _tapAmount)

294:     function setTapToken(address payable _tapToken) external onlyOwner {

302:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

309:     function setPhase2MerkleRoots(bytes32[4] calldata _merkleRoots) external onlyOwner {

321:     function registerUsersForPhase(uint256 _phase, address[] calldata _users, uint256[] calldata _amounts)

343:     function setPaymentToken(ERC20 _paymentToken, ITapiocaOracle _oracle, bytes calldata _oracleData)

355:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

361:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

386:     function setPause(bool _pauseState) external onlyOwner {

399:     function _participatePhase1() internal returns (uint256 oTAPTokenID) {

414:     function _participatePhase2(bytes calldata _data) internal returns (uint256 oTAPTokenID) {

439:     function _participatePhase3(bytes calldata _data) internal returns (uint256 oTAPTokenID) {

469:     function _participatePhase4() internal returns (uint256 oTAPTokenID) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

44:     function setTapToken(address _tapToken) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

59:     function tokenURI(uint256 _tokenId) public view override returns (string memory) {

63:     function isApprovedOrOwner(address _spender, uint256 _tokenId) external view returns (bool) {

69:     function attributes(uint256 _tokenId) external view returns (address, AirdropTapOption memory) {

74:     function exists(uint256 _tokenId) external view returns (bool) {

82:     function setTokenURI(uint256 _tokenId, string calldata _tokenURI) external {

91:     function mint(address _to, uint128 _expiry, uint128 _discount, uint256 _amount)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

151:     function timestampToWeek(uint256 timestamp) external view returns (uint256) {

161:     function getCurrentWeek() external view returns (uint256) {

173:     function getOTCDealDetails(uint256 _oTAPTokenID, ERC20 _paymentToken, uint256 _tapAmount)

228:     function participate(uint256 _tOLPTokenID) external whenNotPaused nonReentrant returns (uint256 oTAPTokenID) {

307:     function exitPosition(uint256 _oTAPTokenID) external whenNotPaused {

365:     function exerciseOption(uint256 _oTAPTokenID, ERC20 _paymentToken, uint256 _tapAmount) external whenNotPaused {

440:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

448:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

455:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

464:     function setPaymentToken(ERC20 _paymentToken, ITapiocaOracle _oracle, bytes calldata _oracleData)

476:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

482:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

500:     function setPause(bool _pauseState) external onlyOwner {

512:     function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {

518:     function _isSGLInRescueMode(LockPosition memory _lock) internal view returns (bool) {

527:     function _isPositionActive(LockPosition memory _lock) internal view returns (bool isPositionActive) {

601:     function _emitToGauges(uint256 _epochTAP) internal {

626:     function onERC721Received(address operator, address from, uint256 tokenId, bytes calldata data)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

126:     function getLock(uint256 _tokenId) external view returns (LockPosition memory) {

132:     function getSingularities() external view returns (uint256[] memory) {

138:     function getSingularityPools() external view returns (SingularityPool[] memory) {

159:     function getTotalPoolDeposited(uint256 _sglAssetId) external view returns (uint256 shares, uint256 amount) {

167:     function isApprovedOrOwner(address _spender, uint256 _tokenId) external view returns (bool) {

172:     function _isApprovedOrOwner(address _spender, uint256 _tokenId) internal view override returns (bool) {

187:     function lock(address _to, IERC20 _singularity, uint128 _lockDuration, uint128 _ybShares)

232:     function unlock(uint256 _tokenId, IERC20 _singularity, address _to) external {

266:     function setSGLPoolWeight(IERC20 singularity, uint256 weight) external onlyOwner updateTotalSGLPoolWeights {

275:     function setRescueCooldown(uint256 _rescueCooldown) external onlyOwner {

283:     function requestSglPoolRescue(uint256 _sglAssetID) external onlyOwner {

294:     function activateSGLPoolRescue(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

311:     function registerSingularity(IERC20 singularity, uint256 assetID, uint256 weight)

334:     function unregisterSingularity(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

366:     function setPause(bool _pauseState) external onlyOwner {

380:     function _computeSGLPoolWeights() internal view returns (uint256) {

393:     function supportsInterface(bytes4 interfaceId) public view override(ERC721, IERC165) returns (bool) {

397:     function onERC1155Received(address operator, address from, uint256 id, uint256 value, bytes calldata data)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

56:     function tokenURI(uint256 tokenId) public view override(ERC721, ERC721NftLoader) returns (string memory) {

60:     function isApprovedOrOwner(address _spender, uint256 _tokenId) external view returns (bool) {

65:     function attributes(uint256 _tokenId) external view returns (address, TapOption memory) {

70:     function exists(uint256 _tokenId) external view returns (bool) {

83:     function mint(address _to, uint128 _expiry, uint128 _discount, uint256 _tOLP) external returns (uint256 tokenId) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/options/twAML.sol

17:     function muldiv(uint256 a, uint256 b, uint256 denominator) internal pure returns (uint256 result) {

122:     function computeMinWeight(uint256 _totalWeight, uint256 _minWeightFactor) internal pure returns (uint256) {

127:     function computeMagnitude(uint256 _timeWeight, uint256 _cumulative) internal pure returns (uint256) {

131:     function computeTarget(uint256 _dMin, uint256 _dMax, uint256 _magnitude, uint256 _cumulative)

145:     function sqrt(uint256 y) internal pure returns (uint256 z) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

48:     function setTwTAP(address _twTap) external virtual {}

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

167:     function transferFrom(address from, address to, uint256 value)

229:     function executeModule(ITapToken.Module _module, bytes memory _data, bool _forwardRevert)

268:     function sendPacket(LZSendParam calldata _lzSendParam, bytes calldata _composeMsg)

290:     function decimals() public pure override returns (uint8) {

297:     function getCurrentWeek() external view returns (uint256) {

304:     function getCurrentWeekEmission() external view returns (uint256) {

312:     function timestampToWeek(uint256 timestamp) external view returns (uint256) {

325:     function getTypedDataHash(ERC20PermitStruct calldata _permitData) public view returns (bytes32) {

352:     function extractTAP(address _to, uint256 _amount) external onlyMinter whenNotPaused {

368:     function removeTAP(uint256 _amount) external whenNotPaused {

384:     function emitForWeek() external onlyMinter returns (uint256) {

424:     function setMinter(address _minter) external onlyOwner {

433:     function setTwTAP(address _twTap) external override onlyOwner onlyHostChain {

443:     function setPause(bool _pauseState) external onlyOwner {

459:     function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {

466:     function _computeEmission() internal view returns (uint256 result) {

473:     function _getChainId() internal view override returns (uint32) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

44:     function buildLockTwTapPositionMsg(LockTwTapPositionMsg memory _lockTwTapPositionMsg)

62:     function decodeLockTwpTapDstMsg(bytes memory _msg)

87:     function buildUnlockTwTapPositionMsg(UnlockTwTapPositionMsg memory _msg) internal pure returns (bytes memory) {

99:     function decodeUnlockTwTapPositionMsg(bytes memory _msg)

120:     function buildRemoteTransferMsg(RemoteTransferMsg memory _remoteTransferMsg) internal pure returns (bytes memory) {

128:     function decodeRemoteTransferMsg(bytes memory _msg)

142:     function buildClaimTwTapRewards(ClaimTwTapRewardsMsg memory _claimTwTapRewardsMsg)

156:     function decodeClaimTwTapRewardsMsg(bytes memory _msg)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

83:     function _toeComposeReceiver(uint16 _msgType, address _srcChainSender, bytes memory _toeComposeMsg)

113:     function _lockTwTapPositionReceiver(address _srcChainSender, bytes memory _data) internal virtual twTapExists {

137:     function _unlockTwTapPositionReceiver(bytes memory _data) internal virtual twTapExists {

152:     function _claimTwpTapRewardsReceiver(bytes memory _data) internal virtual twTapExists {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/Vesting.sol

97:     function claimable() external view returns (uint256) {

103:     function claimable(address _user) public view returns (uint256) {

108:     function vested() external view returns (uint256) {

114:     function vested(address _user) external view returns (uint256) {

119:     function totalClaimed() external view returns (uint256) {

128:     function computeTimeFromAmount(uint256 _start, uint256 _totalAmount, uint256 _amount, uint256 _duration)

161:     function registerUser(address _user, uint256 _amount) external onlyOwner {

180:     function registerUsers(address[] calldata _users, uint256[] calldata _amounts) external onlyOwner {

219:     function init(IERC20 _token, uint256 _seededAmount, uint256 _initialUnlock) external onlyOwner {

245:     function _computeTimeFromAmount(uint256 _start, uint256 _totalAmount, uint256 _amount, uint256 _duration)

259:     function _vested(uint256 _totalAmount) internal view returns (uint256) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/extensions/TapTokenHelper.sol

40:     function buildLockTwTapPositionMsg(LockTwTapPositionMsg calldata _lockTwTapPositionMsg)

52:     function buildUnlockTwpTapPositionMsg(UnlockTwTapPositionMsg memory _unlockTwTapPositionMsg)

71:     function buildClaimRewardsMsg(ClaimTwTapRewardsMsg memory _claimTwTapRewardsMsg)

82:     function _sanitizeMsgTypeExtended(uint16 _msgType) internal pure override returns (bool) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/extensions/TapTokenHelper.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

33:     function _setModule(uint8 _module, address _moduleAddress) internal {

41:     function _extractModule(uint8 _module) internal view returns (address) {

57:     function _executeModule(uint8 _module, bytes memory _data, bool _forwardRevert)

74:     function _getRevertMsg(bytes memory _returnData) internal pure returns (string memory) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-17"></a>[NC-17] Change int to int256

Throughout the code base, some variables are declared as `int`. To favor explicitness, consider changing all instances of `int` to `int256`

*Instances (1)*:

```solidity
File: contracts/tokens/TapToken.sol

141:         if (_data.endpoint == address(0)) revert AddressWrong();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-18"></a>[NC-18] Lack of checks in setters

Be it sanity checks (like checks against `0`-values) or initial setting checks: it's best for Setter functions to have them

*Instances (15)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

49: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

502:         emit LogMaxRewardsLength(maxRewardTokens, _length, rewardTokens.length);
             maxRewardTokens = _length;

510:     // TODO Check if it should be one type of token only? Like OFT?
         function addRewardToken(IERC20 _token) external onlyOwner returns (uint256) {

512:         if (rewardTokenIndex[_token] != 0) revert Registered();
             if (rewardTokens.length + 1 > maxRewardTokens) {
                 revert TokenLimitReached();
             }
             rewardTokens.push(_token);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

316:      * @notice Register users for a phase 1 or 4 with their eligible amount.
          * @param _phase The phase to register the users for
          * @param _users The users to register
          * @param _amounts The eligible amount of TAP for each user

355:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;
         }
     
         /// @notice Collect the payment tokens from the OTC deals
         /// @param _paymentTokens The payment tokens to collect
         function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

365:         uint256 len = _paymentTokens.length;
     
             unchecked {
                 for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

59: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

453:     /// @param _tapOracle The new TapOFT Oracle address
         /// @param _tapOracleData The new TapOFT Oracle data
         function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

459:         emit SetTapOracle(_tapOracle, _tapOracleData);
         }
     
         /// @notice Activate or deactivate a payment token

464:     function setPaymentToken(ERC20 _paymentToken, ITapiocaOracle _oracle, bytes calldata _oracleData)
             external
             onlyOwner
         {
             paymentTokens[_paymentToken].oracle = _oracle;
             paymentTokens[_paymentToken].oracleData = _oracleData;
     
             emit SetPaymentToken(_paymentToken, _oracle, _oracleData);

476:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;
         }
     
         /// @notice Collect the payment tokens from the OTC deals
         /// @param _paymentTokens The payment tokens to collect
         function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

484:         if (_paymentTokenBeneficiary == address(0)) {
                 revert PaymentTokenNotSupported();
             }
             uint256 len = _paymentTokens.length;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

285:         if (sglRescueRequest[_sglAssetID] != 0) revert AlreadyActive();
     
             sglRescueRequest[_sglAssetID] = block.timestamp;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

48:     /**
         * @notice Execute an call to a given module.
         *
         * @param _module The module to execute.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-19"></a>[NC-19] Missing Event for critical parameters change

Events help non-contract tools to track changes, and events prevent users from being surprised by changes.

*Instances (17)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

49: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

502:         emit LogMaxRewardsLength(maxRewardTokens, _length, rewardTokens.length);
             maxRewardTokens = _length;

510:     // TODO Check if it should be one type of token only? Like OFT?
         function addRewardToken(IERC20 _token) external onlyOwner returns (uint256) {

547:     function _requireClaimPermission(address _to, uint256 _tokenId) internal view {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

304:         tapOracleData = _tapOracleData;
     
             emit SetTapOracle(_tapOracle, _tapOracleData);
         }
     
         function setPhase2MerkleRoots(bytes32[4] calldata _merkleRoots) external onlyOwner {

365:         uint256 len = _paymentTokens.length;
     
             unchecked {
                 for (uint256 i; i < len; ++i) {

401:         if (_eligibleAmount == 0) revert NotEligible();
     
             // Close eligibility
             phase1Users[msg.sender] = 0;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

59: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

94:     {
            if (msg.sender != broker) revert OnlyBroker();
    
            tokenId = ++mintedAOTAP;
            AirdropTapOption storage option = options[tokenId];
            option.expiry = _expiry;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

453:     /// @param _tapOracle The new TapOFT Oracle address
         /// @param _tapOracleData The new TapOFT Oracle data
         function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

459:         emit SetTapOracle(_tapOracle, _tapOracleData);
         }
     
         /// @notice Activate or deactivate a payment token

484:         if (_paymentTokenBeneficiary == address(0)) {
                 revert PaymentTokenNotSupported();
             }
             uint256 len = _paymentTokens.length;

517:     /// @param _lock The lock position
         function _isSGLInRescueMode(LockPosition memory _lock) internal view returns (bool) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

285:         if (sglRescueRequest[_sglAssetID] != 0) revert AlreadyActive();
     
             sglRescueRequest[_sglAssetID] = block.timestamp;

382:         uint256 len = singularities.length;
             for (uint256 i; i < len; i++) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

453:     /// =====================
     
         /**
          * @dev Returns the current week given a timestamp
          * @param timestamp The timestamp to use to compute the week
          */
         function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {

460:         return ((timestamp - emissionsStartTime) / EPOCH_DURATION);
         }
     
         /**
          *  @notice returns the available emissions for a given supply

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-20"></a>[NC-20] NatSpec is completely non-existent on functions that should have them

Public and external functions that aren't view or pure should have NatSpec comments

*Instances (9)*:

```solidity
File: contracts/governance/twTAP.sol

512:         if (rewardTokenIndex[_token] != 0) revert Registered();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

304:         tapOracleData = _tapOracleData;

321:     function registerUsersForPhase(uint256 _phase, address[] calldata _users, uint256[] calldata _amounts)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

59: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

94:     {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

633: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

285:         if (sglRescueRequest[_sglAssetID] != 0) revert AlreadyActive();

412:         return bytes4(0);

415: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="NC-21"></a>[NC-21] Incomplete NatSpec: `@param` is missing on actually documented functions

The following functions are missing `@param` NatSpec comments.

*Instances (12)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

49: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

545:      * @dev Use `_isApprovedOrOwner()` internally.
          */
         function _requireClaimPermission(address _to, uint256 _tokenId) internal view {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

353:     /// @notice Set the payment token beneficiary
         /// @param _paymentTokenBeneficiary The new payment token beneficiary
         function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;

400:         uint256 _eligibleAmount = phase1Users[msg.sender];
             if (_eligibleAmount == 0) revert NotEligible();
     
             // Close eligibility

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

100:         option.discount = _discount;
             option.amount = _amount;
     
             _safeMint(_to, tokenId);
             emit Mint(_to, tokenId, option);
         }
     
         /// @notice burns an AOTAP
         /// @param _tokenId tokenId to burn
         function burn(uint256 _tokenId) external {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

474:     /// @notice Set the payment token beneficiary
         /// @param _paymentTokenBeneficiary The new payment token beneficiary
         function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {
             paymentTokenBeneficiary = _paymentTokenBeneficiary;

516:     /// @notice Check if a singularity is in rescue mode
         /// @param _lock The lock position

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

380:     function _computeSGLPoolWeights() internal view returns (uint256) {
             uint256 total;
             uint256 len = singularities.length;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

197:      * @param _guid The unique identifier for the received LayerZero message.
          * @param _message The encoded message.
          * @dev _executor The address of the executor.
          * @dev _extraData Additional data.
          */
         function lzReceive(
             Origin calldata _origin,
             bytes32 _guid,
             bytes calldata _message,
             address _executor, // @dev unused in the default implementation.
             bytes calldata _extraData // @dev unused in the default implementation.
         ) public payable override {
             // Call the internal OApp implementation of lzReceive.
             _executeModule(
                 uint8(ITapToken.Module.TapTokenReceiver),
                 abi.encodeWithSelector(OAppReceiver.lzReceive.selector, _origin, _guid, _message, _executor, _extraData),
                 false
             );
         }
     
         /**
          * @notice Execute a call to a module.
          * @dev Example on how `_data` should be encoded:

448:         }
         }
     
         /// =====================
         /// Internal
         /// =====================
     
         /**

459:     function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {
             return ((timestamp - emissionsStartTime) / EPOCH_DURATION);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

221:         if (_seededAmount == 0) revert NoTokens();
             if (totalRegisteredAmount > _seededAmount) revert NotEnough();
     
             token = _token;
             uint256 availableToken = _token.balanceOf(address(this));
             if (availableToken < _seededAmount) revert BalanceTooLow();
     
             seeded = _seededAmount;
             start = block.timestamp;
     
             if (_initialUnlock > 10_000) revert AmountNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="NC-22"></a>[NC-22] Incomplete NatSpec: `@return` is missing on actually documented functions

The following functions are missing `@return` NatSpec comments.

*Instances (6)*:

```solidity
File: contracts/governance/twTAP.sol

299:         if (_duration < EPOCH_DURATION) revert LockNotAWeek();
             if (_duration > MAX_LOCK_DURATION) revert LockTooLong();
             if (_timestampToWeek(block.timestamp) > currentWeek()) revert AdvanceEpochFirst();
     
             // Transfer TAP to this contract
             {
                 // tapOFT.transferFrom(msg.sender, address(this), _amount);
                 bool isErr = pearlmit.transferFromERC20(msg.sender, address(this), address(tapOFT), _amount);
                 if (isErr) revert NotAuthorized();

518:         uint256 newTokenIndex = rewardTokens.length - 1;
             rewardTokenIndex[_token] = newTokenIndex;
     
             return newTokenIndex;
         }
     
         /**
          * @notice Un/Pauses this contract.
          */
         function setPause(bool _pauseState) external onlyOwner {
             if (_pauseState) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

211:         } else if (cachedEpoch == 2) {
                 aoTAPTokenID = _participatePhase2(_data); // _data = (uint256 role, bytes32[] _merkleProof)
             } else if (cachedEpoch == 3) {
                 aoTAPTokenID = _participatePhase3(_data); // _data = (uint256[] _tokenID)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

100:         option.discount = _discount;
             option.amount = _amount;
     
             _safeMint(_to, tokenId);
             emit Mint(_to, tokenId, option);
         }
     
         /// @notice burns an AOTAP
         /// @param _tokenId tokenId to burn
         function burn(uint256 _tokenId) external {
             if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

233:         if (block.timestamp >= lockExpiry) revert LockExpired();
             if (_timestampToWeek(block.timestamp) > epoch) revert AdvanceEpochFirst();
     
             bool isPositionActive = _isPositionActive(lock);
             if (!isPositionActive) revert OptionExpired();
     
             if (lock.lockDuration < EPOCH_DURATION) revert DurationTooShort();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/oTAP.sol

89:         option.expiry = _expiry;
            option.discount = _discount;
            option.tOLP = _tOLP;
    
            _safeMint(_to, tokenId);
            emit Mint(_to, tokenId, option);
        }
    
        /// @notice burns an OTAP
        /// @param _tokenId tokenId to burn
        function burn(uint256 _tokenId) external {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

### <a name="NC-23"></a>[NC-23] Use a `modifier` instead of a `require/if` statement for a special `msg.sender` actor

If a function is supposed to be access-controlled, a `modifier` should be used instead of a `require/if` statement for more readability.

*Instances (12)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

243:         if (!aoTAP.isApprovedOrOwner(msg.sender, _aoTAPTokenID)) {

423:         if (userParticipation[msg.sender][subPhase]) {

445:             if (PCNFT.ownerOf(_tokenIDs[i]) != msg.sender) revert NotEligible();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

83:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

95:         if (msg.sender != broker) revert OnlyBroker();

110:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

246:         if (!tOLP.isApprovedOrOwner(msg.sender, _tOLPTokenID)) {

380:         if (!oTAP.isApprovedOrOwner(msg.sender, _oTAPTokenID)) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

247:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

84:         if (msg.sender != broker) revert OnlyBroker();

100:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

98:         if (msg.sender != minter) revert OnlyMinter();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-24"></a>[NC-24] Constant state variables defined more than once

Rather than redefining state variable constant, consider using a library to store all constants as this will prevent data redundancy

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

91:     // accurately. Votes in turn are proportional to the amount of TAP locked,

92:     // weighted by a multiplier. This number is at most 107 bits long (see

93:     // definition of `Participation` struct).

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

87:     /// =====-------======

90:     error NotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

85:     error NotInRescueMode();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

58:     /// @dev week is computed using (timestamp - emissionStartTime) / WEEK

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-25"></a>[NC-25] Consider using named mappings

Consider moving to solidity version 0.8.18 or later, and using [named mappings](https://ethereum.stackexchange.com/questions/51629/how-to-name-the-arguments-in-mapping/145555#145555) to make it easier to understand the purpose of each mapping

*Instances (24)*:

```solidity
File: contracts/governance/twTAP.sol

66:     mapping(uint256 => uint256) totalDistPerVote;

77:     mapping(uint256 => Participation) public participants; // tokenId => part.

100:     mapping(IERC20 => uint256) public rewardTokenIndex; // Index 0 is reserved with 0x0 address

104:     mapping(uint256 => mapping(uint256 => uint256)) public claimed;

113:     mapping(uint256 => WeekTotals) public weekTotals;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

56:     mapping(ERC20 => PaymentTokenOracle) public paymentTokens; // Token address => PaymentTokenOracle

59:     mapping(uint256 => mapping(uint256 => uint256)) public aoTAPCalls; // oTAPTokenID => epoch => amountExercised

63:     mapping(address => mapping(uint256 => bool)) public userParticipation; // user address => phase => participated

70:     mapping(address => uint256) public phase1Users;

94:     mapping(address => uint256) public phase4Users;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

34:     mapping(uint256 => AirdropTapOption) public options; // tokenId => Option

35:     mapping(uint256 => string) public tokenURIs; // tokenId => tokenURI

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

62:     mapping(uint256 => Participation) public participants; // tOLPTokenID => Participation

63:     mapping(uint256 => mapping(uint256 => uint256)) public oTAPCalls; // oTAPTokenID => epoch => amountExercised

65:     mapping(uint256 => mapping(uint256 => uint256)) public singularityGauges; // epoch => sglAssetId => availableTAP

67:     mapping(ERC20 => PaymentTokenOracle) public paymentTokens; // Token address => PaymentTokenOracle

71:     mapping(uint256 => TWAMLPool) public twAML; // sglAssetId => twAMLPool

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

55:     mapping(uint256 => LockPosition) public lockPositions; // TokenID => LockPosition

60:     mapping(IERC20 => SingularityPool) public activeSingularities;

61:     mapping(uint256 => IERC20) public sglAssetIDToAddress; // Singularity market YieldBox asset ID => Singularity market address

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

35:     mapping(uint256 => TapOption) public options; // tokenId => Option

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

55:     mapping(uint256 => uint256) public emissionForWeek;

59:     mapping(uint256 => uint256) public mintedInWeek;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

54:     mapping(address => UserData) public users;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="NC-26"></a>[NC-26] Owner can renounce while system is paused

The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (5)*:

```solidity
File: contracts/governance/twTAP.sol

527:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

386:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

500:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

366:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

443:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="NC-27"></a>[NC-27] Adding a `return` statement when the function defines a named return variable, is redundant

*Instances (8)*:

```solidity
File: contracts/governance/twTAP.sol

188:      * @dev index 0 will ALWAYS return 0, as it's used by address(0x0).
          * @dev Should be safe to claim even after position exit.
          * @return claimable amounts mapped by reward token
          */
         function claimable(uint256 _tokenId) public view returns (uint256[] memory) {
             uint256 len = rewardTokens.length;
             uint256[] memory result = new uint256[](len);
     
             Participation memory position = participants[_tokenId];

581:         Participation memory position = participants[_tokenId];
             if (position.expiry > block.timestamp) revert LockNotExpired();
             if (position.tapReleased) {
                 return 0;
             }
     
             releasedAmount = position.tapAmount;
     
             // Remove participation
             if (position.hasVotingPower) {
                 TWAMLPool memory pool = twAML;
                 unchecked {
                     --pool.totalParticipants;
                 }
     
                 // Inverse of the participation. The participation entry tracks
                 // the average magnitude as it was at the time the participant
                 // entered. When going the other way around, this value matches the
                 // one in the pool, but here it does not.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/twAML.sol

26:             // variables such that product = prod1 * 2**256 + prod0
                uint256 prod0; // Least significant 256 bits of the product
                uint256 prod1; // Most significant 256 bits of the product
                assembly {
                    let mm := mulmod(a, b, not(0))
                    prod0 := mul(a, b)
                    prod1 := sub(sub(mm, prod0), lt(mm, prod0))
                }
    
                // Short circuit 256 by 256 division
                // This saves gas when a * b is small, at the cost of making the
                // large case a bit more expensive. Depending on your use case you
                // may want to remove this short circuit and always go through the
                // 512 bit path.
                if (prod1 == 0) {
                    assembly {
                        result := div(prod0, denominator)
                    }
                    return result;
                }
    
                ///////////////////////////////////////////////
                // 512 by 256 division.
                ///////////////////////////////////////////////
    
                // Handle overflow, the result must be < 2**256
                require(prod1 < denominator);
    
                // Make division exact by subtracting the remainder from [prod1 prod0]
                // Compute remainder using mulmod
                // Note mulmod(_, _, 0) == 0
                uint256 remainder;
                assembly {
                    remainder := mulmod(a, b, denominator)
                }
                // Subtract 256 bit number from 512 bit number
                assembly {
                    prod1 := sub(prod1, gt(remainder, prod0))
                    prod0 := sub(prod0, remainder)
                }
    
                // Factor powers of two out of denominator
                // Compute largest power of two divisor of denominator.
                // Always >= 1 unless denominator is zero, then twos is zero.
                uint256 twos = denominator & (~denominator + 1);
                // Divide denominator by power of two
                assembly {
                    denominator := div(denominator, twos)
                }
    
                // Divide [prod1 prod0] by the factors of two
                assembly {
                    prod0 := div(prod0, twos)
                }
                // Shift in bits from prod1 into prod0. For this we need
                // to flip `twos` such that it is 2**256 / twos.
                // If twos is zero, then it becomes one
                assembly {
                    twos := add(div(sub(0, twos), twos), 1)
                }
                prod0 |= prod1 * twos;
    
                // Invert denominator mod 2**256
                // Now that denominator is an odd number, it has an inverse
                // modulo 2**256 such that denominator * inv = 1 mod 2**256.
                // Compute the inverse by starting with a seed that is correct
                // correct for four bits. That is, denominator * inv = 1 mod 2**4
                // If denominator is zero the inverse starts with 2
                uint256 inv = (3 * denominator) ^ 2;
                // Now use Newton-Raphson itteration to improve the precision.
                // Thanks to Hensel's lifting lemma, this also works in modular
                // arithmetic, doubling the correct bits in each step.
                inv *= 2 - denominator * inv; // inverse mod 2**8
                inv *= 2 - denominator * inv; // inverse mod 2**16
                inv *= 2 - denominator * inv; // inverse mod 2**32
                inv *= 2 - denominator * inv; // inverse mod 2**64
                inv *= 2 - denominator * inv; // inverse mod 2**128
                inv *= 2 - denominator * inv; // inverse mod 2**256
                // If denominator is zero, inv is now 128
    
                // Because the division is now exact we can divide by multiplying
                // with the modular inverse of denominator. This will give us the
                // correct result modulo 2**256. Since the precoditions guarantee
                // that the outcome is less than 2**256, this is the final result.
                // We don't need to compute the high bits of the result and prod1
                // is no longer required.
                result = prod0 * inv;
                return result;
            }
        }
    }
    
    abstract contract TWAML is FullMath {
        /// @notice Compute the minimum weight to participate in the twAML voting mechanism
        /// @param _totalWeight The total weight of the twAML system
        /// @param _minWeightFactor The minimum weight factor in BPS
        function computeMinWeight(uint256 _totalWeight, uint256 _minWeightFactor) internal pure returns (uint256) {
            uint256 mul = (_totalWeight * _minWeightFactor);
            return mul >= 1e4 ? mul / 1e4 : _totalWeight;

26:             // variables such that product = prod1 * 2**256 + prod0
                uint256 prod0; // Least significant 256 bits of the product
                uint256 prod1; // Most significant 256 bits of the product
                assembly {
                    let mm := mulmod(a, b, not(0))
                    prod0 := mul(a, b)
                    prod1 := sub(sub(mm, prod0), lt(mm, prod0))
                }
    
                // Short circuit 256 by 256 division
                // This saves gas when a * b is small, at the cost of making the
                // large case a bit more expensive. Depending on your use case you
                // may want to remove this short circuit and always go through the
                // 512 bit path.
                if (prod1 == 0) {
                    assembly {
                        result := div(prod0, denominator)
                    }
                    return result;
                }
    
                ///////////////////////////////////////////////
                // 512 by 256 division.
                ///////////////////////////////////////////////
    
                // Handle overflow, the result must be < 2**256
                require(prod1 < denominator);
    
                // Make division exact by subtracting the remainder from [prod1 prod0]
                // Compute remainder using mulmod
                // Note mulmod(_, _, 0) == 0
                uint256 remainder;
                assembly {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapToken.sol

227:      * @return returnData The return data from the module execution, if any.
          */
         function executeModule(ITapToken.Module _module, bytes memory _data, bool _forwardRevert)
             external
             payable
             returns (bytes memory returnData)
         {
             return _executeModule(uint8(_module), _data, _forwardRevert);
         }
     
         /// ========================
         /// Frequently used modules
         /// ========================
     
         /**
          * @dev Slightly modified version of the OFT send() operation. Includes a `_msgType` parameter.
          * The `_buildMsgAndOptionsByType()` appends the packet type to the message.
          * @dev Executes the send operation.
          * @param _lzSendParam The parameters for the send operation.
          *      - _sendParam: The parameters for the send operation.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

138:      * @param _claimTwTapRewardsMsg Struct of the call.
          *        - tokenId::uint256: The tokenId of the TwTap position to claim rewards from.
          *        - lzSendParams::LZSendParam[]: The LZ send params to pass on the remote chain. (B->A)
          */
         function buildClaimTwTapRewards(ClaimTwTapRewardsMsg memory _claimTwTapRewardsMsg)
             internal
             pure

159:         returns (ClaimTwTapRewardsMsg memory claimTwTapRewardsMsg_)
         {
             return abi.decode(_msg, (ClaimTwTapRewardsMsg));
         }
     }
     

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

92:         } else if (_msgType == MSG_CLAIM_REWARDS) {
                _claimTwpTapRewardsReceiver(_toeComposeMsg);
            } else {
                return false;
            }
    
            return true;
        }
    
        /**
         * @dev Locks TAP for the user in the twTAP contract.
         * @dev The user needs to have approved the TapToken contract to spend the TAP.
         *
         * @param _srcChainSender The address of the sender on the source chain.
         * @param _data The call data containing info about the lock.
         *          - user::address: Address of the user to lock the TAP for.
         *          - duration::uint96: Amount of time to lock for.
         *          - amount::uint256: Amount of TAP to lock.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="NC-28"></a>[NC-28] `require()` / `revert()` statements should have descriptive reason strings

*Instances (2)*:

```solidity
File: contracts/options/twAML.sol

29:             assembly {

64:                 prod0 := sub(prod0, remainder)

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

### <a name="NC-29"></a>[NC-29] Take advantage of Custom Error's return value property

An important feature of Custom Error is that values such as address, tokenID, msg.value can be written inside the () sign, this kind of approach provides a serious advantage in debugging and examining the revert details of dapps such as tenderly.

*Instances (138)*:

```solidity
File: contracts/governance/twTAP.sol

299:         if (_duration < EPOCH_DURATION) revert LockNotAWeek();

300:         if (_duration > MAX_LOCK_DURATION) revert LockTooLong();

301:         if (_timestampToWeek(block.timestamp) > currentWeek()) revert AdvanceEpochFirst();

307:             if (isErr) revert NotAuthorized();

315:         if (magnitude >= pool.cumulative * 4) revert NotValid();

464:         if (lastProcessedWeek != currentWeek()) revert AdvanceWeekFirst();

465:         if (_amount == 0) revert NotValid();

466:         if (_rewardTokenId == 0) revert NotValid(); // @dev rewardTokens[0] is 0x0

512:         if (rewardTokenIndex[_token] != 0) revert Registered();

514:             revert TokenLimitReached();

582:         if (position.expiry > block.timestamp) revert LockNotExpired();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

144:         if (address(tapOracle) == address(0)) revert TapOracleNotSet();

145:         if (address(tapToken) == address(0)) revert TapNotSet();

170:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

178:             revert PaymentTokenNotValid();

183:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

186:         if (tapAmount < 1e18) revert TooLow();

205:         if (cachedEpoch == 0) revert NotStarted();

206:         if (cachedEpoch > LAST_EPOCH) revert Ended();

233:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

241:             revert PaymentTokenNotValid();

244:             revert NotAuthorized();

251:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

254:         if (chosenAmount < 1e18) revert TooLow();

266:             revert TooSoon();

280:         if (!success) revert Failed();

295:         if (address(tapToken) != address(0)) revert NotValid();

310:         if (epoch >= 2) revert NotValid();

325:         if (_users.length != _amounts.length) revert NotValid();

328:             if (epoch >= 1) revert NotValid();

363:             revert TokenBeneficiaryNotSet();

378:         if (epoch <= LAST_EPOCH) revert TooSoon();

401:         if (_eligibleAmount == 0) revert NotEligible();

419:             revert NotEligible();

424:             revert AlreadyParticipated();

445:             if (PCNFT.ownerOf(_tokenIDs[i]) != msg.sender) revert NotEligible();

451:                 revert AlreadyParticipated();

471:         if (_eligibleAmount == 0) revert NotEligible();

492:         if (tapAmount == 0) revert TapAmountNotValid();

499:         if (!success) revert Failed();

504:         if (discountedPaymentAmount == 0) revert PaymentAmountNotValid();

511:             if (isErr) revert Failed();

514:         if (balAfter - balBefore != discountedPaymentAmount) revert Failed();

531:         if (_paymentTokenValuation == 0) revert PaymentTokenValuationNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

38:         if (address(tapToken) == address(0)) revert TapNotSet();

53:         if (!openRedemption) revert RedemptionNotOpen();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

83:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

95:         if (msg.sender != broker) revert OnlyBroker();

110:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

118:         if (broker != address(0)) revert OnlyOnce();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

122:         if (_epochDuration != TapiocaOptionLiquidityProvision(_tOLP).EPOCH_DURATION()) revert NotEqualDurations();

183:             if (!_isPositionActive(tOLPLockPosition)) revert OptionExpired();

192:             revert PaymentTokenNotSupported();

195:             revert OneEpochCooldown();

202:             if (netAmount == 0) revert NoLiquidity();

206:             if (eligibleTapAmount < _tapAmount) revert TooHigh();

210:         if (tapAmount < 1e18) revert TooLow();

233:         if (block.timestamp >= lockExpiry) revert LockExpired();

234:         if (_timestampToWeek(block.timestamp) > epoch) revert AdvanceEpochFirst();

237:         if (!isPositionActive) revert OptionExpired();

239:         if (lock.lockDuration < EPOCH_DURATION) revert DurationTooShort();

247:             revert NotAuthorized();

254:             if (isErr) revert TransferFailed();

261:         if (magnitude > pool.cumulative * 4) revert TooLong();

308:         if (!oTAP.exists(_oTAPTokenID)) revert PositionNotValid();

319:                 revert LockNotExpired();

370:         if (!isPositionActive) revert OptionExpired();

378:             revert PaymentTokenNotSupported();

381:             revert NotAuthorized();

384:             revert OneEpochCooldown();

390:         if (netAmount == 0) revert NoLiquidity();

393:         if (eligibleTapAmount < _tapAmount) revert TooHigh();

396:         if (chosenAmount < 1e18) revert TooLow();

409:         if (_timestampToWeek(block.timestamp) <= epoch) revert TooSoon();

412:         if (singularities.length == 0) revert NoActiveSingularities();

423:         if (!success) revert Failed();

485:             revert PaymentTokenNotSupported();

528:         if (_lock.lockTime == 0) revert PositionNotValid();

529:         if (_isSGLInRescueMode(_lock)) revert SingularityInRescueMode();

552:         if (!success) revert Failed();

563:             if (isErr) revert TransferFailed();

567:             revert TransferFailed();

585:         if (_paymentTokenValuation == 0) revert PaymentTokenValuationNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

192:         if (_lockDuration < EPOCH_DURATION) revert DurationTooShort();

193:         if (_lockDuration > MAX_LOCK_DURATION) revert DurationTooLong();

195:         if (_ybShares == 0) revert SharesNotValid();

198:         if (sgl.rescue) revert SingularityInRescueMode();

201:         if (sglAssetID == 0) revert SingularityNotActive();

209:                 revert TransferFailed();

233:         if (!_exists(_tokenId)) revert PositionExpired();

241:             if (block.timestamp < lockPosition.lockTime + lockPosition.lockDuration) revert LockNotExpired();

244:             revert InvalidSingularity();

247:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

268:             revert NotRegistered();

284:         if (_sglAssetID == 0) revert NotRegistered();

285:         if (sglRescueRequest[_sglAssetID] != 0) revert AlreadyActive();

297:         if (sgl.sglAssetID == 0) revert NotRegistered();

298:         if (sgl.rescue) revert AlreadyActive();

299:         if (sglRescueRequest[sgl.sglAssetID] == 0) revert NotActive();

300:         if (block.timestamp < sglRescueRequest[sgl.sglAssetID] + rescueCooldown) revert RescueCooldownNotReached();

316:         if (assetID == 0) revert AssetIdNotValid();

318:             revert DuplicateAssetId();

321:             revert AlreadyRegistered();

336:         if (sglAssetID == 0) revert NotRegistered();

337:         if (!activeSingularities[singularity].rescue) revert NotInRescueMode();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

84:         if (msg.sender != broker) revert OnlyBroker();

100:         if (!_isApprovedOrOwner(msg.sender, _tokenId)) revert NotAuthorized();

108:         if (broker != address(0)) revert OnlyOnce();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

41:         if (address(twTap) == address(0)) revert twTapNotSet();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

98:         if (msg.sender != minter) revert OnlyMinter();

103:         if (_getChainId() != governanceEid) revert OnlyHostChain();

141:         if (_data.endpoint == address(0)) revert AddressWrong();

152:             if (totalSupply() != INITIAL_SUPPLY) revert SupplyNotValid();

157:         if (_data.tapTokenSenderModule == address(0)) revert NotValid();

158:         if (_data.tapTokenReceiverModule == address(0)) revert NotValid();

353:         if (_amount == 0) revert NotValid();

357:             revert InsufficientEmissions();

385:         if (_getChainId() != governanceEid) revert NotValid();

425:         if (_minter == address(0)) revert NotValid();

435:             revert TwTapAlreadySet();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

85:         if (_duration == 0) revert VestingDurationNotValid();

142:         if (start == 0) revert NotStarted();

144:         if (_claimable == 0) revert NothingToClaim();

162:         if (start > 0) revert Initialized();

163:         if (_user == address(0)) revert AddressNotValid();

164:         if (_amount == 0) revert AmountNotValid();

165:         if (users[_user].amount > 0) revert AlreadyRegistered();

181:         if (start > 0) revert Initialized();

193:             if (_users[i] == address(0)) revert AddressNotValid();

194:             if (_amounts[i] == 0) revert AmountNotValid();

195:             if (users[_users[i]].amount > 0) revert AlreadyRegistered();

210:         if (_cachedTotalAmount > _totalAmount) revert Overflow();

220:         if (start > 0) revert Initialized();

221:         if (_seededAmount == 0) revert NoTokens();

222:         if (totalRegisteredAmount > _seededAmount) revert NotEnough();

226:         if (availableToken < _seededAmount) revert BalanceTooLow();

231:         if (_initialUnlock > 10_000) revert AmountNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

43:         if (module == address(0)) revert ModuleManager__ModuleNotAuthorized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-30"></a>[NC-30] Contract does not follow the Solidity style guide's suggested layout ordering

The [style guide](https://docs.soliditylang.org/en/v0.8.16/style-guide.html#order-of-layout) says that, within a contract, the ordering should be:

1) Type declarations
2) State variables
3) Events
4) Modifiers
5) Functions

However, the contract(s) below do not follow this ordering

*Instances (11)*:

```solidity
File: contracts/governance/twTAP.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.tapOFT
   VariableDeclaration.twAML
   VariableDeclaration.participants
   VariableDeclaration.VIRTUAL_TOTAL_AMOUNT
   VariableDeclaration.MIN_WEIGHT_FACTOR
   VariableDeclaration.dMAX
   VariableDeclaration.dMIN
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.MAX_LOCK_DURATION
   VariableDeclaration.DIST_PRECISION
   VariableDeclaration.rewardTokens
   VariableDeclaration.rewardTokenIndex
   VariableDeclaration.maxRewardTokens
   VariableDeclaration.claimed
   VariableDeclaration.mintedTWTap
   VariableDeclaration.creation
   VariableDeclaration.lastProcessedWeek
   VariableDeclaration.weekTotals
   EventDefinition.LogMaxRewardsLength
   ErrorDefinition.NotAuthorized
   ErrorDefinition.AdvanceWeekFirst
   ErrorDefinition.NotValid
   ErrorDefinition.Registered
   ErrorDefinition.TokenLimitReached
   ErrorDefinition.NotApproved
   ErrorDefinition.Duplicate
   ErrorDefinition.LockNotExpired
   ErrorDefinition.LockNotAWeek
   ErrorDefinition.LockTooLong
   ErrorDefinition.AdvanceEpochFirst
   FunctionDefinition.constructor
   EventDefinition.Participate
   EventDefinition.AMLDivergence
   EventDefinition.ExitPosition
   FunctionDefinition.tokenURI
   FunctionDefinition.getRewardTokens
   FunctionDefinition.currentWeek
   FunctionDefinition.getParticipation
   FunctionDefinition.claimable
   FunctionDefinition.getTypedDataHash
   FunctionDefinition.participate
   FunctionDefinition.claimRewards
   FunctionDefinition.exitPosition
   FunctionDefinition.advanceWeek
   FunctionDefinition.distributeReward
   FunctionDefinition.setVirtualTotalAmount
   FunctionDefinition.setMinWeightFactor
   FunctionDefinition.setMaxRewardTokensLength
   FunctionDefinition.addRewardToken
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._requireClaimPermission
   FunctionDefinition._claimRewards
   FunctionDefinition._releaseTap
   FunctionDefinition._existInArray
   FunctionDefinition._getChainId
   FunctionDefinition.supportsInterface
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.tapOFT
   VariableDeclaration.twAML
   VariableDeclaration.participants
   VariableDeclaration.VIRTUAL_TOTAL_AMOUNT
   VariableDeclaration.MIN_WEIGHT_FACTOR
   VariableDeclaration.dMAX
   VariableDeclaration.dMIN
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.MAX_LOCK_DURATION
   VariableDeclaration.DIST_PRECISION
   VariableDeclaration.rewardTokens
   VariableDeclaration.rewardTokenIndex
   VariableDeclaration.maxRewardTokens
   VariableDeclaration.claimed
   VariableDeclaration.mintedTWTap
   VariableDeclaration.creation
   VariableDeclaration.lastProcessedWeek
   VariableDeclaration.weekTotals
   ErrorDefinition.NotAuthorized
   ErrorDefinition.AdvanceWeekFirst
   ErrorDefinition.NotValid
   ErrorDefinition.Registered
   ErrorDefinition.TokenLimitReached
   ErrorDefinition.NotApproved
   ErrorDefinition.Duplicate
   ErrorDefinition.LockNotExpired
   ErrorDefinition.LockNotAWeek
   ErrorDefinition.LockTooLong
   ErrorDefinition.AdvanceEpochFirst
   EventDefinition.LogMaxRewardsLength
   EventDefinition.Participate
   EventDefinition.AMLDivergence
   EventDefinition.ExitPosition
   FunctionDefinition.constructor
   FunctionDefinition.tokenURI
   FunctionDefinition.getRewardTokens
   FunctionDefinition.currentWeek
   FunctionDefinition.getParticipation
   FunctionDefinition.claimable
   FunctionDefinition.getTypedDataHash
   FunctionDefinition.participate
   FunctionDefinition.claimRewards
   FunctionDefinition.exitPosition
   FunctionDefinition.advanceWeek
   FunctionDefinition.distributeReward
   FunctionDefinition.setVirtualTotalAmount
   FunctionDefinition.setMinWeightFactor
   FunctionDefinition.setMaxRewardTokensLength
   FunctionDefinition.addRewardToken
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._requireClaimPermission
   FunctionDefinition._claimRewards
   FunctionDefinition._releaseTap
   FunctionDefinition._existInArray
   FunctionDefinition._getChainId
   FunctionDefinition.supportsInterface

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.tapOracleData
   VariableDeclaration.tapOracle
   VariableDeclaration.tapToken
   VariableDeclaration.aoTAP
   VariableDeclaration.PCNFT
   VariableDeclaration.epochTAPValuation
   VariableDeclaration.lastEpochUpdate
   VariableDeclaration.epoch
   VariableDeclaration.paymentTokens
   VariableDeclaration.paymentTokenBeneficiary
   VariableDeclaration.aoTAPCalls
   VariableDeclaration.userParticipation
   VariableDeclaration.phase1Users
   VariableDeclaration.PHASE_1_DISCOUNT
   VariableDeclaration.phase2MerkleRoots
   VariableDeclaration.PHASE_2_AMOUNT_PER_USER
   VariableDeclaration.PHASE_2_DISCOUNT_PER_USER
   VariableDeclaration.PHASE_3_AMOUNT_PER_USER
   VariableDeclaration.PHASE_3_DISCOUNT
   VariableDeclaration.phase4Users
   VariableDeclaration.PHASE_4_DISCOUNT
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.LAST_EPOCH
   ErrorDefinition.PaymentTokenNotValid
   ErrorDefinition.OptionExpired
   ErrorDefinition.TooHigh
   ErrorDefinition.TooLow
   ErrorDefinition.NotStarted
   ErrorDefinition.Ended
   ErrorDefinition.NotAuthorized
   ErrorDefinition.TooSoon
   ErrorDefinition.Failed
   ErrorDefinition.NotValid
   ErrorDefinition.TokenBeneficiaryNotSet
   ErrorDefinition.NotEligible
   ErrorDefinition.AlreadyParticipated
   ErrorDefinition.PaymentAmountNotValid
   ErrorDefinition.TapAmountNotValid
   ErrorDefinition.PaymentTokenValuationNotValid
   ErrorDefinition.TapNotSet
   ErrorDefinition.TapOracleNotSet
   FunctionDefinition.constructor
   EventDefinition.Participate
   EventDefinition.ExerciseOption
   EventDefinition.NewEpoch
   EventDefinition.SetPaymentToken
   EventDefinition.SetTapOracle
   EventDefinition.Phase2MerkleRootsUpdated
   ModifierDefinition.tapExists
   FunctionDefinition.getOTCDealDetails
   FunctionDefinition.participate
   FunctionDefinition.exerciseOption
   FunctionDefinition.newEpoch
   FunctionDefinition.aoTAPBrokerClaim
   FunctionDefinition.setTapToken
   FunctionDefinition.setTapOracle
   FunctionDefinition.setPhase2MerkleRoots
   FunctionDefinition.registerUsersForPhase
   FunctionDefinition.setPaymentToken
   FunctionDefinition.setPaymentTokenBeneficiary
   FunctionDefinition.collectPaymentTokens
   FunctionDefinition.daoRecoverTAP
   FunctionDefinition.setPause
   FunctionDefinition._participatePhase1
   FunctionDefinition._participatePhase2
   FunctionDefinition._participatePhase3
   FunctionDefinition._participatePhase4
   FunctionDefinition._processOTCDeal
   FunctionDefinition._getDiscountedPaymentAmount
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.tapOracleData
   VariableDeclaration.tapOracle
   VariableDeclaration.tapToken
   VariableDeclaration.aoTAP
   VariableDeclaration.PCNFT
   VariableDeclaration.epochTAPValuation
   VariableDeclaration.lastEpochUpdate
   VariableDeclaration.epoch
   VariableDeclaration.paymentTokens
   VariableDeclaration.paymentTokenBeneficiary
   VariableDeclaration.aoTAPCalls
   VariableDeclaration.userParticipation
   VariableDeclaration.phase1Users
   VariableDeclaration.PHASE_1_DISCOUNT
   VariableDeclaration.phase2MerkleRoots
   VariableDeclaration.PHASE_2_AMOUNT_PER_USER
   VariableDeclaration.PHASE_2_DISCOUNT_PER_USER
   VariableDeclaration.PHASE_3_AMOUNT_PER_USER
   VariableDeclaration.PHASE_3_DISCOUNT
   VariableDeclaration.phase4Users
   VariableDeclaration.PHASE_4_DISCOUNT
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.LAST_EPOCH
   ErrorDefinition.PaymentTokenNotValid
   ErrorDefinition.OptionExpired
   ErrorDefinition.TooHigh
   ErrorDefinition.TooLow
   ErrorDefinition.NotStarted
   ErrorDefinition.Ended
   ErrorDefinition.NotAuthorized
   ErrorDefinition.TooSoon
   ErrorDefinition.Failed
   ErrorDefinition.NotValid
   ErrorDefinition.TokenBeneficiaryNotSet
   ErrorDefinition.NotEligible
   ErrorDefinition.AlreadyParticipated
   ErrorDefinition.PaymentAmountNotValid
   ErrorDefinition.TapAmountNotValid
   ErrorDefinition.PaymentTokenValuationNotValid
   ErrorDefinition.TapNotSet
   ErrorDefinition.TapOracleNotSet
   EventDefinition.Participate
   EventDefinition.ExerciseOption
   EventDefinition.NewEpoch
   EventDefinition.SetPaymentToken
   EventDefinition.SetTapOracle
   EventDefinition.Phase2MerkleRootsUpdated
   ModifierDefinition.tapExists
   FunctionDefinition.constructor
   FunctionDefinition.getOTCDealDetails
   FunctionDefinition.participate
   FunctionDefinition.exerciseOption
   FunctionDefinition.newEpoch
   FunctionDefinition.aoTAPBrokerClaim
   FunctionDefinition.setTapToken
   FunctionDefinition.setTapOracle
   FunctionDefinition.setPhase2MerkleRoots
   FunctionDefinition.registerUsersForPhase
   FunctionDefinition.setPaymentToken
   FunctionDefinition.setPaymentTokenBeneficiary
   FunctionDefinition.collectPaymentTokens
   FunctionDefinition.daoRecoverTAP
   FunctionDefinition.setPause
   FunctionDefinition._participatePhase1
   FunctionDefinition._participatePhase2
   FunctionDefinition._participatePhase3
   FunctionDefinition._participatePhase4
   FunctionDefinition._processOTCDeal
   FunctionDefinition._getDiscountedPaymentAmount

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.tapToken
   VariableDeclaration.lbp
   VariableDeclaration.openRedemption
   ErrorDefinition.TapNotSet
   ErrorDefinition.RedemptionNotOpen
   FunctionDefinition.constructor
   ModifierDefinition.tapExists
   FunctionDefinition.setTapToken
   FunctionDefinition.setOpenRedemption
   FunctionDefinition.redeem
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.tapToken
   VariableDeclaration.lbp
   VariableDeclaration.openRedemption
   ErrorDefinition.TapNotSet
   ErrorDefinition.RedemptionNotOpen
   ModifierDefinition.tapExists
   FunctionDefinition.constructor
   FunctionDefinition.setTapToken
   FunctionDefinition.setOpenRedemption
   FunctionDefinition.redeem

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

1: 
   Current order:
   VariableDeclaration.mintedAOTAP
   VariableDeclaration.broker
   VariableDeclaration.options
   VariableDeclaration.tokenURIs
   FunctionDefinition.constructor
   EventDefinition.Mint
   EventDefinition.Burn
   ErrorDefinition.NotAuthorized
   ErrorDefinition.OnlyBroker
   ErrorDefinition.OnlyOnce
   FunctionDefinition.tokenURI
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition.attributes
   FunctionDefinition.exists
   FunctionDefinition.setTokenURI
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.brokerClaim
   
   Suggested order:
   VariableDeclaration.mintedAOTAP
   VariableDeclaration.broker
   VariableDeclaration.options
   VariableDeclaration.tokenURIs
   ErrorDefinition.NotAuthorized
   ErrorDefinition.OnlyBroker
   ErrorDefinition.OnlyOnce
   EventDefinition.Mint
   EventDefinition.Burn
   FunctionDefinition.constructor
   FunctionDefinition.tokenURI
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition.attributes
   FunctionDefinition.exists
   FunctionDefinition.setTokenURI
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.brokerClaim

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.tOLP
   VariableDeclaration.tapOracleData
   VariableDeclaration.tapOFT
   VariableDeclaration.oTAP
   VariableDeclaration.tapOracle
   VariableDeclaration.epochTAPValuation
   VariableDeclaration.epoch
   VariableDeclaration.participants
   VariableDeclaration.oTAPCalls
   VariableDeclaration.singularityGauges
   VariableDeclaration.paymentTokens
   VariableDeclaration.paymentTokenBeneficiary
   VariableDeclaration.twAML
   VariableDeclaration.VIRTUAL_TOTAL_AMOUNT
   VariableDeclaration.MIN_WEIGHT_FACTOR
   VariableDeclaration.dMAX
   VariableDeclaration.dMIN
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.emissionsStartTime
   VariableDeclaration.netDepositedForEpoch
   ErrorDefinition.NotEqualDurations
   ErrorDefinition.NotAuthorized
   ErrorDefinition.NoActiveSingularities
   ErrorDefinition.NoLiquidity
   ErrorDefinition.OptionExpired
   ErrorDefinition.PaymentTokenNotSupported
   ErrorDefinition.OneEpochCooldown
   ErrorDefinition.TooHigh
   ErrorDefinition.TooLong
   ErrorDefinition.TooLow
   ErrorDefinition.DurationTooShort
   ErrorDefinition.PositionNotValid
   ErrorDefinition.LockNotExpired
   ErrorDefinition.TooSoon
   ErrorDefinition.Failed
   ErrorDefinition.TransferFailed
   ErrorDefinition.SingularityInRescueMode
   ErrorDefinition.PaymentTokenValuationNotValid
   ErrorDefinition.LockExpired
   ErrorDefinition.AdvanceEpochFirst
   FunctionDefinition.constructor
   EventDefinition.Participate
   EventDefinition.AMLDivergence
   EventDefinition.ExerciseOption
   EventDefinition.NewEpoch
   EventDefinition.ExitPosition
   EventDefinition.SetPaymentToken
   EventDefinition.SetTapOracle
   FunctionDefinition.timestampToWeek
   FunctionDefinition.getCurrentWeek
   FunctionDefinition.getOTCDealDetails
   FunctionDefinition.participate
   FunctionDefinition.exitPosition
   FunctionDefinition.exerciseOption
   FunctionDefinition.newEpoch
   FunctionDefinition.oTAPBrokerClaim
   FunctionDefinition.setVirtualTotalAmount
   FunctionDefinition.setMinWeightFactor
   FunctionDefinition.setTapOracle
   FunctionDefinition.setPaymentToken
   FunctionDefinition.setPaymentTokenBeneficiary
   FunctionDefinition.collectPaymentTokens
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._isSGLInRescueMode
   FunctionDefinition._isPositionActive
   FunctionDefinition._processOTCDeal
   FunctionDefinition._getDiscountedPaymentAmount
   FunctionDefinition._emitToGauges
   FunctionDefinition.onERC721Received
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.tOLP
   VariableDeclaration.tapOracleData
   VariableDeclaration.tapOFT
   VariableDeclaration.oTAP
   VariableDeclaration.tapOracle
   VariableDeclaration.epochTAPValuation
   VariableDeclaration.epoch
   VariableDeclaration.participants
   VariableDeclaration.oTAPCalls
   VariableDeclaration.singularityGauges
   VariableDeclaration.paymentTokens
   VariableDeclaration.paymentTokenBeneficiary
   VariableDeclaration.twAML
   VariableDeclaration.VIRTUAL_TOTAL_AMOUNT
   VariableDeclaration.MIN_WEIGHT_FACTOR
   VariableDeclaration.dMAX
   VariableDeclaration.dMIN
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.emissionsStartTime
   VariableDeclaration.netDepositedForEpoch
   ErrorDefinition.NotEqualDurations
   ErrorDefinition.NotAuthorized
   ErrorDefinition.NoActiveSingularities
   ErrorDefinition.NoLiquidity
   ErrorDefinition.OptionExpired
   ErrorDefinition.PaymentTokenNotSupported
   ErrorDefinition.OneEpochCooldown
   ErrorDefinition.TooHigh
   ErrorDefinition.TooLong
   ErrorDefinition.TooLow
   ErrorDefinition.DurationTooShort
   ErrorDefinition.PositionNotValid
   ErrorDefinition.LockNotExpired
   ErrorDefinition.TooSoon
   ErrorDefinition.Failed
   ErrorDefinition.TransferFailed
   ErrorDefinition.SingularityInRescueMode
   ErrorDefinition.PaymentTokenValuationNotValid
   ErrorDefinition.LockExpired
   ErrorDefinition.AdvanceEpochFirst
   EventDefinition.Participate
   EventDefinition.AMLDivergence
   EventDefinition.ExerciseOption
   EventDefinition.NewEpoch
   EventDefinition.ExitPosition
   EventDefinition.SetPaymentToken
   EventDefinition.SetTapOracle
   FunctionDefinition.constructor
   FunctionDefinition.timestampToWeek
   FunctionDefinition.getCurrentWeek
   FunctionDefinition.getOTCDealDetails
   FunctionDefinition.participate
   FunctionDefinition.exitPosition
   FunctionDefinition.exerciseOption
   FunctionDefinition.newEpoch
   FunctionDefinition.oTAPBrokerClaim
   FunctionDefinition.setVirtualTotalAmount
   FunctionDefinition.setMinWeightFactor
   FunctionDefinition.setTapOracle
   FunctionDefinition.setPaymentToken
   FunctionDefinition.setPaymentTokenBeneficiary
   FunctionDefinition.collectPaymentTokens
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._isSGLInRescueMode
   FunctionDefinition._isPositionActive
   FunctionDefinition._processOTCDeal
   FunctionDefinition._getDiscountedPaymentAmount
   FunctionDefinition._emitToGauges
   FunctionDefinition.onERC721Received

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

1: 
   Current order:
   VariableDeclaration.tokenCounter
   VariableDeclaration.lockPositions
   VariableDeclaration.yieldBox
   VariableDeclaration.activeSingularities
   VariableDeclaration.sglAssetIDToAddress
   VariableDeclaration.singularities
   VariableDeclaration.rescueCooldown
   VariableDeclaration.sglRescueRequest
   VariableDeclaration.totalSingularityPoolWeights
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.MAX_LOCK_DURATION
   ErrorDefinition.NotRegistered
   ErrorDefinition.InvalidSingularity
   ErrorDefinition.DurationTooShort
   ErrorDefinition.DurationTooLong
   ErrorDefinition.SharesNotValid
   ErrorDefinition.SingularityInRescueMode
   ErrorDefinition.SingularityNotActive
   ErrorDefinition.PositionExpired
   ErrorDefinition.LockNotExpired
   ErrorDefinition.AlreadyActive
   ErrorDefinition.AssetIdNotValid
   ErrorDefinition.DuplicateAssetId
   ErrorDefinition.AlreadyRegistered
   ErrorDefinition.NotAuthorized
   ErrorDefinition.NotInRescueMode
   ErrorDefinition.NotActive
   ErrorDefinition.RescueCooldownNotReached
   ErrorDefinition.TransferFailed
   FunctionDefinition.constructor
   EventDefinition.Mint
   EventDefinition.Burn
   EventDefinition.UpdateTotalSingularityPoolWeights
   EventDefinition.SetSGLPoolWeight
   EventDefinition.RequestSglPoolRescue
   EventDefinition.ActivateSGLPoolRescue
   EventDefinition.RegisterSingularity
   EventDefinition.UnregisterSingularity
   ModifierDefinition.updateTotalSGLPoolWeights
   FunctionDefinition.getLock
   FunctionDefinition.getSingularities
   FunctionDefinition.getSingularityPools
   FunctionDefinition.getTotalPoolDeposited
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition._isApprovedOrOwner
   FunctionDefinition.lock
   FunctionDefinition.unlock
   FunctionDefinition.setSGLPoolWeight
   FunctionDefinition.setRescueCooldown
   FunctionDefinition.requestSglPoolRescue
   FunctionDefinition.activateSGLPoolRescue
   FunctionDefinition.registerSingularity
   FunctionDefinition.unregisterSingularity
   FunctionDefinition.setPause
   FunctionDefinition._computeSGLPoolWeights
   FunctionDefinition.supportsInterface
   FunctionDefinition.onERC1155Received
   FunctionDefinition.onERC1155BatchReceived
   
   Suggested order:
   VariableDeclaration.tokenCounter
   VariableDeclaration.lockPositions
   VariableDeclaration.yieldBox
   VariableDeclaration.activeSingularities
   VariableDeclaration.sglAssetIDToAddress
   VariableDeclaration.singularities
   VariableDeclaration.rescueCooldown
   VariableDeclaration.sglRescueRequest
   VariableDeclaration.totalSingularityPoolWeights
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.MAX_LOCK_DURATION
   ErrorDefinition.NotRegistered
   ErrorDefinition.InvalidSingularity
   ErrorDefinition.DurationTooShort
   ErrorDefinition.DurationTooLong
   ErrorDefinition.SharesNotValid
   ErrorDefinition.SingularityInRescueMode
   ErrorDefinition.SingularityNotActive
   ErrorDefinition.PositionExpired
   ErrorDefinition.LockNotExpired
   ErrorDefinition.AlreadyActive
   ErrorDefinition.AssetIdNotValid
   ErrorDefinition.DuplicateAssetId
   ErrorDefinition.AlreadyRegistered
   ErrorDefinition.NotAuthorized
   ErrorDefinition.NotInRescueMode
   ErrorDefinition.NotActive
   ErrorDefinition.RescueCooldownNotReached
   ErrorDefinition.TransferFailed
   EventDefinition.Mint
   EventDefinition.Burn
   EventDefinition.UpdateTotalSingularityPoolWeights
   EventDefinition.SetSGLPoolWeight
   EventDefinition.RequestSglPoolRescue
   EventDefinition.ActivateSGLPoolRescue
   EventDefinition.RegisterSingularity
   EventDefinition.UnregisterSingularity
   ModifierDefinition.updateTotalSGLPoolWeights
   FunctionDefinition.constructor
   FunctionDefinition.getLock
   FunctionDefinition.getSingularities
   FunctionDefinition.getSingularityPools
   FunctionDefinition.getTotalPoolDeposited
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition._isApprovedOrOwner
   FunctionDefinition.lock
   FunctionDefinition.unlock
   FunctionDefinition.setSGLPoolWeight
   FunctionDefinition.setRescueCooldown
   FunctionDefinition.requestSglPoolRescue
   FunctionDefinition.activateSGLPoolRescue
   FunctionDefinition.registerSingularity
   FunctionDefinition.unregisterSingularity
   FunctionDefinition.setPause
   FunctionDefinition._computeSGLPoolWeights
   FunctionDefinition.supportsInterface
   FunctionDefinition.onERC1155Received
   FunctionDefinition.onERC1155BatchReceived

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

1: 
   Current order:
   VariableDeclaration.mintedOTAP
   VariableDeclaration.broker
   VariableDeclaration.options
   FunctionDefinition.constructor
   EventDefinition.Mint
   EventDefinition.Burn
   ErrorDefinition.NotAuthorized
   ErrorDefinition.OnlyBroker
   ErrorDefinition.OnlyOnce
   FunctionDefinition.tokenURI
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition.attributes
   FunctionDefinition.exists
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.brokerClaim
   
   Suggested order:
   VariableDeclaration.mintedOTAP
   VariableDeclaration.broker
   VariableDeclaration.options
   ErrorDefinition.NotAuthorized
   ErrorDefinition.OnlyBroker
   ErrorDefinition.OnlyOnce
   EventDefinition.Mint
   EventDefinition.Burn
   FunctionDefinition.constructor
   FunctionDefinition.tokenURI
   FunctionDefinition.isApprovedOrOwner
   FunctionDefinition.attributes
   FunctionDefinition.exists
   FunctionDefinition.mint
   FunctionDefinition.burn
   FunctionDefinition.brokerClaim

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/BaseTapToken.sol

1: 
   Current order:
   VariableDeclaration.PT_LOCK_TWTAP
   VariableDeclaration.PT_UNLOCK_TWTAP
   VariableDeclaration.PT_CLAIM_REWARDS
   VariableDeclaration.twTap
   FunctionDefinition.constructor
   ErrorDefinition.twTapNotSet
   ModifierDefinition.twTapExists
   FunctionDefinition.setTwTAP
   
   Suggested order:
   VariableDeclaration.PT_LOCK_TWTAP
   VariableDeclaration.PT_UNLOCK_TWTAP
   VariableDeclaration.PT_CLAIM_REWARDS
   VariableDeclaration.twTap
   ErrorDefinition.twTapNotSet
   ModifierDefinition.twTapExists
   FunctionDefinition.constructor
   FunctionDefinition.setTwTAP

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/BaseTapToken.sol)

```solidity
File: contracts/tokens/TapToken.sol

1: 
   Current order:
   VariableDeclaration.INITIAL_SUPPLY
   VariableDeclaration.dso_supply
   VariableDeclaration.decay_rate
   VariableDeclaration.DECAY_RATE_DECIMAL
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.emissionsStartTime
   VariableDeclaration.emissionForWeek
   VariableDeclaration.mintedInWeek
   VariableDeclaration.minter
   VariableDeclaration.governanceEid
   EventDefinition.MinterUpdated
   EventDefinition.Emitted
   EventDefinition.Minted
   EventDefinition.Burned
   EventDefinition.BoostedTAP
   ErrorDefinition.OnlyHostChain
   ErrorDefinition.NotValid
   ErrorDefinition.AddressWrong
   ErrorDefinition.SupplyNotValid
   ErrorDefinition.AllowanceNotValid
   ErrorDefinition.OnlyMinter
   ErrorDefinition.TwTapAlreadySet
   ErrorDefinition.InsufficientEmissions
   ModifierDefinition.onlyMinter
   ModifierDefinition.onlyHostChain
   FunctionDefinition.constructor
   FunctionDefinition.transferFrom
   FunctionDefinition.fallback
   FunctionDefinition.receive
   FunctionDefinition.lzReceive
   FunctionDefinition.executeModule
   FunctionDefinition.sendPacket
   FunctionDefinition.decimals
   FunctionDefinition.getCurrentWeek
   FunctionDefinition.getCurrentWeekEmission
   FunctionDefinition.timestampToWeek
   FunctionDefinition.getTypedDataHash
   FunctionDefinition.extractTAP
   FunctionDefinition.removeTAP
   FunctionDefinition.emitForWeek
   FunctionDefinition.setMinter
   FunctionDefinition.setTwTAP
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._computeEmission
   FunctionDefinition._getChainId
   
   Suggested order:
   VariableDeclaration.INITIAL_SUPPLY
   VariableDeclaration.dso_supply
   VariableDeclaration.decay_rate
   VariableDeclaration.DECAY_RATE_DECIMAL
   VariableDeclaration.EPOCH_DURATION
   VariableDeclaration.emissionsStartTime
   VariableDeclaration.emissionForWeek
   VariableDeclaration.mintedInWeek
   VariableDeclaration.minter
   VariableDeclaration.governanceEid
   ErrorDefinition.OnlyHostChain
   ErrorDefinition.NotValid
   ErrorDefinition.AddressWrong
   ErrorDefinition.SupplyNotValid
   ErrorDefinition.AllowanceNotValid
   ErrorDefinition.OnlyMinter
   ErrorDefinition.TwTapAlreadySet
   ErrorDefinition.InsufficientEmissions
   EventDefinition.MinterUpdated
   EventDefinition.Emitted
   EventDefinition.Minted
   EventDefinition.Burned
   EventDefinition.BoostedTAP
   ModifierDefinition.onlyMinter
   ModifierDefinition.onlyHostChain
   FunctionDefinition.constructor
   FunctionDefinition.transferFrom
   FunctionDefinition.fallback
   FunctionDefinition.receive
   FunctionDefinition.lzReceive
   FunctionDefinition.executeModule
   FunctionDefinition.sendPacket
   FunctionDefinition.decimals
   FunctionDefinition.getCurrentWeek
   FunctionDefinition.getCurrentWeekEmission
   FunctionDefinition.timestampToWeek
   FunctionDefinition.getTypedDataHash
   FunctionDefinition.extractTAP
   FunctionDefinition.removeTAP
   FunctionDefinition.emitForWeek
   FunctionDefinition.setMinter
   FunctionDefinition.setTwTAP
   FunctionDefinition.setPause
   FunctionDefinition._timestampToWeek
   FunctionDefinition._computeEmission
   FunctionDefinition._getChainId

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

1: 
   Current order:
   UsingForDirective.OFTMsgCodec
   UsingForDirective.OFTMsgCodec
   FunctionDefinition.constructor
   EventDefinition.LockTwTapReceived
   EventDefinition.UnlockTwTapReceived
   EventDefinition.ClaimRewardReceived
   ErrorDefinition.InvalidSendParamLength
   FunctionDefinition._lzReceive
   FunctionDefinition._toeComposeReceiver
   FunctionDefinition._lockTwTapPositionReceiver
   FunctionDefinition._unlockTwTapPositionReceiver
   FunctionDefinition._claimTwpTapRewardsReceiver
   
   Suggested order:
   UsingForDirective.OFTMsgCodec
   UsingForDirective.OFTMsgCodec
   ErrorDefinition.InvalidSendParamLength
   EventDefinition.LockTwTapReceived
   EventDefinition.UnlockTwTapReceived
   EventDefinition.ClaimRewardReceived
   FunctionDefinition.constructor
   FunctionDefinition._lzReceive
   FunctionDefinition._toeComposeReceiver
   FunctionDefinition._lockTwTapPositionReceiver
   FunctionDefinition._unlockTwTapPositionReceiver
   FunctionDefinition._claimTwpTapRewardsReceiver

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/Vesting.sol

1: 
   Current order:
   UsingForDirective.IERC20
   VariableDeclaration.token
   VariableDeclaration.start
   VariableDeclaration.cliff
   VariableDeclaration.duration
   VariableDeclaration.seeded
   VariableDeclaration.totalRegisteredAmount
   VariableDeclaration.__initialUnlockTimeOffset
   StructDefinition.UserData
   VariableDeclaration.users
   VariableDeclaration.__totalClaimed
   ErrorDefinition.NotStarted
   ErrorDefinition.NothingToClaim
   ErrorDefinition.Initialized
   ErrorDefinition.AddressNotValid
   ErrorDefinition.AmountNotValid
   ErrorDefinition.AlreadyRegistered
   ErrorDefinition.NoTokens
   ErrorDefinition.NotEnough
   ErrorDefinition.BalanceTooLow
   ErrorDefinition.VestingDurationNotValid
   ErrorDefinition.Overflow
   EventDefinition.UserRegistered
   EventDefinition.Claimed
   FunctionDefinition.constructor
   FunctionDefinition.claimable
   FunctionDefinition.claimable
   FunctionDefinition.vested
   FunctionDefinition.vested
   FunctionDefinition.totalClaimed
   FunctionDefinition.computeTimeFromAmount
   FunctionDefinition.claim
   FunctionDefinition.registerUser
   FunctionDefinition.registerUsers
   FunctionDefinition.init
   FunctionDefinition._computeTimeFromAmount
   FunctionDefinition._vested
   
   Suggested order:
   UsingForDirective.IERC20
   VariableDeclaration.token
   VariableDeclaration.start
   VariableDeclaration.cliff
   VariableDeclaration.duration
   VariableDeclaration.seeded
   VariableDeclaration.totalRegisteredAmount
   VariableDeclaration.__initialUnlockTimeOffset
   VariableDeclaration.users
   VariableDeclaration.__totalClaimed
   StructDefinition.UserData
   ErrorDefinition.NotStarted
   ErrorDefinition.NothingToClaim
   ErrorDefinition.Initialized
   ErrorDefinition.AddressNotValid
   ErrorDefinition.AmountNotValid
   ErrorDefinition.AlreadyRegistered
   ErrorDefinition.NoTokens
   ErrorDefinition.NotEnough
   ErrorDefinition.BalanceTooLow
   ErrorDefinition.VestingDurationNotValid
   ErrorDefinition.Overflow
   EventDefinition.UserRegistered
   EventDefinition.Claimed
   FunctionDefinition.constructor
   FunctionDefinition.claimable
   FunctionDefinition.claimable
   FunctionDefinition.vested
   FunctionDefinition.vested
   FunctionDefinition.totalClaimed
   FunctionDefinition.computeTimeFromAmount
   FunctionDefinition.claim
   FunctionDefinition.registerUser
   FunctionDefinition.registerUsers
   FunctionDefinition.init
   FunctionDefinition._computeTimeFromAmount
   FunctionDefinition._vested

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="NC-31"></a>[NC-31] TODO Left in the code

TODOs may signal that a feature is missing or not ready for audit, consider resolving the issue and removing the TODO comment

*Instances (2)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

17: import {TWAML, FullMath} from "tap-token/options/twAML.sol"; // TODO Naming

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/oTAP.sol

10: import {ERC721Permit} from "tapioca-periph/utils/ERC721Permit.sol"; // TODO audit

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

### <a name="NC-32"></a>[NC-32] Use Underscores for Number Literals (add an underscore every 3 digits)

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

82:     uint256 MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

76:     uint256 public MIN_WEIGHT_FACTOR = 1000; // In BPS, default 10%

79:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

68:     uint256 public immutable EPOCH_DURATION; // 7 days = 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

43:     uint256 constant decay_rate = 8800000000000000; // 0.88%

47:     uint256 public constant EPOCH_DURATION = 1 weeks; // 604800

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/module/ModuleManager.sol

75:         if (_returnData.length > 1000) return "Module: reason too long";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/module/ModuleManager.sol)

### <a name="NC-33"></a>[NC-33] Internal and private variables and functions names should begin with an underscore

According to the Solidity Style Guide, Non-`external` variable and function names should begin with an [underscore](https://docs.soliditylang.org/en/latest/style-guide.html#underscore-prefix-for-non-external-functions-and-variables)

*Instances (18)*:

```solidity
File: contracts/governance/twTAP.sol

89:     // "tokens" at the most commonly used 1e18 precision -- then we can use the

90:     // other 128 bits to store the tokens allotted to a single vote more

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

41:     /// @notice Sets the TAP token address

42:     /// @param _tapToken The TAP token address

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

85:     /// @notice Total amount of participation per epoch

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/twAML.sol

26:             // variables such that product = prod1 * 2**256 + prod0

131:     function computeTarget(uint256 _dMin, uint256 _dMax, uint256 _magnitude, uint256 _cumulative)

140:         target = target > _dMax ? _dMax : target < _dMin ? _dMin : target;

144:     // babylonian method (https://en.wikipedia.org/wiki/Methods_of_computing_square_roots#Babylonian_method)

158: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

57:      * @return lockTwTapPositionMsg_ The data of the lock.

77:         uint256 amount = BytesLib.toUint256(BytesLib.slice(_msg, durationOffset_, _msg.length - durationOffset_), 0);

97:      * @return unlockTwTapPositionMsg_ The needed data.

113:         unlockTwTapPositionMsg_ = UnlockTwTapPositionMsg(user_, tokenId_);

133:         return abi.decode(_msg, (RemoteTransferMsg));

140:      *        - lzSendParams::LZSendParam[]: The LZ send params to pass on the remote chain. (B->A)

154:      *        - lzSendParams::LZSendParam[]: The LZ send params to pass on the remote chain. (B->A)

164: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

### <a name="NC-34"></a>[NC-34] Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

*Instances (22)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

143:     modifier tapExists() {

153:     /// @notice Returns the details of an OTC deal for a given oTAP token ID and a payment token.

154:     ///         The oracle uses the last peeked value, and not the latest one, so the payment amount may be different.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

142:     event NewEpoch(uint256 indexed epoch, uint256 extractedTAP, uint256 epochTAPValuation);

144:     event SetPaymentToken(ERC20 indexed paymentToken, ITapiocaOracle oracle, bytes oracleData);

151:     function timestampToWeek(uint256 timestamp) external view returns (uint256) {

153:             timestamp = block.timestamp;

155:         if (timestamp < emissionsStartTime) return 0;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

110:     event UnregisterSingularity(address sgl, uint256 assetID);

113:     //    MODIFIERS

115:     modifier updateTotalSGLPoolWeights() {

118:         emit UpdateTotalSingularityPoolWeights(totalSingularityPoolWeights);

124:     /// @notice Returns the lock position of a given tOLP NFT and if it's active

126:     function getLock(uint256 _tokenId) external view returns (LockPosition memory) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

84:     // *ERRORS*

87:     error AddressWrong();

90:     error OnlyMinter();

96:     // ===========

98:         if (msg.sender != minter) revert OnlyMinter();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

61:     error InvalidSendParamLength(uint256 expectedLength, uint256 actualLength);

65:     // ********************* //

70:     function _lzReceive(

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="NC-35"></a>[NC-35] Constants should be defined rather than using magic numbers

*Instances (2)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

536:         paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

593:             paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

### <a name="NC-36"></a>[NC-36] `public` functions not called by the contract should be declared `external` instead

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

284:     //    WRITE

445:             // TODO: Prove that math is safe

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

279:             (MessagingReceipt, OFTReceipt)

336:                 _permitData.deadline

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/extensions/TapTokenHelper.sol

56:     {

64:      * @dev The amount field is trivial in this message as it'll be overwritten by the receiver contract.

84:             return true;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/extensions/TapTokenHelper.sol)

## Low Issues

| |Issue|Instances|
|-|:-|:-:|
| [L-1](#L-1) | `approve()`/`safeApprove()` may revert if the current approval is not zero | 1 |
| [L-2](#L-2) | Use a 2-step ownership transfer pattern | 7 |
| [L-3](#L-3) | Some tokens may revert when zero value transfers are made | 7 |
| [L-4](#L-4) | Missing checks for `address(0)` when assigning values to address state variables | 4 |
| [L-5](#L-5) | `decimals()` is not a part of the ERC-20 standard | 4 |
| [L-6](#L-6) | `decimals()` should be of type `uint8` | 2 |
| [L-7](#L-7) | Deprecated approve() function | 1 |
| [L-8](#L-8) | Division by zero not prevented | 11 |
| [L-9](#L-9) | Empty `receive()/payable fallback()` function does not authenticate requests | 1 |
| [L-10](#L-10) | External calls in an un-bounded `for-`loop may result in a DOS | 1 |
| [L-11](#L-11) | Initializers could be front-run | 1 |
| [L-12](#L-12) | Signature use at deadlines should be allowed | 4 |
| [L-13](#L-13) | Prevent accidentally burning tokens | 13 |
| [L-14](#L-14) | NFT ownership doesn't support hard forks | 3 |
| [L-15](#L-15) | Owner can renounce while system is paused | 5 |
| [L-16](#L-16) | Possible rounding issue | 5 |
| [L-17](#L-17) | Loss of precision | 12 |
| [L-18](#L-18) | Solidity version 0.8.20+ may not work on other chains due to `PUSH0` | 14 |
| [L-19](#L-19) | Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership` | 15 |
| [L-20](#L-20) | Unsafe ERC20 operation(s) | 8 |
| [L-21](#L-21) | Upgradeable contract not initialized | 4 |

### <a name="L-1"></a>[L-1] `approve()`/`safeApprove()` may revert if the current approval is not zero

- Some tokens (like the *very popular* USDT) do not work when changing the allowance from an existing non-zero allowance value (it will revert if the current approval is not zero to protect against front-running changes of approvals). These tokens must first be approved for zero and then the actual allowance can be approved.
- Furthermore, OZ's implementation of safeApprove would throw an error if an approve is attempted from a non-zero value (`"SafeERC20: approve from non-zero to non-zero allowance"`)

Set the allowance to zero immediately before each of the existing allowance calls

*Instances (1)*:

```solidity
File: contracts/tokens/TapTokenReceiver.sol

121:         pearlmit.approve(

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="L-2"></a>[L-2] Use a 2-step ownership transfer pattern

Recommend considering implementing a two step process where the owner or admin nominates an account and the nominated account needs to call an `acceptOwnership()` function for the transfer of ownership to fully succeed. This ensures the nominated EOA account is a valid and active account. Lack of two-step procedure for critical operations leaves them error-prone. Consider adding two step procedure on the critical functions.

*Instances (7)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

24: contract ERC721NftLoader is ERC721, Ownable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

69: contract TwTAP is TWAML, ERC721, ERC721Permit, Ownable, PearlmitHandler, ERC721NftLoader, ReentrancyGuard, Pausable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

43: contract AirdropBroker is Pausable, Ownable, PearlmitHandler, FullMath, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

21: contract LTap is Ownable, ERC20Permit {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

30: contract AOTAP is Ownable, PearlmitHandler, ERC721, ERC721Permit, BaseBoringBatchable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

50: contract TapiocaOptionBroker is Pausable, Ownable, PearlmitHandler, IERC721Receiver, TWAML, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/tokens/Vesting.sol

19: contract Vesting is Ownable, ReentrancyGuard {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-3"></a>[L-3] Some tokens may revert when zero value transfers are made

Example: <https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers>.

In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.

*Instances (7)*:

```solidity
File: contracts/governance/twTAP.sol

495:      * @param _minWeightFactor The new minimum weight factor.

580:     function _releaseTap(uint256 _tokenId, address _to) internal returns (uint256 releasedAmount) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

384:      * @notice Un/Pauses this contract.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

59: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

512:     function _timestampToWeek(uint256 timestamp) internal view returns (uint256) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

194:             TapTokenSender(rewardToken_).sendPacket{

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/Vesting.sol

163:         if (_user == address(0)) revert AddressNotValid();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-4"></a>[L-4] Missing checks for `address(0)` when assigning values to address state variables

*Instances (4)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

138:     event NewEpoch(uint256 indexed epoch, uint256 epochTAPValuation);

368:             for (uint256 i; i < len; ++i) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

136:         uint256 indexed epoch, uint256 indexed sglAssetID, uint256 totalDeposited, uint256 tokenId, uint256 discount

487:         uint256 len = _paymentTokens.length;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

### <a name="L-5"></a>[L-5] `decimals()` is not a part of the ERC-20 standard

The `decimals()` function is not a part of the [ERC-20 standard](https://eips.ethereum.org/EIPS/eip-20), and was added later as an [optional extension](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/IERC20Metadata.sol). As such, some valid ERC20 tokens do not support this interface, so it is unsafe to blindly cast all tokens to this interface, and then call this function.

*Instances (4)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

193:             otcAmountInUSD, paymentTokenValuation, aoTapOption.discount, _paymentToken.decimals()

503:             _getDiscountedPaymentAmount(otcAmountInUSD, paymentTokenValuation, discount, _paymentToken.decimals());

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

217:             otcAmountInUSD, paymentTokenValuation, oTAPPosition.discount, _paymentToken.decimals()

556:             _getDiscountedPaymentAmount(otcAmountInUSD, paymentTokenValuation, discount, _paymentToken.decimals());

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

### <a name="L-6"></a>[L-6] `decimals()` should be of type `uint8`

*Instances (2)*:

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

529:         uint256 _paymentTokenDecimals

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

583:         uint256 _paymentTokenDecimals

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

### <a name="L-7"></a>[L-7] Deprecated approve() function

Due to the inheritance of ERC20's approve function, there's a vulnerability to the ERC20 approve and double spend front running attack. Briefly, an authorized spender could spend both allowances by front running an allowance-changing transaction. Consider implementing OpenZeppelin's `.safeApprove()` function to help mitigate this.

*Instances (1)*:

```solidity
File: contracts/tokens/TapTokenReceiver.sol

131:      * @dev Unlocks TAP for the user in the twTAP contract.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="L-8"></a>[L-8] Division by zero not prevented

The divisions below take an input parameter which does not have any zero-value checks, which may lead to the functions reverting when zero is passed.

*Instances (11)*:

```solidity
File: contracts/governance/twTAP.sol

323:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

476:         totals.totalDistPerVote[_rewardTokenId] += (_amount * DIST_PRECISION) / uint256(totals.netActiveVotes);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

533:         uint256 rawPaymentAmount = _otcAmountInUSD / _paymentTokenValuation;

536:         paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

269:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

590:         paymentAmount = discountedOTCAmountInUSD / _paymentTokenValuation;

593:             paymentAmount = paymentAmount / (10 ** (18 - _paymentTokenDecimals));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/twAML.sol

139:         uint256 target = (_magnitude * _dMax) / _cumulative;

151:                 x = (y / x + x) / 2;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/Vesting.sol

250:         return _start - (_start - ((_amount * _duration) / _totalAmount));

274:         return (_totalAmount * (block.timestamp - _start)) / _duration; // Partially vested

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-9"></a>[L-9] Empty `receive()/payable fallback()` function does not authenticate requests

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.

*Instances (1)*:

```solidity
File: contracts/tokens/TapToken.sol

197:      * @param _guid The unique identifier for the received LayerZero message.

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="L-10"></a>[L-10] External calls in an un-bounded `for-`loop may result in a DOS

Consider limiting the number of iterations in for-loops that make external calls

*Instances (1)*:

```solidity
File: contracts/governance/twTAP.sol

580:     function _releaseTap(uint256 _tokenId, address _to) internal returns (uint256 releasedAmount) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

### <a name="L-11"></a>[L-11] Initializers could be front-run

Initializers could be front-run, allowing an attacker to either set their own values, take ownership of the contract, and in the best case forcing a re-deployment

*Instances (1)*:

```solidity
File: contracts/tokens/Vesting.sol

219:     function init(IERC20 _token, uint256 _seededAmount, uint256 _initialUnlock) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-12"></a>[L-12] Signature use at deadlines should be allowed

According to [EIP-2612](https://github.com/ethereum/EIPs/blob/71dc97318013bf2ac572ab63fab530ac9ef419ca/EIPS/eip-2612.md?plain=1#L58), signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.

*Instances (4)*:

```solidity
File: contracts/governance/twTAP.sol

180:         if (participant.expiry <= block.timestamp) {

582:         if (position.expiry > block.timestamp) revert LockNotExpired();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

170:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

233:         if (aoTapOption.expiry < block.timestamp) revert OptionExpired();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

### <a name="L-13"></a>[L-13] Prevent accidentally burning tokens

Minting and burning tokens to address(0) prevention

*Instances (13)*:

```solidity
File: contracts/option-airdrop/LTap.sol

52:     function redeem() external tapExists {

59: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

122: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

264:     /// @param singularity Singularity market address

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

112: 

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

157:         if (_data.tapTokenSenderModule == address(0)) revert NotValid();

158:         if (_data.tapTokenReceiverModule == address(0)) revert NotValid();

160:         _setModule(uint8(ITapToken.Module.TapTokenSender), _data.tapTokenSenderModule);

161:         _setModule(uint8(ITapToken.Module.TapTokenReceiver), _data.tapTokenReceiverModule);

378:      * @notice Emit the TAP for the current week. Follow the emission function.

384:     function emitForWeek() external onlyMinter returns (uint256) {

424:     function setMinter(address _minter) external onlyOwner {

446:         } else {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="L-14"></a>[L-14] NFT ownership doesn't support hard forks

To ensure clarity regarding the ownership of the NFT on a specific chain, it is recommended to add `require(block.chainid == 1, "Invalid Chain")` or the desired chain ID in the functions below.

Alternatively, consider including the chain ID in the URI itself. By doing so, any confusion regarding the chain responsible for owning the NFT will be eliminated.

*Instances (3)*:

```solidity
File: contracts/governance/twTAP.sol

177:     /// @notice Return the participation of a token. Returns 0 votes for expired tokens.
         function getParticipation(uint256 _tokenId) external view returns (Participation memory participant) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

69:     function attributes(uint256 _tokenId) external view returns (address, AirdropTapOption memory) {
            return (ownerOf(_tokenId), options[_tokenId]);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/oTAP.sol

65:     function attributes(uint256 _tokenId) external view returns (address, TapOption memory) {
            return (ownerOf(_tokenId), options[_tokenId]);
        }
    
        /// @notice Check if a token exists
        function exists(uint256 _tokenId) external view returns (bool) {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

### <a name="L-15"></a>[L-15] Owner can renounce while system is paused

The contract owner or single user with a role is not prevented from renouncing the role/ownership while the contract is paused, which would cause any user assets stored in the protocol, to be locked indefinitely.

*Instances (5)*:

```solidity
File: contracts/governance/twTAP.sol

527:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

386:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

500:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

366:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

443:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

### <a name="L-16"></a>[L-16] Possible rounding issue

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator. Also, there is indication of multiplication and division without the use of parenthesis which could result in issues.

*Instances (5)*:

```solidity
File: contracts/governance/twTAP.sol

323:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

476:         totals.totalDistPerVote[_rewardTokenId] += (_amount * DIST_PRECISION) / uint256(totals.netActiveVotes);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

269:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/twAML.sol

124:         return mul >= 1e4 ? mul / 1e4 : _totalWeight;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/Vesting.sol

250:         return _start - (_start - ((_amount * _duration) / _totalAmount));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-17"></a>[L-17] Loss of precision

Division by large numbers may result in the result being zero, due to solidity not supporting fractions. Consider requiring a minimum amount for the numerator to ensure that it is always larger than the denominator

*Instances (12)*:

```solidity
File: contracts/governance/twTAP.sol

174:         return (block.timestamp - creation) / EPOCH_DURATION;

260:             result[i] = ((votes * net) / DIST_PRECISION) - claimed[_tokenId][i];

323:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

356:         uint256 w1 = (expiry - creation) / EPOCH_DURATION;

476:         totals.totalDistPerVote[_rewardTokenId] += (_amount * DIST_PRECISION) / uint256(totals.netActiveVotes);

541:         return ((timestamp - creation) / EPOCH_DURATION);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

269:             pool.averageMagnitude = (pool.averageMagnitude + magnitude) / pool.totalParticipants; // compute new average magnitude

513:         return ((timestamp - emissionsStartTime) / EPOCH_DURATION);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/twAML.sol

124:         return mul >= 1e4 ? mul / 1e4 : _totalWeight;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/twAML.sol)

```solidity
File: contracts/tokens/TapToken.sol

460:         return ((timestamp - emissionsStartTime) / EPOCH_DURATION);

467:         result = (dso_supply * decay_rate) / DECAY_RATE_DECIMAL;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

250:         return _start - (_start - ((_amount * _duration) / _totalAmount));

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-18"></a>[L-18] Solidity version 0.8.20+ may not work on other chains due to `PUSH0`

The compiler for Solidity 0.8.20 switches the default target EVM version to [Shanghai](https://blog.soliditylang.org/2023/05/10/solidity-0.8.20-release-announcement/#important-note), which includes the new `PUSH0` op code. This op code may not yet be implemented on all L2s, so deployment on these chains will fail. To work around this issue, use an earlier [EVM](https://docs.soliditylang.org/en/v0.8.20/using-the-compiler.html?ref=zaryabs.com#setting-the-evm-version-to-target) [version](https://book.getfoundry.sh/reference/config/solidity-compiler#evm_version). While the project itself may or may not compile with 0.8.20, other projects with which it integrates, or which extend this project may, and those projects will have problems deploying these contracts/libraries.

*Instances (14)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/options/oTAP.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/oTAP.sol)

```solidity
File: contracts/tokens/TapToken.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenCodec.sol

3: pragma solidity ^0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenCodec.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

```solidity
File: contracts/tokens/TapTokenSender.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenSender.sol)

```solidity
File: contracts/tokens/Vesting.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

```solidity
File: contracts/tokens/extensions/TapTokenHelper.sol

2: pragma solidity 0.8.22;

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/extensions/TapTokenHelper.sol)

### <a name="L-19"></a>[L-19] Use `Ownable2Step.transferOwnership` instead of `Ownable.transferOwnership`

Use [Ownable2Step.transferOwnership](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol) which is safer. Use it as it is more secure due to 2-stage ownership transfer.

**Recommended Mitigation Steps**

Use <a href="https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol">Ownable2Step.sol</a>
  
  ```solidity
      function acceptOwnership() external {
          address sender = _msgSender();
          require(pendingOwner() == sender, "Ownable2Step: caller is not the new owner");
          _transferOwnership(sender);
      }
```

*Instances (15)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

28:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

10: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

12: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

128:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

7: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

42:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

11: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

129:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

13: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

97:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

139:         _transferOwnership(_data.owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

6: import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

90:         _transferOwnership(_owner);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="L-20"></a>[L-20] Unsafe ERC20 operation(s)

*Instances (8)*:

```solidity
File: contracts/governance/twTAP.sol

618:         tapOFT.transfer(_to, releasedAmount);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

380:         tapToken.transfer(msg.sender, tapToken.balanceOf(address(this)));

516:         tapToken.transfer(msg.sender, tapAmount);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

356:         tOLP.transferFrom(address(this), otapOwner, oTAPPosition.tOLP);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

253:         yieldBox.transfer(address(this), _to, lockPosition.sglAssetID, lockPosition.ybShares);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

172:         return BaseTapiocaOmnichainEngine.transferFrom(from, to, value);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/TapTokenReceiver.sol

121:         pearlmit.approve(

186:                 IERC20(rewardToken_).transfer(sendTo_, dust);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="L-21"></a>[L-21] Upgradeable contract not initialized

Upgradeable contracts are initialized via an initializer function rather than by a constructor. Leaving such a contract uninitialized may lead to it being taken over by a malicious user

*Instances (4)*:

```solidity
File: contracts/tokens/Vesting.sol

63:     error Initialized();

162:         if (start > 0) revert Initialized();

181:         if (start > 0) revert Initialized();

220:         if (start > 0) revert Initialized();

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

## Medium Issues

| |Issue|Instances|
|-|:-|:-:|
| [M-1](#M-1) | Centralization Risk for trusted owners | 41 |
| [M-2](#M-2) | Using `transferFrom` on ERC721 tokens | 2 |
| [M-3](#M-3) | Direct `supportsInterface()` calls may cause caller to revert | 2 |
| [M-4](#M-4) | Return values of `transfer()`/`transferFrom()` not checked | 1 |
| [M-5](#M-5) | Unsafe use of `transfer()`/`transferFrom()` with `IERC20` | 1 |

### <a name="M-1"></a>[M-1] Centralization Risk for trusted owners

#### Impact

Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

*Instances (41)*:

```solidity
File: contracts/erc721NftLoader/ERC721NftLoader.sol

24: contract ERC721NftLoader is ERC721, Ownable {

45:     function setNftLoader(address _nftLoader) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/erc721NftLoader/ERC721NftLoader.sol)

```solidity
File: contracts/governance/twTAP.sol

69: contract TwTAP is TWAML, ERC721, ERC721Permit, Ownable, PearlmitHandler, ERC721NftLoader, ReentrancyGuard, Pausable {

489:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

497:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

501:     function setMaxRewardTokensLength(uint256 _length) external onlyOwner {

511:     function addRewardToken(IERC20 _token) external onlyOwner returns (uint256) {

527:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/option-airdrop/AirdropBroker.sol

43: contract AirdropBroker is Pausable, Ownable, PearlmitHandler, FullMath, ReentrancyGuard {

294:     function setTapToken(address payable _tapToken) external onlyOwner {

302:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

309:     function setPhase2MerkleRoots(bytes32[4] calldata _merkleRoots) external onlyOwner {

355:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

361:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

377:     function daoRecoverTAP() external onlyOwner {

386:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/AirdropBroker.sol)

```solidity
File: contracts/option-airdrop/LTap.sol

21: contract LTap is Ownable, ERC20Permit {

44:     function setTapToken(address _tapToken) external onlyOwner {

48:     function setOpenRedemption() external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/LTap.sol)

```solidity
File: contracts/option-airdrop/aoTAP.sol

30: contract AOTAP is Ownable, PearlmitHandler, ERC721, ERC721Permit, BaseBoringBatchable {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/option-airdrop/aoTAP.sol)

```solidity
File: contracts/options/TapiocaOptionBroker.sol

50: contract TapiocaOptionBroker is Pausable, Ownable, PearlmitHandler, IERC721Receiver, TWAML, ReentrancyGuard {

440:     function setVirtualTotalAmount(uint256 _virtualTotalAmount) external onlyOwner {

448:     function setMinWeightFactor(uint256 _minWeightFactor) external onlyOwner {

455:     function setTapOracle(ITapiocaOracle _tapOracle, bytes calldata _tapOracleData) external onlyOwner {

476:     function setPaymentTokenBeneficiary(address _paymentTokenBeneficiary) external onlyOwner {

482:     function collectPaymentTokens(address[] calldata _paymentTokens) external onlyOwner nonReentrant {

500:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

45:     Ownable,

266:     function setSGLPoolWeight(IERC20 singularity, uint256 weight) external onlyOwner updateTotalSGLPoolWeights {

275:     function setRescueCooldown(uint256 _rescueCooldown) external onlyOwner {

283:     function requestSglPoolRescue(uint256 _sglAssetID) external onlyOwner {

294:     function activateSGLPoolRescue(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

334:     function unregisterSingularity(IERC20 singularity) external onlyOwner updateTotalSGLPoolWeights {

366:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

```solidity
File: contracts/tokens/TapToken.sol

424:     function setMinter(address _minter) external onlyOwner {

433:     function setTwTAP(address _twTap) external override onlyOwner onlyHostChain {

443:     function setPause(bool _pauseState) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapToken.sol)

```solidity
File: contracts/tokens/Vesting.sol

19: contract Vesting is Ownable, ReentrancyGuard {

161:     function registerUser(address _user, uint256 _amount) external onlyOwner {

180:     function registerUsers(address[] calldata _users, uint256[] calldata _amounts) external onlyOwner {

219:     function init(IERC20 _token, uint256 _seededAmount, uint256 _initialUnlock) external onlyOwner {

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/Vesting.sol)

### <a name="M-2"></a>[M-2] Using `transferFrom` on ERC721 tokens

The `transferFrom` function is used instead of `safeTransferFrom` and [it's discouraged by OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/109778c17c7020618ea4e035efb9f0f9b82d43ca/contracts/token/ERC721/IERC721.sol#L84). If the arbitrary address is a contract and is not aware of the incoming ERC721 token, the sent token could be locked.

*Instances (2)*:

```solidity
File: contracts/options/TapiocaOptionBroker.sol

253:             bool isErr = pearlmit.transferFromERC721(msg.sender, address(this), address(tOLP), _tOLPTokenID);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionBroker.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

207:                 pearlmit.transferFromERC1155(msg.sender, address(this), address(yieldBox), sglAssetID, _ybShares);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="M-3"></a>[M-3] Direct `supportsInterface()` calls may cause caller to revert

Calling `supportsInterface()` on a contract that doesn't implement the ERC-165 standard will result in the call reverting. Even if the caller does support the function, the contract may be malicious and consume all of the transaction's available gas. Call it via a low-level [staticcall()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L119), with a fixed amount of gas, and check the return code, or use OpenZeppelin's [`ERC165Checker.supportsInterface()`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/f959d7e4e6ee0b022b41e5b644c79369869d8411/contracts/utils/introspection/ERC165Checker.sol#L36-L39).

*Instances (2)*:

```solidity
File: contracts/governance/twTAP.sol

648:         return super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/governance/twTAP.sol)

```solidity
File: contracts/options/TapiocaOptionLiquidityProvision.sol

394:         return interfaceId == type(IERC1155Receiver).interfaceId || super.supportsInterface(interfaceId);

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/options/TapiocaOptionLiquidityProvision.sol)

### <a name="M-4"></a>[M-4] Return values of `transfer()`/`transferFrom()` not checked

Not all `IERC20` implementations `revert()` when there's a failure in `transfer()`/`transferFrom()`. The function signature has a `boolean` return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

*Instances (1)*:

```solidity
File: contracts/tokens/TapTokenReceiver.sol

194:             TapTokenSender(rewardToken_).sendPacket{

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)

### <a name="M-5"></a>[M-5] Unsafe use of `transfer()`/`transferFrom()` with `IERC20`

Some tokens do not implement the ERC20 standard properly but are still accepted by most code that accepts ERC20 tokens.  For example Tether (USDT)'s `transfer()` and `transferFrom()` functions on L1 do not return booleans as the specification requires, and instead have no return value. When these sorts of tokens are cast to `IERC20`, their [function signatures](https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca) do not match and therefore the calls made, revert (see [this](https://gist.github.com/IllIllI000/2b00a32e8f0559e8f386ea4f1800abc5) link for a test case). Use OpenZeppelin's `SafeERC20`'s `safeTransfer()`/`safeTransferFrom()` instead

*Instances (1)*:

```solidity
File: contracts/tokens/TapTokenReceiver.sol

194:             TapTokenSender(rewardToken_).sendPacket{

```

[Link to code](https://github.com/Tapioca-DAO/tap-token/blob/20a83b1d2d5577653610a6c3879dff9df4968345/contracts/tokens/TapTokenReceiver.sol)
