# Ethereum Go Client P2P-Related Changes

## [v1.15.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.11) (2025-05-05)

This is a maintenance release, correcting issues with log and transaction indexing. Upgrading to this release is not required to follow the Pectra fork on mainnet, you can keep using v1.15.9 or v1.15.10, but we recommend you upgrade at your convenience to fix RPC issues.

### All changes

- A stall condition in `eth_getTransactionByHash` is resolved. (#31752)
- Two bugs in the new log indexer are resolved. Note: upon upgrading the log index will be rebuilt, expect high CPU load after startup. (#31750, #31734)
- Log indexing performance has been improved. (#31716)
- `eth_simulateV1` now correctly returns the transaction sender (`from`). (#31480)
- A corner-case in `eth_estimateGas` related to `floorDataGas` is resolved. (#31735)
- `ethclient`'s `BlockByNumber` can now retrieve the pending block. (#31504)
- `geth init` will once again exit in error when trying to re-initialize a database with an incompatible config. (#31743)
- Transaction pool reorgs are slightly faster. (#31715)
- A transaction pool data race in `geth --dev` mode is resolved. (#31758)

For a full rundown of the changes please consult the Geth 1.15.11 [release milestone](https://github.com/ethereum/go-ethereum/milestone/188?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.10) (2025-04-25)

This is a bug-fix release that corrects an issue with the new log indexer and configures the beacon chain light client for the Electra fork on mainnet (we forgot about this in v1.15.9).

**This release is also suitable for following the Pectra fork on Mainnet.**

### All changes

- A deadlock condition in the log indexer is resolved. This only affected deployments of Geth in archive mode, and old databases still using LevelDB. (#31708)
- The beacon chain light client is now configured for the Pectra fork on mainnet. (#31706)
- The default block gas limit has been increased to 36M. (#31705)
- ethclient now allows passing an EIP-7702 `authorizationList` to calls. (#31198)
- A new RPC endpoint `debug_setMemoryLimit` has been added for tweaking Go garbage collector behavior. We do not recommended using this, it's just a facility for debugging Geth. (#31441)


For a full rundown of the changes please consult the Geth 1.15.10 [release milestone](https://github.com/ethereum/go-ethereum/milestone/187?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.9) (2025-04-21)

This release enables the Prague execution-layer fork on mainnet.

#### Prague

As of this release, the Prague fork is scheduled to occur on mainnet at block timestamp `1746612311` (Wed May 07 10:05:11 2025 UTC). As a reminder, the fork contains the following EIPs:

- [EIP-2537](https://eips.ethereum.org/EIPS/eip-2537): Precompile for BLS12-381 curve operations
- [EIP-2935](https://eips.ethereum.org/EIPS/eip-2935): Save historical block hashes in state
- [EIP-6110](https://eips.ethereum.org/EIPS/eip-6110): Supply validator deposits on chain
- [EIP-7002](https://eips.ethereum.org/EIPS/eip-7002): Execution layer triggerable exits
- [EIP-7251](https://eips.ethereum.org/EIPS/eip-7251): Increase the `MAX_EFFECTIVE_BALANCE`
- [EIP-7549](https://eips.ethereum.org/EIPS/eip-7549): Move committee index outside Attestation
- [EIP-7623](https://eips.ethereum.org/EIPS/eip-7623): Increase calldata cost
- [EIP-7685](https://eips.ethereum.org/EIPS/eip-7685): General purpose execution layer requests
- [EIP-7691](https://eips.ethereum.org/EIPS/eip-7691): Blob throughput increase
- [EIP-7702](https://eips.ethereum.org/EIPS/eip-7702): Set EOA account code
- [EIP-7840](https://eips.ethereum.org/EIPS/eip-7840): Add blob schedule to EL config files

#### All changes

- The Prague fork timestamp was added for mainnet. (#31535)
- Transaction-sending RPCs will now add txs to the 'locals' tracker only when they have any chance of inclusion. This is a bit of a revert from the behavior we added in v1.15.4, where APIs such as `eth_sendRawTransaction` would always return a txhash, even if the transaction wasn't includable on chain. (#31618)
- If an EVM system call fails during block execution, the block is considered invalid. (#31639)
- An corner-case crash in `eth_feeHistory` related to blob fees is resolved. (#31663)
- Several correctness bugs in the new log indexer have been fixed. (#31590, #31680, #31671, #31668, #31642)
- The history pruning implementation was further improved. (#31638, #31636, #31656)
- Geth will now print periodic logs when a non-activated fork is configured. (#31340)
- Geth will now occasionally drop peers at random after being fully synced. (#31476)
- CPU usage of tx propagation has been optimized. (#31657)
- Peer disconnect metrics are improved. (#31629, #31621)

For a full rundown of the changes please consult the Geth 1.15.9 [release milestone](https://github.com/ethereum/go-ethereum/milestone/186?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.8) (2025-04-11)

This is a bug-fix release with some performance improvements.

#### Geth

- `geth import` now applies database and cache flags correctly. (#31577, #31534)
- The new log indexer now exports metrics about its operation. (#31511)
- The beacon chain light client, `blsync` now has a feature to export checkpoint files. (#31469)

#### Core library

- Database writes have been made fully synchronous again. We disabled the use of `fsync` a while ago to improve performance on slow filesystems, but it has lead to reports of instability. The performance hit from enabling data sync is negligible. (#31519)
- The transaction pool now takes pending blob transactions into account when limiting pending EIP-7702 authorizations for an account. (#31526)
- A logic race in EIP-7702 transaction validation is resolved. (#31373)
- The blob transaction pool performs less disk reads when sending transaction announcements. (#31433)
- The EVM now has a special fast-path for PUSH2, which is the most common instruction. (#31267)
- The Trezor hardware wallet implementation now supports 32-bit chain IDs. (#17439)
- Geth can now stop syncing history at the PoS merge point. This behavior is not enabled yet. (#31414)

#### RPC

- When trying to access pruned history, all RPC APIs now return error code 4444. (#31361)

#### P2P networking

- UPnP support has been improved and some bugs got fixed. (#30265, #31486, #31566)
- The discv5 'talk request' API has been changed to pass `*enode.Node` to handlers. This is a breaking change, but the only known user of this API is the shisui portal network client. (#31075)
- A flaw in the recently added discv5 challenge resend logic was fixed. (#31543)
- The eth protocol now properly handles very large `skip` values when processing `GetBlockHeaders` messages from peers. This is not a security fix, despite looking like one, it's more about correctness. (#31522)

#### Build

- This release is built with Go 1.24.2 (#31538)
- Note: due to issues with our build environment, we can no longer provide binary builds for macOS. These may be restored at a later date, hopefully soon. For now, you'll have to install from Homebrew.
- The previous release's git tag, v1.15.7, was published twice, leading to an issue with the Go module cache.

For a full rundown of the changes please consult the Geth 1.15.8 [release milestone](https://github.com/ethereum/go-ethereum/milestone/185?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.7) (2025-03-31)

This is a bug fix release. We are putting is out specifically to address a critical issue that could break archive node databases.

- Fixed an issue for `--state.scheme=hash` where the log indexer would accidentally delete trie nodes. (#31525)
- Fixed an issue with tx submission, where the local pool didn't track pending nonces correctly. (#31496)
- The log indexer will now disable itself when hitting missing receipts in the database. (#31500)
- Another txpool issue reorg issue in `ethclient/simulated.Backend` is fixed in this release. (#31228)
- Memory allocation for trie operations has been reduced significantly. (#30932)
- `eth_createAccessList` now supports state overrides like `eth_call`. (#31497)
- `eth_createAccessList` will now exclude 7702 authorities from the result. (#31336)
- The abigen library now correctly forwards access lists to `eth_estimateGas`. (#31394)

For a full rundown of the changes please consult the Geth 1.15.7 [release milestone](https://github.com/ethereum/go-ethereum/milestone/184?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.6) (2025-03-25)

🚧  **Note: we are investigating an issue with this release that affects archive nodes (--gcmode=archive).
 If you are running Geth in this mode, please hold off upgrading.** 🚧 

This is a feature release, with two exciting upgrades:

- Log filtering in Geth receives a huge performance upgrade with the introduction of our new 'filtermaps' index. Unlike the previous 'bloombits' index, query performance no longer suffers as the density of logs in a block increases. The new index design is also a step towards a future where filtering results can be proven by the server. See [the PR](https://github.com/ethereum/go-ethereum/pull/31079) and associated design documents for more information.

  In practical terms, the new index is a bit larger than the old one. As before, you can choose the amount of historical blocks to be indexed using the `--history.logs` command-line flag. We have adjusted the default value of this flag to cover one year of history, and the resulting index has a size of ~10GB for Ethereum mainnnet. Indexing of the entire chain with `--history.logs=0` will take up ~61GB.

  Once the index is built, searches will be fast, but note that querying outside of the indexed block range will fall back to a very slow unindexed search. We will continue optimizing log searches in future releases, and welcome your feedback and bug reports in this area. (#31065, #31079, #31080, #31081, #31419, #31429, #31450, #31463, #31455)

- [abigen v2](https://geth.ethereum.org/docs/developers/dapp-developer/native-bindings-v2) is finally here. abigen is a tool for creating Go bindings for Solidity contracts. In v1, the generated bindings presented an API for sending transactions, filtering logs, and performing read-only calls as Go methods on the contract object. In the new version, we have updated the interface of the generated code to focus purely on encoding and decoding ABI payloads. Generic helper functions are provided in a library package to enable the same interactions as before, but you can also use your own custom method of signing & sending transactions. Generated bindings are also significantly smaller. (#31379)

Other changes in this release:

#### RPC

- A regression in `eth_sendRawTransaction` - where transactions with too-low nonce would be accepted by the API - has been fixed. (#31473)
- `eth_call`/`estimateGas` RPC methods will now always return error code 3 for reverts. It previously only returned this code when the EVM produced revert data. (#31456)
- `eth_simulateV1` now returns a correct logs bloom value in the simulated block (#31411)
- `eth_simulateV1` supports block overrides for the beacon root and withdrawals (#31304)
- `debug_traceCall`: the `movePrecompileTo` override feature should now work correctly (#31348)
- `debug_traceCall` and other related RPC methods now hex-encode the EVM return value. **This is a breaking change.** (#31216, #31445)
- `ethclient` has a new method `EstimateGasAtBlock` (#27508)

#### Geth

- Support for the [Hoodi testnet](https://github.com/eth-clients/hoodi) has been added. (#31406)
- The beacon chain light client has been updated to support the Electra fork. (#31243, #31470)
- `geth import` now properly handles Ctrl-C interrupts (#31360)
- `geth --dev` mode will now pre-fund all precompile accounts supported by the Pectra upgrade (#31342)
- `geth --dev` now respects the `--miner.pending.feeRecipient` flag (#31316)

#### Core library

- The performance of transaction signature validation has improved significantly (#31242, #31258, #31434)
- The transaction pool has seen some correctness and performance improvements. (#31430, #31307, #31332)
- We have added a lot of preliminary changes for History Expiry (EIP-4444). There shouldn't be any user-facing consequences of these just yet, just reporting it here since this area was a big focus this cycle. (#31424, #31355, #31362, #31383, #31365, #31117, #31356, #31384, #31393)

For a full rundown of the changes please consult the Geth 1.15.6 [release milestone](https://github.com/ethereum/go-ethereum/milestone/183?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.5) (2025-03-05)

Alright 🥲 This is a hotfix release for an issue on the Sepolia testnet. Shortly after the Pectra fork activation, an issue in deposit contract logs parsing was discovered. Sepolia has a custom version of the deposit contract that also implements ERC-20, and thus unexpected log events may be created by transactions to it. This issue affects all Ethereum execution clients.

The Sepolia testnet may take a short while to recover while the update is adopted by nodes. An incident report will be published once the network has fully recovered. ❤️‍🩹 

Other changes in this release:

- In output of `debug_traceTransaction`, the "memory" and "storage" fields will be omitted if empty. (#31289)
- This release is built with Go 1.24.1 (#31313)

For a full rundown of the changes please consult the Geth 1.15.5 [release milestone](https://github.com/ethereum/go-ethereum/milestone/182?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).


## [v1.15.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.4) (2025-03-01)

This is a bug fix release. 

**Note: you need to upgrade to v1.15.3 or this release to be compatible with the Pectra fork on the Sepolia network (activates Wed, Mar 5 at 07:29:36 UTC)**.

- Fixed a v1.15.0 regression in `eth_feeHistory` that caused incorrect `blobGasRatio` return values. (#31246, #31270)
- A v1.15.0 regression in RPC transaction submission has been fixed: if a transaction did not pass txpool verification (e.g. low fees), an error was returned by RPC, but the transaction would be added to the local pool anyway. This is now fixed and no error will be returned by the API in this case. (#31202)
- Txpool logic was reworked to avoid an error log flood about EIP-7702 authorities. (#31249)
- Certain invalid blob transactions no longer cause disconnect issues in the p2p layer. (#31219)
- ethclient now provides a `BlobBaseFee` method to request the current blob basefee. (#31290)
- The PPA package build was fixed after being broken in v1.15.3 by the upgrade to Go 1.24. (#31282, #31283)

For a full rundown of the changes please consult the Geth 1.15.4 [release milestone](https://github.com/ethereum/go-ethereum/milestone/181?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.3) (2025-02-25)

Oh look, another hotfix release! We are issuing this Geth release to correct the predefined fork configuration of the Holesky and Sepolia testnets. The deposit contract address was missing in the configuration for these networks, causing a chain validation failure.

This issue was discovered on the Holesky network after it had already forked into Pectra (Prague). As a reminder, the Sepolia network will fork to Pectra at slot 7118848 (Wed, Mar 5 at 07:29:36 UTC). You need to upgrade to Geth v1.15.3 until then in order to use the testnet after the fork.

All changes in this release:

- Deposit contract addresses are now defined in testnet fork configuration. (#31247)
- The `eth_simulateV1` RPC method was improved to match regular block processing semantics. (#31176, #31122)
- A peer-finding issue with discovery v5 is fixed in this release. (#31251)
- An invalid `--discovery.dns` flag value will now cause an error at Geth startup. (#31233)
- Geth `--dev` mode can now handle custom genesis configs with forks older than the latest. (#31084)
- The EVM assembler/disassembler (package `core/asm`) has been removed. (#31211)
- Encoding of nested byte arrays in EIP-712 signature processing was fixed. (#31049)
- The cloudflare-go dependency has been updated to resolve a dependabot warning. (#31240)
- This release is built with Go 1.24.0 (#31159)

For a full rundown of the changes please consult the Geth 1.15.3 [release milestone](https://github.com/ethereum/go-ethereum/milestone/180?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.2) (2025-02-17)

This release fixes a few regressions. In particular, it restores block building on mainnet, which was broken in v1.15.1.

**Note: a regression in the Prague fork implementation was discovered and fixed in a follow-up release. Please upgrade to at least v1.15.3 to follow the fork.**

All changes in this release:

- Block building on mainnet works again. ([#31191](https://github.com/ethereum/go-ethereum/pull/31191))
- Discv5 and DNS peer discovery has been restored. It was accidentally disabled in v1.14.9. ([#31185](https://github.com/ethereum/go-ethereum/pull/31185))
- An edge-case for `geth dumpconfig` related to the `P2P.NAT` setting in TOML is fixed. ([#31192](https://github.com/ethereum/go-ethereum/pull/31192))

For a full rundown of the changes please consult the Geth 1.15.2 [release milestone](https://github.com/ethereum/go-ethereum/milestone/179?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.1) (2025-02-13)

This release enables the Prague fork for the Holesky and Sepolia testnets. It is a required upgrade if you want to participate in these networks.

**Note: a regression in the Prague fork implementation was discovered and fixed in a follow-up release. Please upgrade to at least v1.15.3 to follow the fork.**

The fork is set to activate at

- **Holesky** slot: 3710976, timestamp: Mon, Feb 24 at 21:55:12 UTC
- **Sepolia** slot: 7118848, timestamp: Wed, Mar 5 at 07:29:36 UTC

Other user-facing changes in this release:

- Support for EIP-7702 transactions has been enabled in the transaction pool. (#31073)
- A v1.15.0 regression was identified in genesis handling for custom networks, related to the new `blobSchedule` setting. Geth v1.15.1 has been updated no longer crash in this case, and we will have an improved fix in v1.15.2 (#31171)
- A v1.15.0 regression in the `evm` command related to state dumps has been fixed. (#31158)
- A v1.15.0 regression with the ancient store in read-only mode has been fixed. (#31173)
- A crash in `debug_getRaw*` RPC methods has been fixed. (#31157)
- The supranational/blst dependency has been upgraded to v0.3.14 in order to allow building Geth with Go 1.24. (#31165)

For a full rundown of the changes please consult the Geth 1.15.1 [release milestone](https://github.com/ethereum/go-ethereum/milestone/178?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.15.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.0) (2025-02-06)

We are proud to announce the Geth 1.15.0 release.

This release contains some breaking changes to the database. Downgrading to v1.14.x requires re-syncing the chain.

### Prague Fork

As of this release, Geth implements the Prague fork specifications up to devnet-6.

- [EIP-2537](https://eips.ethereum.org/EIPS/eip-2537) - BLS12-381 curve operations - has seen some updates. ([#30978](https://github.com/ethereum/go-ethereum/pull/30978))
- [EIP-7623](https://eips.ethereum.org/EIPS/eip-7623) - Increase calldata cost - was implemented this cycle. ([#30946](https://github.com/ethereum/go-ethereum/pull/30946))
- [EIP-7685](https://eips.ethereum.org/EIPS/eip-7685) - Execution layer requests - support was updated. ([#30670](https://github.com/ethereum/go-ethereum/pull/30670), [#30829](https://github.com/ethereum/go-ethereum/pull/30829), [#31103](https://github.com/ethereum/go-ethereum/pull/31103), [#31010](https://github.com/ethereum/go-ethereum/pull/31010), [#31071](https://github.com/ethereum/go-ethereum/pull/31071), [#30938](https://github.com/ethereum/go-ethereum/pull/30938), [#31102](https://github.com/ethereum/go-ethereum/pull/31102))
- [EIP-7702](https://eips.ethereum.org/EIPS/eip-7685) - Set EOA account code - was implemented this cycle. Note Geth does not support EIP-7702 transactions in the txpool just yet. ([#30078](https://github.com/ethereum/go-ethereum/pull/30078), [#30935](https://github.com/ethereum/go-ethereum/pull/30935), [#30936](https://github.com/ethereum/go-ethereum/pull/30936), [#31054](https://github.com/ethereum/go-ethereum/pull/31054), [#31032](https://github.com/ethereum/go-ethereum/pull/31032), [#30933](https://github.com/ethereum/go-ethereum/pull/30933), [#30982](https://github.com/ethereum/go-ethereum/pull/30982), [#31089](https://github.com/ethereum/go-ethereum/pull/31089), [#30926](https://github.com/ethereum/go-ethereum/pull/30926))
- [EIP-7840](https://eips.ethereum.org/EIPS/eip-7840) - Blob schedule in EL config - was implemented this cycle. ([#31002](https://github.com/ethereum/go-ethereum/pull/31002), [#31101](https://github.com/ethereum/go-ethereum/pull/31101), [#31128](https://github.com/ethereum/go-ethereum/pull/31128))

### Command changes

- The `evm` command has seen lots of updates, improving its command-line interface and internals. ([#30633](https://github.com/ethereum/go-ethereum/pull/30633), [#30849](https://github.com/ethereum/go-ethereum/pull/30849), [#30806](https://github.com/ethereum/go-ethereum/pull/30806), [#30927](https://github.com/ethereum/go-ethereum/pull/30927), [#30780](https://github.com/ethereum/go-ethereum/pull/30780), [#30854](https://github.com/ethereum/go-ethereum/pull/30854), [#31055](https://github.com/ethereum/go-ethereum/pull/31055), [#30805](https://github.com/ethereum/go-ethereum/pull/30805), [#30804](https://github.com/ethereum/go-ethereum/pull/30804), )
- Geth metrics can now be enabled via the TOML config file. This used to require the `--metrics` flag. ([#30814](https://github.com/ethereum/go-ethereum/pull/30814))
- Similarly, you can now set the NAT traversal method (`--nat` flag) with the config file. ([#31041](https://github.com/ethereum/go-ethereum/pull/31041))
- The `geth --trace` flag is now called `--go-execution-trace` to avoid confusion with EVM tracing. ([#30846](https://github.com/ethereum/go-ethereum/pull/30846))
- The `bootnode` utility has been removed. ([#30813](https://github.com/ethereum/go-ethereum/pull/30813))

### RPC & Tracing

- The live tracing Go API has been updated with new hooks and optional EVM revert tracking. See the [tracing API changelog](https://github.com/ethereum/go-ethereum/blob/master/core/tracing/CHANGELOG.md) for more information. ([#30441](https://github.com/ethereum/go-ethereum/pull/30441), [#30786](https://github.com/ethereum/go-ethereum/pull/30786), [#30830](https://github.com/ethereum/go-ethereum/pull/30830), [#30784](https://github.com/ethereum/go-ethereum/pull/30784), [#31007](https://github.com/ethereum/go-ethereum/pull/31007))
- RPC method `eth_estimateGas` now supports block overrides like `eth_call`. ([#30695](https://github.com/ethereum/go-ethereum/pull/30695))
- RPC method `eth_simulateV1` now advances the timestamp by 12 seconds per block. ([#30981](https://github.com/ethereum/go-ethereum/pull/30981))
- Package `accounts/abi` now supports unpacking Solidity error types. ([#30738](https://github.com/ethereum/go-ethereum/pull/30738))
- Package `accounts/abi/bind` has new functions `WaitMinedHash`, `WaitDeployedHash`. ([#30079](https://github.com/ethereum/go-ethereum/pull/30079))
- Package `accounts/usbwallet` was updated to support new Ledger firmware and the Ledger Flex device. ([#31004](https://github.com/ethereum/go-ethereum/pull/31004))
- In package `ethclient/simulated`, a bug was fixed where transactions couldn't be sent after `Rollback` of the simulated chain. ([#31020](https://github.com/ethereum/go-ethereum/pull/31020))
- The struct logger reports the EVM revert reason correctly again. ([#31013](https://github.com/ethereum/go-ethereum/pull/31013))

### Database

This release introduces a new database schema version. **A downgrade to v1.14.x requires a resync.**

- The 'total difficulty' of the chain is no longer stored in the database. ([#30744](https://github.com/ethereum/go-ethereum/pull/30744))
- The ancient store employs a new strategy for avoiding fsync on each update. This change should improve block import latency on some systems. ([#30392](https://github.com/ethereum/go-ethereum/pull/30392))
- Reverse state diffs use a new encoding for future compatibility with the Verkle Tree. ([#30107](https://github.com/ethereum/go-ethereum/pull/30107), [#31060](https://github.com/ethereum/go-ethereum/pull/31060))
- Internal handling of the SELFDESTRUCT operation has been simplified. This is possible since classic selfdestruct is no longer supported by the EVM after the Cancun fork. ([#30752](https://github.com/ethereum/go-ethereum/pull/30752), [#30802](https://github.com/ethereum/go-ethereum/pull/30802))
- Work towards a re-implementation of the state snapshot system is ongoing. ([#30643](https://github.com/ethereum/go-ethereum/pull/30643), [#30650](https://github.com/ethereum/go-ethereum/pull/30650), [#30654](https://github.com/ethereum/go-ethereum/pull/30654))
- Work on the Verkle Tree integration is also progressing. ([#31036](https://github.com/ethereum/go-ethereum/pull/31036), [#30856](https://github.com/ethereum/go-ethereum/pull/30856), [#30907](https://github.com/ethereum/go-ethereum/pull/30907), [#31097](https://github.com/ethereum/go-ethereum/pull/31097))

### Core Library & Networking

- The fix for [CVE-2025-24883](https://github.com/ethereum/go-ethereum/security/advisories/GHSA-q26p-9cq4-7fc2) (recent v1.14.13 hotfix release) is also in v1.15.0 ([#31100](https://github.com/ethereum/go-ethereum/pull/31100)).
- The transaction pool's notion of "locals" has been changed. This is a breaking change in some ways, and requires some explanation. Geth used to accept transactions at any fee level via RPC, storing them into the node's pool, and marking the sender as "local". When creating a new block (or the pending block), it would choose "local" transactions first. Finally, Geth tracks "local" transactions in a persistent journal file, and restores them on startup. The effect of all this is that once you had sent a transaction to Geth via RPC, it would keep prioritizing the sender account and its transactions (even ones received through p2p).

  In Geth v1.15, we are changing this model to a different one. When receiving a transaction via RPC, it is stored into the pool like any other transaction, but it is also inserted into a separate "locals tracker" that keeps reinjecting the tx into the main pool periodically. When building a block, Geth no longer prefers the txpool locals, but you can configure a set of addresses that should get priority using the `--txpool.locals` CLI flag. ([#30559](https://github.com/ethereum/go-ethereum/pull/30559), [#31127](https://github.com/ethereum/go-ethereum/pull/31127))
- For static nodes (from configuration or RPC `admin_addPeer`), the node URL can contain a DNS name. Geth will now resolve these names when attempting to connect to the peer, instead of at configuration parsing time. This is useful for setting up testing networks in Docker or Kubernetes where the IP address of a node might not be known ahead of time. ([#30822](https://github.com/ethereum/go-ethereum/pull/30822))
- Geth can now use the STUN protocol to figure out its own IP. There is a built-in list of public servers, or you can specify your own. Use `--nat=stun` to enable. ([#31064](https://github.com/ethereum/go-ethereum/pull/31064))
- Some minor bugs in the p2p protocol implementation got fixed. ([#30855](https://github.com/ethereum/go-ethereum/pull/30855), [#30918](https://github.com/ethereum/go-ethereum/pull/30918))

### Build

- This release is built with Go 1.23.6 ([#31130](https://github.com/ethereum/go-ethereum/pull/31130))
- The 'toolchain' line has been removed from go.mod ([#31057](https://github.com/ethereum/go-ethereum/pull/31057))
- go-ethereum library functionality should now build and run on wasip1 ([#31090](https://github.com/ethereum/go-ethereum/pull/31090))

For a full rundown of the changes please consult the Geth 1.15.0 [release milestone](https://github.com/ethereum/go-ethereum/milestone/177?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.13) (2025-01-30)

This is a security release, fixing a vulnerability (CVE-2025-24883). 

**Please update your nodes ASAP.**

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.12) (2024-11-19)

This release covers quite a lot of time, and has many changes across the codebase. In particular; changes in tracing and account management, optimizations in database, trie and evm, and, as always bugfixes. 

This release removes the `personal` RPC namespace. It was already previously deprecated, and has not been accessible by default for nearly two years. We also removed the `--unlock` command-line parameter, with a view towards removing key/account management from the `geth` binary.

- Key management:
  - Remove `--unlock` command line flag from `geth` ([#30737](https://github.com/ethereum/go-ethereum/pull/30737))
  - Remove `personal` RPC namespace ([#30704](https://github.com/ethereum/go-ethereum/pull/30704))
- Builds:
  - RISC-V docker images ([#30739](https://github.com/ethereum/go-ethereum/pull/30739))
  - ppa-builds for ubuntu `24.10` ([#30580](https://github.com/ethereum/go-ethereum/pull/30580))
- Tracing:
  - invoke OnCodeChange-hook on selfdestruct ([#30686](https://github.com/ethereum/go-ethereum/pull/30686), [#30497](https://github.com/ethereum/go-ethereum/pull/30497))
  - improvements to `flatCallTracer` ([#30539](https://github.com/ethereum/go-ethereum/pull/30539))
  - invoke tx-end hook in runtime helpers ([#30711](https://github.com/ethereum/go-ethereum/pull/30711))
  - `disableCode` and `disableStorage` options for `prestateTracer` ([#30648](https://github.com/ethereum/go-ethereum/pull/30648))
  - tracing of system calls ([#30666](https://github.com/ethereum/go-ethereum/pull/30666))
  - Change to how chainconfig is passed to tracers (**breaking change**) ([#30540](https://github.com/ethereum/go-ethereum/pull/30540))
  - add `GetTransientState` method to StateDB interface ([#30531](https://github.com/ethereum/go-ethereum/pull/30531))
- Signing:
  - improved support for EIP-712 array types ([#30620](https://github.com/ethereum/go-ethereum/pull/30620))
  - non-legacy transactions support for ledger wallets ([#30180](https://github.com/ethereum/go-ethereum/pull/30180))
- Bugfixes:
  - Set basefee for `AccessList` based on given block, not chain tip ([#30538](https://github.com/ethereum/go-ethereum/pull/30538))
  - Multiple issues in benchmarks  ([#30667](https://github.com/ethereum/go-ethereum/pull/30667))
  - Fix bug with resolving a payload ([#30615](https://github.com/ethereum/go-ethereum/pull/30615))
- Database optimizations
  - Run pebble in non-sync mode ([#30573](https://github.com/ethereum/go-ethereum/pull/30573), [#29792](https://github.com/ethereum/go-ethereum/pull/29792)). This change makes quite a big difference on certain OS:es, particularly MacOSX/Darwin, where
    it has been noted that `fsync` is notoriously slow.
  - Use to increasing level sizes ([#30602](https://github.com/ethereum/go-ethereum/pull/30602)), This change makes pebble use larger files, reducing the number of files from 160K to 10K.
- Assorted:
  - Make jwtsecretflag expand tilde
  - Work on verkle ([#30672](https://github.com/ethereum/go-ethereum/pull/30672))
  - Work on `EIP-7002` and `EIP 7251` ([#30571](https://github.com/ethereum/go-ethereum/pull/30571))
  - Implement `EIP-7685` and `EIP-6110` (flat requests enconding) ([#30425](https://github.com/ethereum/go-ethereum/pull/30425))
  - Validation of EOF containers ([#30418](https://github.com/ethereum/go-ethereum/pull/30418))
  - More helpful responses for rejected transactions ([#30715](https://github.com/ethereum/go-ethereum/pull/30715))
  - Work on cross-execution witness ([#30698](https://github.com/ethereum/go-ethereum/pull/30698))
  - EVM speed optimizations ([#30662](https://github.com/ethereum/go-ethereum/pull/30662), [#30629](https://github.com/ethereum/go-ethereum/pull/30629))
  - Reduce peak memory usage during reorg ([#30600](https://github.com/ethereum/go-ethereum/pull/30600))
  - Speed up trie commit via concurrent workers ([#30545](https://github.com/ethereum/go-ethereum/pull/30545))
  - Fuzzing improvments ([#30585](https://github.com/ethereum/go-ethereum/pull/30585))
   - Implement new `engine_getBlobsV1` API method ([#30537](https://github.com/ethereum/go-ethereum/pull/30537))



For a full rundown of the changes please consult the Geth [1.14.12 release milestone](https://github.com/ethereum/go-ethereum/milestone/175?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.11) (2024-10-01)

This is a minor release, with the primary goal of publishing new `stable` and `latest` docker images. A problem in the CI pipeline prevented the publishing of docker images. We have now resolved the problem, and hope that the `v1.14.11`-release will be published as usual on Docker hub. 

We have now switched the way the docker images are built, and the `-amd64` and `-arm64`-tagged versions  **will no longer be maintained**: 
- `alltools-latest-amd64`, `alltools-latest-arm64` -> **`alltools-latest`**
- `latest-amd64`,`latest-arm64` -> **`latest`**
- `v1.14.10-amd64`, `v1.14.10-arm64` -> **`v1.14.10`**


**NOTE:** If you are a docker user on `stable`/`latest`, there's a high chance that you are not using any of the last two releases. If so, you are advised to look through the release-notes of those releases respectively: [v1.14.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.10) and [v1.14.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.9). 

Other changes since v1.14.10 include
- Remove `totalDifficulty` field from RPC, in accordance with [spec update](https://github.com/ethereum/execution-apis/pull/570),  [#30386](https://github.com/ethereum/go-ethereum/pull/30386)
- Fix flaw in simulated backend  [#30465](https://github.com/ethereum/go-ethereum/pull/30465)
- New method of building multi-platform docker images [#30530](https://github.com/ethereum/go-ethereum/pull/30530)
- Ability to disable `FINDNODE` liveness checks in tests [#30512](https://github.com/ethereum/go-ethereum/pull/30512)

For a full rundown of the changes please consult the Geth 1.14.11 [release milestone](https://github.com/ethereum/go-ethereum/milestone/174?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).


## [v1.14.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.10) (2024-09-27)

Geth v1.14.10 is a hotfix release to fix a [blob pool regression](https://github.com/ethereum/go-ethereum/pull/30518) introduced in v1.14.9. Users running the previous bad version should update ASAP. That said, there is no immediate danger to these users, just to the health of blob transaction propagation and inclusion in the network.

Beside the hotfix, this release:

- Ships stateless witness production and verification into the engine API ([#30069](https://github.com/ethereum/go-ethereum/pull/30069)).
- Use 0 global gas cap as unlimited in simulated calls too ([#30474](https://github.com/ethereum/go-ethereum/pull/30474), [#30496](https://github.com/ethereum/go-ethereum/pull/30496)).

For a full rundown of the changes please consult the Geth [1.14.10 release milestone](https://github.com/ethereum/go-ethereum/milestone/173?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.9) (2024-09-18)

***This release has been nuked. It broke the blob pool, please do not use it.***

---

This is a maintenance release, but also introduces support for the new multicall spec, which is a much anticipated feature providing a `eth_simulateV1` call, that takes a list of blocks and executes them as if calling multiple `eth_call`s sequentially. It accepts optional state and precompile overrides, as well as transfer log events. This release also ships with improved verkle support.

## Command line

* Remove Goerli flag and config (https://github.com/ethereum/go-ethereum/pull/30289)

## Pectra

* Implement [EIP-6110: Supply validator deposits on chain](https://eips.ethereum.org/EIPS/eip-6110) (https://github.com/ethereum/go-ethereum/pull/29431)
* Implement [EIP-2935: Serve historical block hashes from state](https://eips.ethereum.org/EIPS/eip-2935) (https://github.com/ethereum/go-ethereum/pull/29465)

## Core

* Refactorings in the internal journaling (https://github.com/ethereum/go-ethereum/pull/28880)
* Fix a flaw in which the system halts if snapshot generation is stopped while it's not running (https://github.com/ethereum/go-ethereum/pull/30040)
* Fix potential out-of-bound issue in mempool (https://github.com/ethereum/go-ethereum/pull/30430)
* Add state reader abstraction (https://github.com/ethereum/go-ethereum/pull/29761)

## Txpool

* Fetch transactions from a peer in the order they were announced in order to minimize nonce gap, causing blob txs to rejected. A special rule is applied to blob transactions: they are retrieved from the network upon reception of the announcement, as blob transactions are never broadcast over the p2p network (https://github.com/ethereum/go-ethereum/pull/30125)

## Networking

* Add support for quic entry in ENR (https://github.com/ethereum/go-ethereum/pull/30283)
* fix Write method in metered connection (https://github.com/ethereum/go-ethereum/pull/30355)
* Enable discv5 by default (https://github.com/ethereum/go-ethereum/pull/30327)
* Proper handling for count=0 requests (https://github.com/ethereum/go-ethereum/pull/30305)
* Fix permissions for cloudflare deploy (https://github.com/ethereum/go-ethereum/pull/30326)
* Dial nodes from discv5 (https://github.com/ethereum/go-ethereum/pull/30302)
 
## RPC / tracing

* support for `eth_simulateV1`, which allows for the simulation a chain of blocks or simply a processing a sequence of eth_calls in one go. It is the implementation of the latest [multicall spec](https://github.com/ethereum/execution-apis/pull/484) (https://github.com/ethereum/go-ethereum/pull/27720)
* Add timeout to `Client.Unsubscribe` (https://github.com/ethereum/go-ethereum/pull/30318)
* Add coinbase addr to js tracing context (https://github.com/ethereum/go-ethereum/pull/30231)

## Misc

* Handle ABIs with `contract` type parameters (https://github.com/ethereum/go-ethereum/pull/30315)
* Support fixed-size arrays in eip-712 txs (https://github.com/ethereum/go-ethereum/pull/30175)
* Fix txpool deadlock in `--dev` mode (https://github.com/ethereum/go-ethereum/pull/30264)
* Use post-interop verkle costs (https://github.com/ethereum/go-ethereum/pull/30409, https://github.com/ethereum/go-ethereum/pull/30357)
* Verkle witness builder (https://github.com/ethereum/go-ethereum/pull/30129)
 
## Build

* Work towards [reproducible builds](https://github.com/ethereum/go-ethereum/issues/28987) (https://github.com/ethereum/go-ethereum/pull/30344, https://github.com/ethereum/go-ethereum/pull/30342, [#30346](https://github.com/ethereum/go-ethereum/pull/30346), [#30325](https://github.com/ethereum/go-ethereum/pull/30325), [#30321](https://github.com/ethereum/go-ethereum/pull/30321), [#30320](https://github.com/ethereum/go-ethereum/pull/30320), [#29723](https://github.com/ethereum/go-ethereum/pull/29723))

**Full Changelog**: https://github.com/ethereum/go-ethereum/compare/v1.14.8...v1.14.9

For a full rundown of the changes please consult the Geth [1.14.9 release milestone](https://github.com/ethereum/go-ethereum/milestone/172?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.8) (2024-08-12)

This is a maintenance release with bug fixes only.

### Command changes

- Blobpool related flags in Geth now actually work. ([#30203](https://github.com/ethereum/go-ethereum/pull/30203))
- The `evm run` command no longer overwrites the sender account in genesis.json. ([#30259](https://github.com/ethereum/go-ethereum/pull/30259))
- `evm run` now allows configuring `baseFeePerGas` in genesis.json. ([#30281](https://github.com/ethereum/go-ethereum/pull/30281))

### Go API

- `core/types.Transaction.ChainID` had a bug where it modified the signature for very large ChainID (>= 2^64). ([#30157](https://github.com/ethereum/go-ethereum/pull/30157))
- `ethclient.Client.NetworkID` now supports values returned in hex format by the server. ([#30263](https://github.com/ethereum/go-ethereum/pull/30263))
- `ethclient/simulated.Backend.AdjustTime` was fixed to apply the correct time divison. ([#30138](https://github.com/ethereum/go-ethereum/pull/30138))
- `accounts/abi/bind.TransactOpts` now supports setting an access list for created transactions. ([#30195](https://github.com/ethereum/go-ethereum/pull/30195))
- The package `p2p/simulations` has been removed. ([#30250](https://github.com/ethereum/go-ethereum/pull/30250))

### Core

- A snap-sync database corruption related to sync restarts is fixed in this release. ([#30258](https://github.com/ethereum/go-ethereum/pull/30258))
- `eth_call` storage overrides now work as originally intended: if a storage replacement object is specified in the call, previous storage values of the account are cleared. ([#30185](https://github.com/ethereum/go-ethereum/pull/30185))
- The txpool did not use the transaction's inline sender cache in some cases. ([#30208](https://github.com/ethereum/go-ethereum/pull/30208))
- The performance of EVM stack swaps was improved a bit. ([#30249](https://github.com/ethereum/go-ethereum/pull/30249))

### Networking

- The downloader now takes withdrawals into account when sizing its queue. ([#30276](https://github.com/ethereum/go-ethereum/pull/30276))
- The new discovery node revalidation could hot-spin in certain rare scenarios. ([#30239](https://github.com/ethereum/go-ethereum/pull/30239))
- Configuring an external IP using `--nat=extip:...` could lead to invalid discovery packets being generated. ([#30234](https://github.com/ethereum/go-ethereum/pull/30234))

### Build

- github.com/btcsuite/btcd/btcec has been upgraded to resolve a build error caused by upstream API changes. ([#30181](https://github.com/ethereum/go-ethereum/pull/30181))
- Previous releases will not build with Go 1.23, but this one will. ([#30253](https://github.com/ethereum/go-ethereum/pull/30253))
- This release is built with Go 1.22.6. ([#30273](https://github.com/ethereum/go-ethereum/pull/30273))

For a full rundown of the changes please consult the Geth [1.14.8 release milestone](https://github.com/ethereum/go-ethereum/milestone/171?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.7) (2024-07-11)

This is a hot-fix release for a bug ([#30139](https://github.com/ethereum/go-ethereum/issues/30139)) which affects only the previous release. Users of v1.14.6 are kindly requested to update.

For a full rundown of the changes please consult the Geth [1.14.7 release milestone](https://github.com/ethereum/go-ethereum/milestone/170?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.6) (2024-07-02)

Geth v1.14.6 is a maintenance release, but it does ship with the _experimental_ witness building validation code used in @karalabe's ["cross validation" proposal](https://gist.github.com/karalabe/47c906f0ab4fdc5b8b791b74f084e5f9). Note that the engine API part is not included in this release.

---

Shipped features:
 
 * Add stateless witness builder and (self-)cross validator ([#29719](https://github.com/ethereum/go-ethereum/pull/29719), [#29807](https://github.com/ethereum/go-ethereum/pull/29807), [#29970](https://github.com/ethereum/go-ethereum/pull/29970), [#30024](https://github.com/ethereum/go-ethereum/pull/30024))
 * Set a 2KB hard limit for p2p handshake messages ([#30029](https://github.com/ethereum/go-ethereum/pull/30029))
 * Improved display of database statistics ([#29948](https://github.com/ethereum/go-ethereum/pull/29948))

Shipped bugfixes:

 * Fix issue in which the beacon root contract balance would not be saved in developer mode, causing an error on restart ([#29963](https://github.com/ethereum/go-ethereum/pull/29963))
 * Fix shutdown crash when geth runs in blsync mode ([#29946](https://github.com/ethereum/go-ethereum/pull/29946))
 * Fix data races in snapshot access ([#30001](https://github.com/ethereum/go-ethereum/pull/30001), [#30011](https://github.com/ethereum/go-ethereum/pull/30011))
 * Fix out of bounds access in json unmarshalling ([#30014](https://github.com/ethereum/go-ethereum/pull/30014))
 * Add missing lock in peer discovery ([#29960](https://github.com/ethereum/go-ethereum/pull/29960))

For a full rundown of the changes please consult the Geth [1.14.6 release milestone](https://github.com/ethereum/go-ethereum/milestone/169?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.5) (2024-06-06)

Geth v1.14.5 is a hotfix release that addresses a regression introduced in v1.14.4, which prevented the node from discovering other peers in certain networking setups ([#29944](https://github.com/ethereum/go-ethereum/pull/29944)). It is otherwise identical to [v1.14.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.4).

---

Geth v1.14.4 in a usual maintenance release, but it does ship a 5-7% block import [speed improvement](https://github.com/ethereum/go-ethereum/pull/29519). Furthermore, v1.14.4 also finally includes an [Ether supply live tracer](https://github.com/ethereum/go-ethereum/pull/29347), that you can enable via `--vmtrace supply`. Also please note, the default value for miner tip enforcement was dropped from 1 gwei to 0.001 gwei (block producers can change this via `--miner.gasprice`).

Shipped features:

- Reduce the default required minimum miner tip from 1 gwei to 0.001 gwei to cater better for network conditions ([#29895](https://github.com/ethereum/go-ethereum/pull/29895)).
- Load trie nodes concurrently with trie updates, speeding up block import by 5-7% ([#29519](https://github.com/ethereum/go-ethereum/pull/29519), [#29768](https://github.com/ethereum/go-ethereum/pull/29768), [#29919](https://github.com/ethereum/go-ethereum/pull/29919)).
- Introduce an Ether supply tracker as a live chain tracer ([#29347](https://github.com/ethereum/go-ethereum/pull/29347)).
- Implement Verkle stateless gas accounting ([EIP-4762](https://eips.ethereum.org/EIPS/eip-4762)) ([#29338](https://github.com/ethereum/go-ethereum/pull/29338)).
- Optimise trie dirty tracking to reduce disk loads a bit ([#29731](https://github.com/ethereum/go-ethereum/pull/29731)).
- Ensure the [beacon chain roots system contract](https://eips.ethereum.org/EIPS/eip-4788) is deployed in dev mode ([#29655](https://github.com/ethereum/go-ethereum/pull/29655)).
- Add an additional snap sync check for data validity before inserting into the database ([#29485](https://github.com/ethereum/go-ethereum/pull/29485)).
- Improve the discovery protocol's node revalidation ([#29572](https://github.com/ethereum/go-ethereum/pull/29572), [#29864](https://github.com/ethereum/go-ethereum/pull/29864), [#29836](https://github.com/ethereum/go-ethereum/pull/29836)).
- Continue working towards pathdb support in archive mode ([#29530](https://github.com/ethereum/go-ethereum/pull/29530), [#29924](https://github.com/ethereum/go-ethereum/pull/29924)).

Shipped bugfixes:

- Fix a gas estimation regression that caused longer runtimes ([#29738](https://github.com/ethereum/go-ethereum/pull/29738)).
- Fix a potential crash in JSON logging for EVM blocktests ([#29795](https://github.com/ethereum/go-ethereum/pull/29795)).
- Fix utility commands to support post-merge opcodes ([#29799](https://github.com/ethereum/go-ethereum/pull/29799)).
- Fix a txpool synchronicity issue in simulated chains ([#29876](https://github.com/ethereum/go-ethereum/pull/29876)).
- Fix a iteration order when using a trie node iterator ([#27838](https://github.com/ethereum/go-ethereum/pull/27838)).
- Fix a TCP/UDP discovery port test in cmd/devp2p ([#29879](https://github.com/ethereum/go-ethereum/pull/29879)).
- Fix IPv6 endpoint determination ([#29801](https://github.com/ethereum/go-ethereum/pull/29801), [#29827](https://github.com/ethereum/go-ethereum/pull/29827)).

For a full rundown of the changes please consult the Geth [1.14.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/167?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.4) (2024-06-05)

Geth v1.14.4 in a usual maintenance release, but it does ship a 5-7% block import [speed improvement](https://github.com/ethereum/go-ethereum/pull/29519). Furthermore, v1.14.4 also finally includes an [Ether supply live tracer](https://github.com/ethereum/go-ethereum/pull/29347), that you can enable via `--vmtrace supply`. Also please note, the default value for miner tip enforcement was dropped from 1 gwei to 0.001 gwei (block producers can change this via `--miner.gasprice`).

Shipped features:

- Reduce the default required minimum miner tip from 1 gwei to 0.001 gwei to cater better for network conditions ([#29895](https://github.com/ethereum/go-ethereum/pull/29895)).
- Load trie nodes concurrently with trie updates, speeding up block import by 5-7% ([#29519](https://github.com/ethereum/go-ethereum/pull/29519), [#29768](https://github.com/ethereum/go-ethereum/pull/29768), [#29919](https://github.com/ethereum/go-ethereum/pull/29919)).
- Introduce an Ether supply tracker as a live chain tracer ([#29347](https://github.com/ethereum/go-ethereum/pull/29347)).
- Implement Verkle stateless gas accounting ([EIP-4762](https://eips.ethereum.org/EIPS/eip-4762)) ([#29338](https://github.com/ethereum/go-ethereum/pull/29338)).
- Optimise trie dirty tracking to reduce disk loads a bit ([#29731](https://github.com/ethereum/go-ethereum/pull/29731)).
- Ensure the [beacon chain roots system contract](https://eips.ethereum.org/EIPS/eip-4788) is deployed in dev mode ([#29655](https://github.com/ethereum/go-ethereum/pull/29655)).
- Add an additional snap sync check for data validity before inserting into the database ([#29485](https://github.com/ethereum/go-ethereum/pull/29485)).
- Improve the discovery protocol's node revalidation ([#29572](https://github.com/ethereum/go-ethereum/pull/29572), [#29864](https://github.com/ethereum/go-ethereum/pull/29864), [#29836](https://github.com/ethereum/go-ethereum/pull/29836)).
- Continue working towards pathdb support in archive mode ([#29530](https://github.com/ethereum/go-ethereum/pull/29530), [#29924](https://github.com/ethereum/go-ethereum/pull/29924)).

Shipped bugfixes:

- Fix a gas estimation regression that caused longer runtimes ([#29738](https://github.com/ethereum/go-ethereum/pull/29738)).
- Fix a potential crash in JSON logging for EVM blocktests ([#29795](https://github.com/ethereum/go-ethereum/pull/29795)).
- Fix utility commands to support post-merge opcodes ([#29799](https://github.com/ethereum/go-ethereum/pull/29799)).
- Fix a txpool synchronicity issue in simulated chains ([#29876](https://github.com/ethereum/go-ethereum/pull/29876)).
- Fix a iteration order when using a trie node iterator ([#27838](https://github.com/ethereum/go-ethereum/pull/27838)).
- Fix a TCP/UDP discovery port test in cmd/devp2p ([#29879](https://github.com/ethereum/go-ethereum/pull/29879)).
- Fix IPv6 endpoint determination ([#29801](https://github.com/ethereum/go-ethereum/pull/29801), [#29827](https://github.com/ethereum/go-ethereum/pull/29827)).

For a full rundown of the changes please consult the Geth [1.14.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/167?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.3) (2024-05-09)

We're issuing this (v1.14.3) release to finally publish v1.14 on the Ubuntu PPA. It is otherwise identical to [v1.14.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.2).

---

This is a maintenance release containing bug-fixes. In case you are wondering where v1.14.1 (and v1.14.2) went, let's just say, the continuous integration gods have not been good to us.

List of changes in detail:

#### Geth

- When using `geth --dev` with a custom genesis block, the genesis file must now set difficulty and terminal total difficulty to zero. ([#29579](https://github.com/ethereum/go-ethereum/pull/29579))
- For fork scheduling errors in `geth init`, fork timestamps will now be printed correctly. ([#29514](https://github.com/ethereum/go-ethereum/pull/29514))
- Certain aspects of state handling are now parallelized resulting in a 5-10% speedup for block processing. ([#29681](https://github.com/ethereum/go-ethereum/pull/29681))

#### RPC

- `eth_feeHistory` was changed to apply a limit on the number of requested percentiles ([#29644](https://github.com/ethereum/go-ethereum/pull/29644))
- `eth_createAccessList` now honors request cancellation and terminates background work ([#29686](https://github.com/ethereum/go-ethereum/pull/29686))
- `eth_estimateGas` takes tx blobs into account for low-balance scenarios ([#29703](https://github.com/ethereum/go-ethereum/pull/29703))

#### Tracing

- The live tracing interface has new hooks around EVM system calls ([#29355](https://github.com/ethereum/go-ethereum/pull/29355))
- flatCallTracer was fixed to return the correct error result when interrupted ([#29623](https://github.com/ethereum/go-ethereum/pull/29623))

#### Build

- This release is built with Go 1.22.3 ([#29725](https://github.com/ethereum/go-ethereum/pull/29725))
- We no longer provide deb packages for Ubunty 14.04 Trusty Tahr ([#29651](https://github.com/ethereum/go-ethereum/pull/29651), [#29649](https://github.com/ethereum/go-ethereum/pull/29649), [#29648](https://github.com/ethereum/go-ethereum/pull/29648), [#29647](https://github.com/ethereum/go-ethereum/pull/29647))
- CI builders have been updated to Ubuntu 24.04 Noble Numbat ([#29723](https://github.com/ethereum/go-ethereum/pull/29723))

For a full rundown of the changes please consult the Geth [1.14.1 release milestone](https://github.com/ethereum/go-ethereum/milestone/165?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.2) (2024-05-08)

This is a maintenance release containing bug-fixes. In case you are wondering where v1.14.1 went, let's just say, the continuous integration gods have not been good to us.

List of changes in detail:

#### Geth

- When using `geth --dev` with a custom genesis block, the genesis file must now set difficulty and terminal total difficulty to zero. ([#29579](https://github.com/ethereum/go-ethereum/pull/29579))
- For fork scheduling errors in `geth init`, fork timestamps will now be printed correctly. ([#29514](https://github.com/ethereum/go-ethereum/pull/29514))
- Certain aspects of state handling are now parallelized resulting in a 5-10% speedup for block processing. ([#29681](https://github.com/ethereum/go-ethereum/pull/29681))

#### RPC

- `eth_feeHistory` was changed to apply a limit on the number of requested percentiles ([#29644](https://github.com/ethereum/go-ethereum/pull/29644))
- `eth_createAccessList` now honors request cancellation and terminates background work ([#29686](https://github.com/ethereum/go-ethereum/pull/29686))
- `eth_estimateGas` takes tx blobs into account for low-balance scenarios ([#29703](https://github.com/ethereum/go-ethereum/pull/29703))

#### Tracing

- The live tracing interface has new hooks around EVM system calls ([#29355](https://github.com/ethereum/go-ethereum/pull/29355))
- flatCallTracer was fixed to return the correct error result when interrupted ([#29623](https://github.com/ethereum/go-ethereum/pull/29623))

#### Build

- This release is built with Go 1.22.3 ([#29725](https://github.com/ethereum/go-ethereum/pull/29725))
- We no longer provide deb packages for Ubunty 14.04 Trusty Tahr ([#29651](https://github.com/ethereum/go-ethereum/pull/29651), [#29649](https://github.com/ethereum/go-ethereum/pull/29649), [#29648](https://github.com/ethereum/go-ethereum/pull/29648), [#29647](https://github.com/ethereum/go-ethereum/pull/29647))
- CI builders have been updated to Ubuntu 24.04 Noble Numbat ([#29723](https://github.com/ethereum/go-ethereum/pull/29723))

For a full rundown of the changes please consult the Geth [1.14.1 release milestone](https://github.com/ethereum/go-ethereum/milestone/165?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.14.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.0) (2024-04-24)

Geth v1.14.0 (Asteria) is a major release with some juicy new features as well as some breaking changes. Please read through the release notes before updating to it. Whilst it should not adversely impact most users, there're always those rare occurrences.

### Highlights

- Geth v1.14.0 switches over the default state trie representation from `hash` mode to `path` mode (i.e. `--state.scheme` flipped form `hash` to `path`) ([#29108](https://github.com/ethereum/go-ethereum/pull/29108)). This change does not affect Geth instances with pre-existing databases, in the case of which Geth continues to use whatever the existing database's format is. If no previous database exists however, for full nodes, Geth will now default to `pathdb`. The main advantage is built-in, online historical state pruning; no more runaway state growth.
  - **If you want to force the old behaviour, you can run Geth with `--state.scheme=hash` for now.** That said, we will be dropping `hash` mode sooner rather than later, so we advise everyone running full nodes to gradually switch, if they haven't yet.
  - **Archive mode support is not yet finalised for `path` mode, so archive nodes will still run in `hash` mode.** Naturally, hash mode will not be dropped until a full `path` archive lands and people have enough time to switch to it.

- Geth v1.14.0 introduces a brand new *live-tracing* feature, where one or more transaction tracers might be injected into the block processing pipeline, ensuring that tracing and execution happen in lockstep ([#29189](https://github.com/ethereum/go-ethereum/pull/29189)). Since Go does not have a cross platform, OS native plugin infrastructure, adding live tracers needs to be done at the Geth source code level, and Geth itself subsequently rebuilt. That said, the advantage is that such tracers have full execution flexibility to do whatever they like and however they like. Please see the live-tracer [changelog](https://github.com/ethereum/go-ethereum/blob/master/core/tracing/CHANGELOG.md) and [docs](https://geth.ethereum.org/docs/developers/evm-tracing/live-tracing) for details.
  - **Live tracing runs in lockstep with block execution, one waiting for the other.** You should never run a live tracer on a validating node, or any other where latency is important. The recommended practice is to have tracers collect and export the bare minimum data needed and do any post-processing in external systems where latency is not relevant.
  - **The live tracer work required a number of breaking internal API changes.** If you had your own native tracers implemented before this change, the [changelog](https://github.com/ethereum/go-ethereum/blob/master/core/tracing/CHANGELOG.md) contains the necessary steps needed to update your old code for the new APIs.

- Geth v1.14.0 replaces the completely random transaction propagation paths (in the Ethereum P2P network) with pseudo-random ones, that ensure transactions from the same account follow the same path in the network (as long as no peer churn happens). The purpose is to allow a burst of transactions from the same account to ripple through the network on the same connections, minimising reordering. Whilst this doesn't provide any guarantees, nor does it replace the potential need for more complex routing logic, it should make nonce gaps significantly less likely, reducing the probability of dropped transactions during bursts ([#29034](https://github.com/ethereum/go-ethereum/pull/29034)).
  - **This change only applies to networking, so other clients are not required to follow suit.** That said, the network would behave more consistently overall if all clients implemented some similar logic (the exact routing algorithm is irrelevant, as long as it's stable in the accounts).

- Geth v1.14.0 drops support for running pre-merge networks ([#29169](https://github.com/ethereum/go-ethereum/pull/29169)). This does not mean that Geth will not be able to process or validate pre-merge blocks, rather that starting v1.14.0, Geth *must* have a consensus client drive it's chain selection. Geth drops the *forward-sync* mode of operation, where it imports blocks based on PoW (ethash) or PoA (clique) difficulties.
  - **Non-merged networks will need to stick to Geth v1.13.x until they transition to post-merge.** As Ethereum has transitioned towards a post-merge world, we cannot maintain a significant chunk of code that's been dead for 2 years now on mainnet.
  - **Post-merge networks must be marked so with the `terminalTotalDifficultyPassed: true` field in their `genesis.json`.** This requirement will be dropped (along with even caring at all about this field) in a few releases, but for now this field produces a clear error message for the user of what's wrong with their pre-merge network.

- Geth v1.14.0 stops automatically constructing the pending block ([#28623](https://github.com/ethereum/go-ethereum/pull/28623)). Performance wise there is a significant cost to creating a potential pending block, yet most node operators do not care about it. Not even validators need the pending block. This work also drops support for mining/signing a block "off-demand" (i.e. not via the engine API, rather by Geth itself), meaning Clique signing is also removed going forward.
  - **With Clique mining removed, Geth v1.14.0 cannot be a Clique signer any more.** This however goes hand-in-hand with the removal or pre-merge chain selection and synchronisation support described earlier.
  - **The pending block can still be constructed on demand for now.** Whenever an RPC method is called that is based on the pending block, Geth will generate it on the fly and cache it for 1 second for subsequent calls. The first such call will have a few hundred ms execution delay.

- **Geth v1.14.0 removes support for filtering pending logs.** This is following the previous change of creating the pending block only on demand. We see next to no value in monitoring the logs of a random subset of transactions that might (but probably mostly will not) end up in the next block. Power users who wish to monitor the transaction pool for MEV (or similar) purposes, should hook into Geth directly and extract the exact data they need, rather than rely on insufficient API endpoints.

- Geth v1.14.0 ships a *beacon chain* light client ([#28822](https://github.com/ethereum/go-ethereum/pull/28822), [#29308](https://github.com/ethereum/go-ethereum/pull/29308), [#29335](https://github.com/ethereum/go-ethereum/pull/29335), [#29532](https://github.com/ethereum/go-ethereum/pull/29532), [#29567](https://github.com/ethereum/go-ethereum/pull/29567)). It uses the REST API of beacon nodes and requires the `beacon / light_client namespace`, currently supported by Lodestar and Nimbus ([specs](https://ethereum.github.io/beacon-APIs/#/Beacon)).
  - **The beacon light client provides access to consensus layer data, not execution layer data.** This currently means that it can follow the beacon chain as a light client and extract the latest head and finalised execution layer block. That can be used to drive a full execution node without running an own beacon node. There is no support yet for a full execution light client.
  -  **Validation and staking is not possible with a beacon light client.** Since the sync committee signatures on each beacon head are canonised in the next block, the beacon light client can only follow the chain with one slot delay.
  - **The beacon light client exists both as a standalone executable and integrated into Geth.** The `blsync` executable can drive any execution layer node through the standard engine API or can just do a "test run" printing block numbers and hashes to the console. The integrated mode can do the same inside Geth. If you specify at least one suitable consensus layer REST API endpoint with the `--beacon.api` flag, then Geth will run without a beacon node connected through the engine API.

- Geth v1.14.0 switched to using Go v1.22 by default ([#28946](https://github.com/ethereum/go-ethereum/pull/28946)), which means we've dropped support for Go v1.20. Geth also started using of new features from Go v1.21 in our codebase, so building with Go v1.20 will probably error from now on. Just a heads up.

### Features

- Add the `geth db inspect-history` command to inspect pathdb state history ([#29267](https://github.com/ethereum/go-ethereum/pull/29267)).
- Enable the Cancun hard-fork when running Geth in dev mode ([#28829](https://github.com/ethereum/go-ethereum/pull/28829)).
- Use beacon chain finality as the marker to move old blocks into ancients ([#28683](https://github.com/ethereum/go-ethereum/pull/28683)).
- Drop large-contract (500MB+) deletion DoS protection from pathdb post Cancun ([#28940](https://github.com/ethereum/go-ethereum/pull/28940)).
- Switch Geth's USB library from `libusb` to `hid` for simpler hardware wallet support ([#28945](https://github.com/ethereum/go-ethereum/pull/28945), [#29175](https://github.com/ethereum/go-ethereum/pull/29175), [#29176](https://github.com/ethereum/go-ethereum/pull/29176)).
- Update the `prestateTrancer` to take into account 4844 blobs and their fees ([#29168](https://github.com/ethereum/go-ethereum/pull/29168)).
- Extend the `ethclient` and `gethclient` packages with support for 4844 blob fields ([#29198](https://github.com/ethereum/go-ethereum/pull/29198)).
- Detailed block execution metrics are now always reported, deprecating `--metrics.expensive` ([#29191](https://github.com/ethereum/go-ethereum/pull/29191)).
- Avoid a lot of memory allocations while shuffling 4844 blobs within Geth ([#29050](https://github.com/ethereum/go-ethereum/pull/29050)).
- Switch to using Go's native `log/slog` package instead of `golang/exp` ([#29302](https://github.com/ethereum/go-ethereum/pull/29302)).
- When computing state roots, apply state updates before deletes to avoid some trie lookups. ([#29201](https://github.com/ethereum/go-ethereum/pull/29201)).
- Track not only total peer count, but also how many are inbound or outbound ([#29424](https://github.com/ethereum/go-ethereum/pull/29424)).
- Add support for signing blob transactions ([#28976](https://github.com/ethereum/go-ethereum/pull/28976), [#29470](https://github.com/ethereum/go-ethereum/pull/29470)).
- Surface *max initcode exceeded* errors from the VM to outer callers ([#29354](https://github.com/ethereum/go-ethereum/pull/29354)).
- Update the EVM to reject contract creation on top of non-empty storage ([#28912](https://github.com/ethereum/go-ethereum/pull/28912)).
- Switch over the BLS implementation from `kilic` to `gnark` ([#29441](https://github.com/ethereum/go-ethereum/pull/29441)).
- Start emitting performance degradation warnings if `pebble` is overloaded ([#29478](https://github.com/ethereum/go-ethereum/pull/29478)).
- Add `eth_blobBaseFee` RPC method, extend `eth_feeHistory` with blobs ([#29140](https://github.com/ethereum/go-ethereum/pull/29140)).

### Bugfixes

- Shave an allocation off of pathdb trie writes caused by a bad initial buffer size ([#29106](https://github.com/ethereum/go-ethereum/pull/29106)).
- Fix a crash in dev mode using pathdb when rewinding more than 128 blocks ([#29107](https://github.com/ethereum/go-ethereum/pull/29107)).
- Fix some non-conformance issues in the engine API's error returns ([#29115](https://github.com/ethereum/go-ethereum/pull/29115)).
- Fix a too early db close in `geth dump` and `geth snapshot dump` ([#29100](https://github.com/ethereum/go-ethereum/pull/29100)).
- Fix the `devp2p` command to not ignore specified bootnodes ([#29091](https://github.com/ethereum/go-ethereum/pull/29091)).
- Fix the `prestateTracer` to return the correct nonce for contract creations ([#29099](https://github.com/ethereum/go-ethereum/pull/29099)).
- Fix blob fee formatting on RPC output from decimal to hexadecimal ([#29166](https://github.com/ethereum/go-ethereum/pull/29166)).
- Fix the blob pool tx acceptance error if it is already known vs underprices ([#29210](https://github.com/ethereum/go-ethereum/pull/29210)).
- Fix a few Windows file path joining issues in various go-ethereum utilities ([#29227](https://github.com/ethereum/go-ethereum/pull/29227), [#29479](https://github.com/ethereum/go-ethereum/pull/29479), [#29489](https://github.com/ethereum/go-ethereum/pull/29489)).
- Fix a data race when tracing a block via the JavaScript tracers ([#29238](https://github.com/ethereum/go-ethereum/pull/29238)).
- Fix a number of chain rewinding corner-cases after crashes ([#29196](https://github.com/ethereum/go-ethereum/pull/29196), [#29245](https://github.com/ethereum/go-ethereum/pull/29245)).
- Fix an ENR RLP decoding issue in the `devp2p` command ([#29257](https://github.com/ethereum/go-ethereum/pull/29257)).
- Fix withdrawal collection for simulated chains and dev mode ([#29344](https://github.com/ethereum/go-ethereum/pull/29344)).
- Fix a snap sync corner-case in `hashdb` mode ([#29341](https://github.com/ethereum/go-ethereum/pull/29341)).
- Fix an engine API field name mismatch for `GetClientVersion` ([#29351](https://github.com/ethereum/go-ethereum/pull/29351)).
- Fix a maximum unix socket path length check on macos ([#29385](https://github.com/ethereum/go-ethereum/pull/29385)).
- Fix an issue in the blob-pool that prevented Geth to start after a crash ([#29451](https://github.com/ethereum/go-ethereum/pull/29451)).
- Fix an issue that caused the JSON loggers to discard some log level ([#29471](https://github.com/ethereum/go-ethereum/pull/29471)).
- Fix the (now post-merge) genesis block difficulty in dev mode ([#29469](https://github.com/ethereum/go-ethereum/pull/29469)).
- Fix the precompile addresses of BLS ops (not live code yet) ([#29445](https://github.com/ethereum/go-ethereum/pull/29445)).
- Fix a data race that sometimes returned reorged receipts ([#29343](https://github.com/ethereum/go-ethereum/pull/29343)).
- Fix a snap-sync restart quirk that sometimes resulted in redownloading some data ([#29378](https://github.com/ethereum/go-ethereum/pull/29378)).
- Fix a snap-sync restart quirk that sometimes resulted in corrupting some data ([#29313](https://github.com/ethereum/go-ethereum/pull/29313)).
- Fix ancient header reader to hard cap at 2MB (network soft limit) ([#29534](https://github.com/ethereum/go-ethereum/pull/29534)).
- Fix the topic filtering to enforce some sanity nesting limits ([#29535](https://github.com/ethereum/go-ethereum/pull/29535)).

*We'd also like to shout out to a very high number of authors sending many nitpick PRs. Whilst we are not entirely sure about the intent behind these PRs (commit farming and all), we are nonetheless happy that the Geth codebase gets better.*

For a full rundown of the changes please consult the Geth [1.14.0 release milestone](https://github.com/ethereum/go-ethereum/milestone/164?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.15) (2024-04-17)

Geth v1.13.15 is a maintenance-release that contains some fixes mainly to avoid snapsync-related data-corruption.

We recommend all users to upgrade to v1.13.15 as soon as possible. 

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.14) (2024-02-27)

Geth v1.13.14 is a small maintenance release with a handful of polishes to the blob pool:

- Disallow blob transactions below the protocol minimum of 1 wei to enter the pool ([#29081](https://github.com/ethereum/go-ethereum/pull/29081)).
- Reduce the blob pool's max capacity to 2.5GB for the rollout. ([#29090](https://github.com/ethereum/go-ethereum/pull/29090)).
- Fix gas estimation for blob transactions ([#29085](https://github.com/ethereum/go-ethereum/pull/29085)).

***This release is NOT critical for the Cancun fork, but recommended to make Geth lighter in anticipation to unknown blob load.***

Other fixes:

- Support overriding the basefee during tracing ([#29051](https://github.com/ethereum/go-ethereum/pull/29051)).
- Fix call tracers missing top level logs in top-only mode ([#29068](https://github.com/ethereum/go-ethereum/pull/29068)).
- Support unlimited gas for `eth_createAccessList` if `--gascap=0` ([#28846](https://github.com/ethereum/go-ethereum/pull/28846)).

For a full rundown of the changes please consult the Geth [1.13.14 release milestone](https://github.com/ethereum/go-ethereum/milestone/162?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.13) (2024-02-21)

This is a minor release with fixes for several issues related to the upcoming Cancun mainnet fork. As such, it is recommended for all mainnet users.

Changes in this release:

- Block-building performance with blob transactions has been improved a lot. ([#29026](https://github.com/ethereum/go-ethereum/pull/29026), [#29008](https://github.com/ethereum/go-ethereum/pull/29008), [#29005](https://github.com/ethereum/go-ethereum/pull/29005))
- A corner case in the EVM related to out-of-order fork scheduling has been fixed. ([#29023](https://github.com/ethereum/go-ethereum/pull/29023))
- `eth_fillTransaction` has seen some bug fixes related to blob transactions as well. ([#28929](https://github.com/ethereum/go-ethereum/pull/28929), [#29037](https://github.com/ethereum/go-ethereum/pull/29037))
- A rare panic in the ethstats client related to chain reorgs is resolved. ([#29020](https://github.com/ethereum/go-ethereum/pull/29020))
- The blobpool database will now recover from disk corruption faults instead of crashing geth on startup. ([#29001](https://github.com/ethereum/go-ethereum/pull/29001))
- Geth now implements `getClientVersionV1` on the Engine API endpoint. ([#28915](https://github.com/ethereum/go-ethereum/pull/28915), [#28994](https://github.com/ethereum/go-ethereum/pull/28994))

Go API changes:

- `ethereum.CallMsg` now contains EIP-4844 related fields ([#28989](https://github.com/ethereum/go-ethereum/pull/28989))
- `core.GenesisAlloc` is now available from package `core/types`. We hope this change will reduce external dependencies on package `core`. ([#29003](https://github.com/ethereum/go-ethereum/pull/29003))

For a full rundown of the changes please consult the Geth [1.13.13 release milestone](https://github.com/ethereum/go-ethereum/milestone/161?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.12) (2024-02-09)

This release embeds the mainnet fork number for Cancun, scheduled to go live on 13th March, 2024 (unix `1710338135`). The specification can be read [here](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/cancun.md), and it contains the following changes:

* [EIP-1153: Transient storage opcodes](https://eips.ethereum.org/EIPS/eip-1153)
* [EIP-4788: Beacon block root in the EVM ](https://eips.ethereum.org/EIPS/eip-4788)
* [EIP-4844: Shard Blob Transactions](https://eips.ethereum.org/EIPS/eip-4844)
* [EIP-5656: MCOPY - Memory copying instruction](https://eips.ethereum.org/EIPS/eip-5656)
* [EIP-6780: SELFDESTRUCT only in same transaction](https://eips.ethereum.org/EIPS/eip-6780)
* [EIP-7516: BLOBBASEFEE opcode](https://eips.ethereum.org/EIPS/eip-7516)

To go along Cancun, we're providing refreshed Grafana dashboards:

- [Geth Cancun InfluxDB Dashboard](https://github.com/ethereum/go-ethereum/files/14211069/Geth-Cancun-InfluxDB.json)
- [Geth Cancun Prometheus Dashboard](https://github.com/ethereum/go-ethereum/files/14211070/Geth-Cancun-Prometheus.json)

Other than that, the following assorted fixes and features are included in this release:

- Initial implementation of the `era` format. The `era` format is meant to provide a cross-client archive format 
  for block data ([#26621](https://github.com/ethereum/go-ethereum/pull/26621), [#28959](https://github.com/ethereum/go-ethereum/pull/28959))
- Make rpc request limits configurable ([#28948](https://github.com/ethereum/go-ethereum/pull/28948))
- Fix memory-leak with blob transactions ([#28917](https://github.com/ethereum/go-ethereum/pull/28917))
- Stricter adherence to engine api spec ([#28882](https://github.com/ethereum/go-ethereum/pull/28882))
- Fix enforcement of minimum miner tip ([#28933](https://github.com/ethereum/go-ethereum/pull/28933))


For a full rundown of the changes please consult the Geth 1.13.12 [release milestone](https://github.com/ethereum/go-ethereum/milestone/160?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.11) (2024-01-24)

This release fixes a few bugs and enables the Cancun upgrade for the Sepolia and Holesky networks; Sepolia will upgrade on Jan 31, and Holesky on Feb 7, and naturally this is a required upgrade if you intend to follow either chain.  

- Enable Cancun on Sepolia and Holesky, plus Cancun-related changes ([#28834](https://github.com/ethereum/go-ethereum/pull/28834), [#28246](https://github.com/ethereum/go-ethereum/pull/28246), [#28230](https://github.com/ethereum/go-ethereum/pull/28230), [#28827](https://github.com/ethereum/go-ethereum/pull/28827))
- Support EIP-4844 transactions in API-methods ([#28786](https://github.com/ethereum/go-ethereum/pull/28786))
- Change how transaction indexing operates. As of 1.13.11, the behaviour of `eth_syncing` is slightly changed, so that it now
  does reports `true` until transaction indexing is finished. ([#28703](https://github.com/ethereum/go-ethereum/pull/28703))
- `rlpdump`: add `-pos` flag for displaying byte positions ([#28785](https://github.com/ethereum/go-ethereum/pull/28785))
- Fixes logging configuration ([#28801](https://github.com/ethereum/go-ethereum/pull/28801))

For a full rundown of the changes please consult the Geth 1.13.11 [release milestone](https://github.com/ethereum/go-ethereum/milestone/159?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.10) (2024-01-11)

**This release is equivalent to v1.13.9, just contains a version bump. The reason is that a bad commit was tagged on 1.13.9 originally and whilst it was untagged and fixed, some caches (Go's package manager (`go mod`)) managed to store the temporary bad version. As there is no way for us to flush the bad version out, it's cleaner to tag a next version instead. Apologies about the mess.**

---

This release fixes a few issues and **enables the Cancun upgrade for the Goerli network** at block timestamp 1705473120 ([#28719](https://github.com/ethereum/go-ethereum/pull/28719)) which is 6:32:am 17. Jan. 2024 UTC.

:warning: **If you are running Goerli, this is a required update!**

Apart from the Goerli configuration update, we have a few other changes.

- The 'simulated backend' in package `accounts/abi/backends` was rewritten. The improved version is available from the new package `ethclient/simulated`. A backwards-compatibility wrapper remains in the old location. ([#28202](https://github.com/ethereum/go-ethereum/pull/28202))
- Fix ABI-encoding of negative big.Int in topics ([#28764](https://github.com/ethereum/go-ethereum/pull/28764))
- In JSON logging output, the "error" level is now correctly emitted as `"error"`. ([#28774](https://github.com/ethereum/go-ethereum/pull/28774), [#28780](https://github.com/ethereum/go-ethereum/pull/28780))
- Fixed an issue with configuration of stdlib package `log` for consumers of the geth library ([#28747](https://github.com/ethereum/go-ethereum/pull/28747))
- `geth removedb` can now be run non-interactively ([#28725](https://github.com/ethereum/go-ethereum/pull/28725))
- We're building a package for ubuntu 23.10: mantic minotaur now ([#28728](https://github.com/ethereum/go-ethereum/pull/28728))

### Testing
- Add `currentExcessBlobGas` to the state tests for better coverage of state tests ([#28735](https://github.com/ethereum/go-ethereum/pull/28735))
- Fixed an issue in t8n regarding blob gas usage ([#28735](https://github.com/ethereum/go-ethereum/pull/28734))

For a full rundown of the changes please consult the Geth 1.13.9 [release milestone](https://github.com/ethereum/go-ethereum/milestone/157?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.9) (2024-01-10)

This release fixes a few issues and **enables the Cancun upgrade for the Goerli network** at block timestamp 1705473120 ([#28719](https://github.com/ethereum/go-ethereum/pull/28719)) which is 6:32:am 17. Jan. 2024 UTC.

:warning: **If you are running Goerli, this is a required update!**

Apart from the Goerli configuration update, we have a few other changes.

- The 'simulated backend' in package `accounts/abi/backends` was rewritten. The improved version is available from the new package `ethclient/simulated`. A backwards-compatibility wrapper remains in the old location. ([#28202](https://github.com/ethereum/go-ethereum/pull/28202))
- Fix ABI-encoding of negative big.Int in topics ([#28764](https://github.com/ethereum/go-ethereum/pull/28764))
- In JSON logging output, the "error" level is now correctly emitted as `"error"`. ([#28774](https://github.com/ethereum/go-ethereum/pull/28774), [#28780](https://github.com/ethereum/go-ethereum/pull/28780))
- Fixed an issue with configuration of stdlib package `log` for consumers of the geth library ([#28747](https://github.com/ethereum/go-ethereum/pull/28747))
- `geth removedb` can now be run non-interactively ([#28725](https://github.com/ethereum/go-ethereum/pull/28725))
- We're building a package for ubuntu 23.10: mantic minotaur now ([#28728](https://github.com/ethereum/go-ethereum/pull/28728))

### Testing
- Add `currentExcessBlobGas` to the state tests for better coverage of state tests ([#28735](https://github.com/ethereum/go-ethereum/pull/28735))
- Fixed an issue in t8n regarding blob gas usage ([#28735](https://github.com/ethereum/go-ethereum/pull/28734))

For a full rundown of the changes please consult the Geth 1.13.9 [release milestone](https://github.com/ethereum/go-ethereum/milestone/157?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.8) (2023-12-22)

This is a hotfix release for a regression which affects v1.13.6 and v1.13.7: if the node is shut down during sync, the node will refuse to start, with the error message `Fatal: Failed to register the Ethereum service: waiting for sync` ([#28718](https://github.com/ethereum/go-ethereum/pull/28718), [#28724](https://github.com/ethereum/go-ethereum/pull/28724)).

Please also see the release notes for [v1.13.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.6) and [v1.13.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.7)

For a full rundown of the changes please consult the Geth 1.13.8 [release milestone](https://github.com/ethereum/go-ethereum/milestone/156?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.7) (2023-12-19)

We're issuing this release to fix an issue with our build pipeline. There are also some other changes:

- The `eth_sendTransaction` RPC method now behaves more correctly for low-fee transactions. ([#27834](https://github.com/ethereum/go-ethereum/pull/27834))
- We have upgraded the golang.org/x/crypto module dependency. The Go team has issued a new version to fix a vulnerability in the ssh package. While we do not use this package, we have upgraded the dependency in order to stop dependabot warnings. ([#28702](https://github.com/ethereum/go-ethereum/pull/28702))

For a full rundown of the changes please consult the Geth 1.13.7 [release milestone](https://github.com/ethereum/go-ethereum/milestone/155?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.6) (2023-12-18)

Geth v1.13.6 is a scheduled maintenance release, but it also contains some changes which might affect node operators, concerning logging. 

### Gas estimation changes

The gas estimator was heavily reworked  ([#28600](https://github.com/ethereum/go-ethereum/pull/28600), [#28618](https://github.com/ethereum/go-ethereum/pull/28618)). The new version runs quite a bit faster (normally completing in 7-8 attempts rather than 18-20). However, the results have an error ratio of `1.5%`, and the estimation outcome won't always be deterministic. 

### Logging changes

In the absence of an 'official' Go logging framework, go-ethereum has, for a very long time, used a custom in-house logger.  However, just such an 'official' Go logging framework has now arrived, with the [`slog`](https://go.dev/blog/slog) package. 

As of `v1.13.6` , geth now uses `slog`, which will affect Geth users in different ways. 
    
Main changes are as follows:

* Verbosity level constants are changed to match slog constant values.  Internal translation is done to make this opaque to the user and backwards compatible with existing `--verbosity` and `--vmodule` options.
* `--log.backtraceat` and `--log.debug` are removed.
* Removes interface `log.Format` and the method `log.FormatFunc`,
* Unexports `TerminalHandler.TerminalFormat` formatting methods (renamed to `TerminalHandler.format`)
* Removes the notion of `log.Lazy` values
    
 The external-facing API is largely the same as the existing Geth logger.  Method signatures remain unchanged. A small semantic difference is that a `Handler` can only be set once per `Logger` and not changed dynamically.  This just means that a new logger must be instantiated every time the handler of the root logger is changed.
    
For users of the `github.com/ethereum/go-ethereum/log` package: If you were using this package for your own project, you will need to change the initialization. If you previously did

```go
    log.Root().SetHandler(log.LvlFilterHandler(log.LvlInfo, log.StreamHandler(os.Stderr, log.TerminalFormat(true))))
```

You now instead need to do

```go
    log.SetDefault(log.NewLogger(log.NewTerminalHandlerWithLevel(os.Stderr, log.LevelInfo, true)))
```

The lazy handler was useful in the old log package, since it could defer the evaluation of costly attributes until later in the log pipeline. Thus, if the logging was done at 'Trace', we could skip evaluation if logging only was set to 'Info'. With the move to slog, this way of deferring evaluation is no longer needed, since slog introduced 'Enabled'. Thus the caller can do the evaluate-or-not decision at the callsite, which is much more straight-forward than dealing with lazy reflect-based evaluation.

See more about reasoning here: https://github.com/ethereum/go-ethereum/issues/28558#issuecomment-1820606613

More detailed information can be found in the PRs [#28187](https://github.com/ethereum/go-ethereum/pull/28187), [#28621](https://github.com/ethereum/go-ethereum/pull/28621), [#28622](https://github.com/ethereum/go-ethereum/pull/28622) )

### Other changes

* Fixes a database corruption issue that could occur during state healing ([#28595](https://github.com/ethereum/go-ethereum/pull/28595))
* Fixes an issue where node liveness was not always verified correctly in discv5 ([#28686](https://github.com/ethereum/go-ethereum/pull/28686))
* Fix so state-dump can be performed after test execution ([#28650](https://github.com/ethereum/go-ethereum/pull/28650), [#28504](https://github.com/ethereum/go-ethereum/pull/28504))
* Fix a `ns/µs` mismatch in metrics for rpc-methods ([#28649](https://github.com/ethereum/go-ethereum/pull/28649))
* Fix a bug with wrong priority for `HTTPHost`, `WSHost` flags ([#28669](https://github.com/ethereum/go-ethereum/pull/28669))
* Fix type inconsistencies in tracer framework ([#28488](https://github.com/ethereum/go-ethereum/pull/28488))
* Add contextual information to errors returned by abi unpack ([#28529](https://github.com/ethereum/go-ethereum/pull/28529))
* Make `evm t8n` support custom tracers ([#28557](https://github.com/ethereum/go-ethereum/pull/28557))


For a full rundown of the changes please consult the Geth 1.13.6 [release milestone](https://github.com/ethereum/go-ethereum/milestone/154?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.5) (2023-11-14)

Geth v1.13.5 is a scheduled maintenance release fixing a potential data corruption in path scheme which could occur due to a power failure (i.e. entire OS / machine crash).

- Extend `ethclient` and the `simulated` backend to allow `eth_call` against specific block hashes ([#28084](https://github.com/ethereum/go-ethereum/pull/28084)).
- Downgrade annoying stale transaction propagation logs from warning to debug ([#28364](https://github.com/ethereum/go-ethereum/pull/28364)).
- Switch to the new KZG trusted setup parameters ([#28383](https://github.com/ethereum/go-ethereum/pull/28383)).
- Return an error on GraphQL if querying invalid block ranges ([#28393](https://github.com/ethereum/go-ethereum/pull/28393), [#28412](https://github.com/ethereum/go-ethereum/pull/28412)).
- Start publishing Apple Silicon pre-built binaries ([#28474](https://github.com/ethereum/go-ethereum/pull/28474), [#28475](https://github.com/ethereum/go-ethereum/pull/28475)).

And bugfixes: 

- Fix a number of corner-cases in path scheme state management ([#28198](https://github.com/ethereum/go-ethereum/pull/28198), [#28426](https://github.com/ethereum/go-ethereum/pull/28426), [#28483](https://github.com/ethereum/go-ethereum/pull/28483)).
- Fix an issue when allocating excessively large Pebble caches ([#28444](https://github.com/ethereum/go-ethereum/pull/28444)).
- Fix a potential snap sync issue with the path based storage ([#28327](https://github.com/ethereum/go-ethereum/pull/28327)).
- Fix ethclient to properly forwarding explicit 1559 gas caps ([#28462](https://github.com/ethereum/go-ethereum/pull/28462)).
- Fix gas estimation for 0 priced txs accessing the basefee ([#28470](https://github.com/ethereum/go-ethereum/pull/28470)).
- Fix an issue where resubscribing to events would hang ([#28359](https://github.com/ethereum/go-ethereum/pull/28359)).
- Fix ethstats transaction count report regressiob ([#28398](https://github.com/ethereum/go-ethereum/pull/28398)).
- Fix negative number encoding in ethclient/rpc ([#28358](https://github.com/ethereum/go-ethereum/pull/28358)).
- Fix GraphQL content type in the response ([#28417](https://github.com/ethereum/go-ethereum/pull/28417)).


For a full rundown of the changes please consult the Geth 1.13.5 [release milestone](https://github.com/ethereum/go-ethereum/milestone/153?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.4) (2023-10-17)

Geth v1.13.4 is a non-urgent hotfix release. The previous version of Geth (v1.13.3) introduced a warning log for bad transaction announcements, and on mainnet it generated too much logging noise due to a protocol violation in Erigon. To prevent overwhelming logging systems, Geth v1.13.4 lower the log to a more reasonable level until the bug in Erigon is fixed [#28356](https://github.com/ethereum/go-ethereum/pull/28356).

Apart from the above reason, the release contains:

- Fix a snap sync corner-case that could cause a hang by a maliciously constructed contract storage ([#28306](https://github.com/ethereum/go-ethereum/pull/28306)).
- Update various dependencies to unstick versions of Go libs ([#28329](https://github.com/ethereum/go-ethereum/pull/28329), [#28333](https://github.com/ethereum/go-ethereum/pull/28333), [#28334](https://github.com/ethereum/go-ethereum/pull/28334), [#28332](https://github.com/ethereum/go-ethereum/pull/28332), [#28336](https://github.com/ethereum/go-ethereum/pull/28336)).
- Enable Pebble database support on 32bit platforms and on OpenBSD too ([#28335](https://github.com/ethereum/go-ethereum/pull/28335)).
- Fix returning the correct code hash for eth_getProof with empty storage ([#28357](https://github.com/ethereum/go-ethereum/pull/28357)).
- Simplify trie range prover for some upcoming snap sync optimisations ([#28311](https://github.com/ethereum/go-ethereum/pull/28311)).
- Fix a timeout mechanism in the transaction fetcher ([#28220](https://github.com/ethereum/go-ethereum/pull/28220)).

For a full rundown of the changes please consult the Geth 1.13.4 [release milestone](https://github.com/ethereum/go-ethereum/milestone/152?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.3) (2023-10-12)

Geth v1.13.3 is a scheduled maintenance release with various small additions and an important Pebble database fix.

- Update Pebble to fix an occasional IO and CPU runaway, adding some debugging capabilities too ([#28224](https://github.com/ethereum/go-ethereum/pull/28224), [#28070](https://github.com/ethereum/go-ethereum/pull/28070)).
- Support full syncing to a specific hash without a beacon client via a `--synctarget` ([#28209](https://github.com/ethereum/go-ethereum/pull/28209)).
- Allow configuring websocket message limits via the Go RPC client ([#27801](https://github.com/ethereum/go-ethereum/pull/27801)).
- Drop support for `eth/66` (Cancun will require `eth/68` anyway) ([#28239](https://github.com/ethereum/go-ethereum/pull/28239)).
- Lower `snap` missing `eth` protocol warning to debug level ([#28249](https://github.com/ethereum/go-ethereum/pull/28249)).
- Enforce transaction metadata announcements in `eth/68` ([#28261](https://github.com/ethereum/go-ethereum/pull/28261)).

Features related to the Cancun hardfork:

- Implement the `BLOBFEE` opcode for the upcoming Cancun hard fork ([#28098](https://github.com/ethereum/go-ethereum/pull/28098)).
- Enable blob transaction propagation and mining in Cancun networks ([#28243](https://github.com/ethereum/go-ethereum/pull/28243)).
- Start throttling transaction retrievals to prepare for blobs in Cancun ([#28304](https://github.com/ethereum/go-ethereum/pull/28304)).

For a full rundown of the changes please consult the Geth 1.13.3 [release milestone](https://github.com/ethereum/go-ethereum/milestone/151?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.2) (2023-09-28)

Geth v1.13.2 is a bugfix release for the 1.13 family as well as contains the Holesky testnet relaunch.

*Note, if you have previously ran Geth with the old Holesky testnet configs, the new version will probably fail to start with a genesis hash mismatch error. You will need to manually delete your `holesky/chaindata` folder and restart. Geth did not implement special code for cleaning up the failed launch of the testnet.*

- Fix various pathdb corruption corner-cases during snap sync node restart ([#28171](https://github.com/ethereum/go-ethereum/pull/28171), [#28163](https://github.com/ethereum/go-ethereum/pull/28163)).
- Reconfigure the Holesky testnet with an updated genesis ([#28191](https://github.com/ethereum/go-ethereum/pull/28191), [#28192](https://github.com/ethereum/go-ethereum/pull/28192), [#28193](https://github.com/ethereum/go-ethereum/pull/28193)).
- Remove the rollback mechanism from snap sync, unneeded post-merge ([#28147](https://github.com/ethereum/go-ethereum/pull/28147)).
- Make the `block` parameter in `eth_call` optional, defaulting to `latest` ([#28165](https://github.com/ethereum/go-ethereum/pull/28165)).
- Forget transactions previously marked underpriced after 5 minutes ([#28097](https://github.com/ethereum/go-ethereum/pull/28097)).
- Fix JSON marshalling issue from `ethclient` retrieving block receipts ([#28087](https://github.com/ethereum/go-ethereum/pull/28087)).
- Fix `--bootnodes` flag if the list is also configured in the toml file ([#28095](https://github.com/ethereum/go-ethereum/pull/28095)).

For a full rundown of the changes please consult the Geth 1.13.2 [release milestone](https://github.com/ethereum/go-ethereum/milestone/150?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.1) (2023-09-17)

Geth v1.13.1 is a hotfix release for v1.13.0.

It fixes the following issues:

- Fix the active fork detection on the engine API, causing the signer to create invalid blocks ([#28135](https://github.com/ethereum/go-ethereum/pull/28135)).
- Fix a db corruption in `path` scheme caused by a weirdly restarted snap sync ([#28124](https://github.com/ethereum/go-ethereum/pull/28124), [#28126](https://github.com/ethereum/go-ethereum/pull/28126)).
- Fix `geth db inspect` command running against old `hash` scheme databases ([#28108](https://github.com/ethereum/go-ethereum/pull/28108)).
- Fix an effective gas price calculation regression on the RPC APIs ([#28130](https://github.com/ethereum/go-ethereum/pull/28130)).

Apart from the fixes, v1.13.1 introduces support for configuring Geth via environmental variables ([#28103](https://github.com/ethereum/go-ethereum/pull/28103), [#28119](https://github.com/ethereum/go-ethereum/pull/28119))!

For a full rundown of the changes please consult the Geth 1.13.1 [release milestone](https://github.com/ethereum/go-ethereum/milestone/149?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.13.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.0) (2023-09-12)

Geth v1.13.0 is a major milestone in the lifetime of Geth, bits and bobs being in development for around 6 years now. Since a release note cannot do it justice, please see our [Geth v1.13.0 release blog post](https://blog.ethereum.org/2023/09/12/geth-v1-13-0).

Still, just to quickly recap, Geth v1.13.0 finally ships a new database model which supports proper, full pruning of historical states; meaning you will never need to take your node offline again to resync or to manually prune. The new database model is optional for now (you need to enable it via `--state.scheme=path`) and does require resyncing the state, since we need to store it completely different (you can keep your ancients, no need to resync the chain too).

The path database will become the default eventually, but for safety reasons, we're keeping it opt-in for the moment. The old database model is not going away soon, though long term - unless there's something fundamentally wrong with the path db -  it will. As for archive node users, we're working on a new model there too, but it does need a bit more work on top, so that's for another release.

***The all important disclaimer: Geth's new path-based storage is considered stable and production ready, but was obviously not battle tested yet outside of the team. Everyone is welcome to use it, but if you have significant risks if your node crashes or goes out of consensus, you might want to wait a bit to see if anyone with a lower risk profile hits any issues.***

---

Apart from the pruning work, the release contains:

- Built in support for the Holešky (Holešovice) testnet ([#28007](https://github.com/ethereum/go-ethereum/pull/28007)).
- Index transactions even if no blocks are received ([#27847](https://github.com/ethereum/go-ethereum/pull/27847)).
- Expose Geth version metadata into the metrics ([#24877](https://github.com/ethereum/go-ethereum/pull/24877)).
- Optimise `eth_estimateGas` to do fewer runs ([#27710](https://github.com/ethereum/go-ethereum/pull/27710)).
- Add `eth_getBlockReceipts` RPC API call ([#27702](https://github.com/ethereum/go-ethereum/pull/27702)).
- Support unpacking Solidity panic events ([#27868](https://github.com/ethereum/go-ethereum/pull/27868)).
- Reject GraphQL block queries where both number and hash is specified ([#27876](https://github.com/ethereum/go-ethereum/pull/27876)).
- Increase batch limits for RPC calls on the authenticated endpoint ([#27924](https://github.com/ethereum/go-ethereum/pull/27924)).
- Optimise logging library to avoid expensive call stack lookups ([#28069](https://github.com/ethereum/go-ethereum/pull/28069)).

And bugfixes:

- Fix forkid computation for genesis-merged non-zero timestamp networks ([#27895](https://github.com/ethereum/go-ethereum/pull/27895), [#28034](https://github.com/ethereum/go-ethereum/pull/28034)).
- Fix a potential data race in the websocket ping/pong mechanism ([#27733](https://github.com/ethereum/go-ethereum/pull/27733)).
- Fix js tracers to return the gas price in base 16, not base 10 ([#27903](https://github.com/ethereum/go-ethereum/pull/27903)).
- Fix finalized block number in dev (`--dev`) mode ([#27886](https://github.com/ethereum/go-ethereum/pull/27886)).

For a full rundown of the changes please consult the Geth 1.13.0 [release milestone](https://github.com/ethereum/go-ethereum/milestone/148?closed=1), though do note that the state scheme changes and pruner have been gradually merged over the past year so are not explicitly tagged in this milestone.

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.12.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.12.2) (2023-08-12)

Hot on the heels of v1.12.1 comes our next release, fixing some regressions reported by the community.

Here are the changes:

- A crash related to leveldb metrics is resolved ([#27904](https://github.com/ethereum/go-ethereum/pull/27904))
- Metrics names used by the blobpool have been changed to be compatible with Prometheus ([#27901](https://github.com/ethereum/go-ethereum/pull/27901))
- The c-kzg-4844 and blst libraries have been updated, hopefully fixing some build issues ([#27890](https://github.com/ethereum/go-ethereum/pull/27890), [#27907](https://github.com/ethereum/go-ethereum/pull/27907), [#27910](https://github.com/ethereum/go-ethereum/pull/27910))
- We have also adapted go-ethereum to the latest changes in the 'slices' package provided by the golang.org/x/exp module. The Go authors decided to push an incompatible update, but didn't create a new release of that module, causing build issues when consumers mix-and-match dependency versions. ([#27909](https://github.com/ethereum/go-ethereum/pull/27909))

For a full rundown of the changes please consult the Geth 1.12.2 [release milestone](https://github.com/ethereum/go-ethereum/milestone/147?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.12.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.12.1) (2023-08-10)

Geth v1.12.1 is a maintenance release, albeit a rather large one, since we haven't put out a version since May.
This release is a recommended upgrade for all users and contains security-related fixes.

Here's the list of changes:

#### Cancun fork

Development for the upcoming Cancun hard fork has been a focus in this release cycle. Do note however, that Geth v1.12.1 is **not yet ready** for Cancun.

- For EIP-4844 'Shard Blob Transactions', this release contains basic type definitions and state transition logic ([#27356](https://github.com/ethereum/go-ethereum/pull/27356), [#27382](https://github.com/ethereum/go-ethereum/pull/27382), [#27392](https://github.com/ethereum/go-ethereum/pull/27392), [#27393](https://github.com/ethereum/go-ethereum/pull/27393), [#27470](https://github.com/ethereum/go-ethereum/pull/27470), [#27721](https://github.com/ethereum/go-ethereum/pull/27721), [#27736](https://github.com/ethereum/go-ethereum/pull/27736), [#27743](https://github.com/ethereum/go-ethereum/pull/27743), [#27767](https://github.com/ethereum/go-ethereum/pull/27767), [#27789](https://github.com/ethereum/go-ethereum/pull/27789), [#27791](https://github.com/ethereum/go-ethereum/pull/27791), [#27793](https://github.com/ethereum/go-ethereum/pull/27793), [#27796](https://github.com/ethereum/go-ethereum/pull/27796), [#27797](https://github.com/ethereum/go-ethereum/pull/27797), [#27825](https://github.com/ethereum/go-ethereum/pull/27825), [#27874](https://github.com/ethereum/go-ethereum/pull/27874))
- We have also implemented an entirely new mempool -- the blobpool -- for EIP-4844 transactions. The new pool is backed by an on-disk database and has an overall simpler design than the txpool we use for regular transactions. The blobpool is not yet used by p2p networking and is still being tested in the Cancun devnets. ([#26940](https://github.com/ethereum/go-ethereum/pull/26940), [#27429](https://github.com/ethereum/go-ethereum/pull/27429), [#27463](https://github.com/ethereum/go-ethereum/pull/27463), [#27481](https://github.com/ethereum/go-ethereum/pull/27481), [#27790](https://github.com/ethereum/go-ethereum/pull/27790), [#27822](https://github.com/ethereum/go-ethereum/pull/27822))
- EIP-6780: 'SELFDESTRUCT only in same transaction' ([#27189](https://github.com/ethereum/go-ethereum/pull/27189))
- EIP-5656: 'MCOPY instruction' ([#26181](https://github.com/ethereum/go-ethereum/pull/26181))
- EIP-1153: 'Transient storage opcodes' was already implemented and is enabled for Cancun ([#27663](https://github.com/ethereum/go-ethereum/pull/27663), [#27613](https://github.com/ethereum/go-ethereum/pull/27613))

#### Geth command changes

- The Rinkeby testnet is no longer supported in Geth ([#27406](https://github.com/ethereum/go-ethereum/pull/27406))
- `geth --dev` now simulates a PoS-based chain ([#27327](https://github.com/ethereum/go-ethereum/pull/27327))
- `evm blocktest` can now output structured logs ([#27396](https://github.com/ethereum/go-ethereum/pull/27396))
- Geth will now configure GOMAXPROCS based on CPU quota settings. This should improve efficiency when running in Docker containers with a CPU core limit applied. ([#27506](https://github.com/ethereum/go-ethereum/pull/27506), [#27814](https://github.com/ethereum/go-ethereum/pull/27814))
- An IPv6 listening address for can now be configured for HTTP/WS ([#27628](https://github.com/ethereum/go-ethereum/pull/27628)) ([#27635](https://github.com/ethereum/go-ethereum/pull/27635))

#### RPC/GraphQL API changes

- JSON transactions now have a `yParity` fields, as mandated by the RPC API spec ([#27744](https://github.com/ethereum/go-ethereum/pull/27744), [#27882](https://github.com/ethereum/go-ethereum/pull/27882))
- Legacy transactions now have a `chainID` field in RPC responses, like all other transaction types ([#27452](https://github.com/ethereum/go-ethereum/pull/27452))
- Block headers returned by RPC no longer report a non-standard `size` field ([#27347](https://github.com/ethereum/go-ethereum/pull/27347))
- `eth_estimateGas` now supports state overrides like `eth_call` ([#27845](https://github.com/ethereum/go-ethereum/pull/27845))
- `eth_estimateGas` now handles internal chain reorgs more correctly ([#27505](https://github.com/ethereum/go-ethereum/pull/27505))
- `eth_getProof` is slight more efficient, and will now return a response in the canonical encoding even for off-spec input parameters ([#27309](https://github.com/ethereum/go-ethereum/pull/27309), [#27310](https://github.com/ethereum/go-ethereum/pull/27310))
- `eth_getTransactionReceipt` now returns `null` when the transaction is not available. It used return an error in that case. ([#27712](https://github.com/ethereum/go-ethereum/pull/27712))
- `debug_storageRangeAt` now takes a block hash or number as parameter ([#27328](https://github.com/ethereum/go-ethereum/pull/27328))
- The new `debug_getTrieFlushInterval` method reports the internal state saving interval ([#27303](https://github.com/ethereum/go-ethereum/pull/27303))
- A crash in the prestate tracer is resolved ([#27691](https://github.com/ethereum/go-ethereum/pull/27691))
- Structured EVM logs returned by tracing now contain the `returnData` ([#27704](https://github.com/ethereum/go-ethereum/pull/27704))
- GraphQL now supports withdrawals (EIP-4895) ([#27072](https://github.com/ethereum/go-ethereum/pull/27072))

#### Go library changes

- The RPC server now enforces limits on batch requests and responses. This is a potentially breaking change. 
  If you use batch requests with geth, and also use the go-ethereum RPC client library, **we strongly recommend updating your go-ethereum library dependency** as well. The new client version handles invalid batch responses way better than before. ([#26681](https://github.com/ethereum/go-ethereum/pull/26681))
- The RPC client has multiple new ways to test whether the transport supports real time subscriptions ([#25942](https://github.com/ethereum/go-ethereum/pull/25942))
- fsync is now enabled for pebble database writes ([#27615](https://github.com/ethereum/go-ethereum/pull/27615), [#27522](https://github.com/ethereum/go-ethereum/pull/27522))
- Function calls timed by metrics will now run even if metrics are disabled ([#27724](https://github.com/ethereum/go-ethereum/pull/27724), [#27723](https://github.com/ethereum/go-ethereum/pull/27723))
- `Node.Attach` no longer returns an error. This is a breaking Go API change. ([#27450](https://github.com/ethereum/go-ethereum/pull/27450))
- The keystore has improved verification of keys loaded from disk ([#27432](https://github.com/ethereum/go-ethereum/pull/27432))
- Per-level metrics are now available for LevelDB ([#27643](https://github.com/ethereum/go-ethereum/pull/27643))

#### Core

- All block creation activity is now paused while the node is syncing ([#27218](https://github.com/ethereum/go-ethereum/pull/27218))
- Two minor bugs in the transaction pool are resolved in this release ([#27404](https://github.com/ethereum/go-ethereum/pull/27404), [#27479](https://github.com/ethereum/go-ethereum/pull/27479))
- Geth no longer uses a 'clean cache file' to persist internal caches across restarts. While persistent cache added a small performance boost right after startup, it could cause obscure issues in certain restart scenarios. ([#27525](https://github.com/ethereum/go-ethereum/pull/27525))
- A large portion of the new Path-Based State Storage scheme has been implemented. While it isn't active yet, we are planning to make this new storage method available in the next release. ([#25963](https://github.com/ethereum/go-ethereum/pull/25963), [#27323](https://github.com/ethereum/go-ethereum/pull/27323), [#27349](https://github.com/ethereum/go-ethereum/pull/27349), [#27428](https://github.com/ethereum/go-ethereum/pull/27428), [#27687](https://github.com/ethereum/go-ethereum/pull/27687), [#27753](https://github.com/ethereum/go-ethereum/pull/27753), [#27815](https://github.com/ethereum/go-ethereum/pull/27815))
- As part of testing the new storage scheme, some inconsistencies in selfdestruct handling were discovered by fuzz tests and had to be fixed ([#27376](https://github.com/ethereum/go-ethereum/pull/27376), [#27339](https://github.com/ethereum/go-ethereum/pull/27339))
- We have also made significant progress on the integration of Verkle Trees, which required changes to internal state-handling APIs ([#27000](https://github.com/ethereum/go-ethereum/pull/27000), [#27209](https://github.com/ethereum/go-ethereum/pull/27209), [#27464](https://github.com/ethereum/go-ethereum/pull/27464), [#27476](https://github.com/ethereum/go-ethereum/pull/27476), [#27544](https://github.com/ethereum/go-ethereum/pull/27544), [#27853](https://github.com/ethereum/go-ethereum/pull/27853))

#### Networking

- A serious memory leak related to database writes in snap sync is fixed in this release ([#27842](https://github.com/ethereum/go-ethereum/pull/27842))
- Large transactions (> 4kB) are no longer broadcasted to peers. This resolves a potential network congestion issue ([#27618](https://github.com/ethereum/go-ethereum/pull/27618))
- The p2p networking layer has learned to announce alternate ports returned by UPnP/NAT-PMP ([#26359](https://github.com/ethereum/go-ethereum/pull/26359))
- The p2p server now properly tracks all peer goroutines ([#27887](https://github.com/ethereum/go-ethereum/pull/27887))
- Networking initialization now really disables all discovery when `--nodiscover` is used ([#27518](https://github.com/ethereum/go-ethereum/pull/27518))
- Obsolete parts of the LES protocol implementation, which is currently non-functional, have been removed ([#27737](https://github.com/ethereum/go-ethereum/pull/27737))
- Discovery bootstrap nodes will now be filtered by the netrestrict setting, like all other nodes ([#27701](https://github.com/ethereum/go-ethereum/pull/27701))
- We now provide additional metrics around p2p dialing, making it possible to measure the efficieny of peer discovery ([#27621](https://github.com/ethereum/go-ethereum/pull/27621))
- The downloader no longer accumulates goroutines/memory while processing reorgs ([#27397](https://github.com/ethereum/go-ethereum/pull/27397))
- A very rare crash related to peer connection tracking is resolved ([#27665](https://github.com/ethereum/go-ethereum/pull/27665))
- It is now possible to configure certain discovery internals for experimentation ([#27387](https://github.com/ethereum/go-ethereum/pull/27387))

#### Build

- This release is built with Go 1.20.7 ([#27835](https://github.com/ethereum/go-ethereum/pull/27835), [#27708](https://github.com/ethereum/go-ethereum/pull/27708))
- On UNIX-like OSes, package rpc no longer uses cgo ([#27447](https://github.com/ethereum/go-ethereum/pull/27447))
- Building go-ethereum no longer fails when .DS_Store files exist in unexpected locations ([#27521](https://github.com/ethereum/go-ethereum/pull/27521))
- On macOS, a build warning related to libusb is resolved ([#27698](https://github.com/ethereum/go-ethereum/pull/27698))
- An obscure build issue related to the NDEBUG C macro is resolved ([#27550](https://github.com/ethereum/go-ethereum/pull/27550))

For a full rundown of the changes please consult the Geth 1.12.1 [release milestone](https://github.com/ethereum/go-ethereum/milestone/146?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.12.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.12.0) (2023-05-25)

**Geth v1.12.0 is a potentially breaking change, hence it was deemed to deserve  version bump, to `1.12`.**

The v1.12 release family drops support for proof-of-work, and thus can not be used any more on PoW-based private chains, or as an upstream library for projects depending on `ethash` PoW (#27178, #27147).

In our GraphQL API, a breaking change is that all numeric values are now encoded as hex strings (#26894). The internal GraphQL UI was updated to version 2.0. (#27294).

Regarding our move from `leveldb` to `pebble`, Geth now defaults to use Pebble as a backend if no existing database is found (#27136). If a previous LevelDB database exists Geth will keep using that, and if you must have LevelDB for some compatibility reasons, you can force it in Geth with the `--db.engine=leveldb` flag.

We have made progress on "EIP-4844: Shard Blob Transactions" (#27257, #27256, #27155, #27049), beacon light sync (#27292), and path-based state storage (#27176, #26813) but neither is finished as of yet.

Other improvements: 
  - Add block overrides to eth_call (#26414)
  - Make batched state-test execution possible (#27318)

Assorted bugfixes: 
  - Do not ignore `null` address while iterative dump (#27320)   
  - Fix flatCallTracer crasher (#27304)
  - Prevent pebble shutdown-panic (#27238)
  - Make websocket use default "HTTP_PROXY" by default (#27264)
  - Make `eth_estimateGas` use `latest` block by default (#24363)
  - Add `txHash` field on txTraceResult (#27183)
  - Fix crash on querying finalized block (#27162)

For a full rundown of the changes please consult the Geth [1.12.0 release milestone](https://github.com/ethereum/go-ethereum/milestone/145?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).


## [v1.11.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.6) (2023-04-20)

Geth v1.11.6 is a maintenance release, fixing some minor issues and adding some log management features.

#### Log management 

Log rotation has landed in `geth`, via ([#26843](https://github.com/ethereum/go-ethereum/pull/26843)). Log rotation can be activated using the flag `--log.rotate`. Additional parameters that can be given are:

  - `--log.maxsize` to set maximum size before files are rotated,
  - `--log.maxbackups` to set how many files are retailed,
  - `--log.maxage` to configure max age of rotated files,
  - `--log.compress` whether to compress rotated files

The location the logfile(s) can be configured as previously, using the `--log.logfile` parameter. 

A new log output format, [`logfmt`](https://brandur.org/logfmt) was added ([#27001](https://github.com/ethereum/go-ethereum/pull/27001), [#26970](https://github.com/ethereum/go-ethereum/pull/26970)). It can be enabled using `--log.format`, which currently supports the options `json`, `logfmt` or `terminal`. (Thus, the `--log.json` option is now deprecated). 

And finally, the flag `--vmodule` was renamed to `--log.vmodule` ([#27071](https://github.com/ethereum/go-ethereum/pull/27071)). 

#### Assorted

- New sepolia bootnodes managed by EF devops are added ([#27099](https://github.com/ethereum/go-ethereum/pull/27099))
- Converting the codebase to use `atomic` types ([#27068](https://github.com/ethereum/go-ethereum/pull/27068), [#27031](https://github.com/ethereum/go-ethereum/pull/27031), [#27030](https://github.com/ethereum/go-ethereum/pull/27030), [#27013](https://github.com/ethereum/go-ethereum/pull/27013),[#27014](https://github.com/ethereum/go-ethereum/pull/27014), [#27011](https://github.com/ethereum/go-ethereum/pull/27011), [#26992](https://github.com/ethereum/go-ethereum/pull/26992), [#26993](https://github.com/ethereum/go-ethereum/pull/26993), [#26951](https://github.com/ethereum/go-ethereum/pull/26951), [#26935](https://github.com/ethereum/go-ethereum/pull/26935), [#27121](https://github.com/ethereum/go-ethereum/pull/27121))
- Bugfixes to make tracers more correct avoid crashes ([#27029](https://github.com/ethereum/go-ethereum/pull/27029), [#26848](https://github.com/ethereum/go-ethereum/pull/26848))
- Fix race-conditions in `graphql` ([#26965](https://github.com/ethereum/go-ethereum/pull/26965))
- New CPU counter-typed metrics ([#26796](https://github.com/ethereum/go-ethereum/pull/26796))

For a full rundown of the changes please consult the Geth [1.11.6 release milestone](https://github.com/ethereum/go-ethereum/milestone/144?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.5) (2023-03-21)

Geth v1.11.5 **enables the Shanghai upgrade on Wednesday, April 12, 2023 10:27:35 PM UTC**

The Shanghai upgrade contains the following EIPs:

- [EIP-3651: Warm COINBASE](https://eips.ethereum.org/EIPS/eip-3651)
- [EIP-3855: PUSH0 instruction](https://eips.ethereum.org/EIPS/eip-3855)
- [EIP-3860: Limit and meter initcode](https://eips.ethereum.org/EIPS/eip-3860)
- [EIP-4895: Beacon chain push withdrawals as operations](https://eips.ethereum.org/EIPS/eip-4895)
- [EIP-6049: Deprecate SELFDESTRUCT](https://eips.ethereum.org/EIPS/eip-6049)


:warning: In order to be able to follow the chain through the upgrade on Wednesday, April 12, 2023 10:27:35 PM UTC, this upgrade is **required**. :warning: 

#### Notable changes:

- Enable Shanghai at `1681338455` unix time ([#26908](https://github.com/ethereum/go-ethereum/pull/26908))
- Improve error message on bad blocks ([#26870](https://github.com/ethereum/go-ethereum/pull/26870))

#### Minor changes and bugfixes 
- Preparatory changes for path-based storage ([#26763](https://github.com/ethereum/go-ethereum/pull/26763))
- Changes needed use overlay protocols in discv5 ([#26699](https://github.com/ethereum/go-ethereum/pull/26699))
- Add support for encoding uint256.Int in RLP ([#26898](https://github.com/ethereum/go-ethereum/pull/26898))
- Fixed a bug where local transaction would be rejected with `-32000, future transaction tries to replace pending` when send out of order ([#26930](https://github.com/ethereum/go-ethereum/pull/26930))
- Reject keywords `safe` or `pending` over RPC pre merge ([#26862](https://github.com/ethereum/go-ethereum/pull/26862)
- Fix file permissions on `admin_exportChain` ([#26912](https://github.com/ethereum/go-ethereum/pull/26912))

For a full rundown of the changes please consult the Geth [1.11.5 release milestone](https://github.com/ethereum/go-ethereum/milestone/143?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.4) (2023-03-10)

This is a security release of Geth, improving resilience of the transaction pool against certain kinds of DoS attacks. These attacks have recently been observed in testnets.

Notable changes:

- TxPool validation rules have been tightened to defend against certain DoS attacks. The new checks are based on research by @dwn1998, @wangyibo0308, @ZhouYuxuan97, @tristartom, @fs3l. Thank you for your report and cooperation! ([#26648](https://github.com/ethereum/go-ethereum/pull/26648))
- EIP-712 signing in Clef now recognizes all primitive types of Solidity. ([#26770](https://github.com/ethereum/go-ethereum/pull/26770))
- EF bootstrap nodes on MS Azure have been removed because they will be decommissioned soon. ([#26828](https://github.com/ethereum/go-ethereum/pull/26828))
- The `core.Message` type has been changed from interface to struct and `types.Message` (in core/types) has been removed. This is a breaking API change in core/types. We believe the removal should not cause additional disruption for downstream projects because `types.Messages` had no meaningful use outside of package core. ([#25977](https://github.com/ethereum/go-ethereum/pull/25977))
- `core.BlockGen` has a new `Timestamp` method. ([#26844](https://github.com/ethereum/go-ethereum/pull/26844))

For a full rundown of the changes please consult the Geth [1.11.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/142?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.3) (2023-03-07)

This is a minor release, fixing a couple of issues and enabling the Shanghai upgrade on Goerli at block timestamp 1678832736 ([#26795](https://github.com/ethereum/go-ethereum/pull/26795)).

:warning: **If you are running Goerli, this is a required update.**

### Minimum Go version

In accordance with our policy to only support the newest two versions of Go, we have changed the minimum required compiler version to Go 1.19 ([#26803](https://github.com/ethereum/go-ethereum/pull/26803)).

### Pebble

In v1.11.0 we released Pebble support (guarded by the `--db.engine=pebble` flag). Thanks to everyone testing it! We found and fixed a few issues:

- You can now set more than 4 GB in `--cache` when using Pebble. ([#26776](https://github.com/ethereum/go-ethereum/pull/26776))
- Range compaction now works correctly for Pebble. ([#26771](https://github.com/ethereum/go-ethereum/pull/26771))
- Pebble support is disabled on OpenBSD to resolve a compilation error. ([#26801](https://github.com/ethereum/go-ethereum/pull/26801))

### RPC changes

- Add support for Parity-style flat traces with the new built-in `flatCallTracer`. ([#26377](https://github.com/ethereum/go-ethereum/pull/26377))
- The `callTracer` now reports a `null` address for failed contract creation operations. ([#26779](https://github.com/ethereum/go-ethereum/pull/26779))
- `head` and `difficulty` have been removed in `admin_peerInfo` responses. ([#26804](https://github.com/ethereum/go-ethereum/pull/26804))

### Other fixes

- `types.Receipt` now contains the `EffectiveGasPrice` of the transaction, so you can get the true gas price using the `TransactionReceipt` method of ethclient. ([#26713](https://github.com/ethereum/go-ethereum/pull/26713))
- ethclient no longer panics when requesting missing blocks. This fixes a regression introduced in v1.11.2. ([#26817](https://github.com/ethereum/go-ethereum/pull/26817))
- ethclient now returns block withdrawals, if present. ([#26778](https://github.com/ethereum/go-ethereum/pull/26778))
- During building of new blocks, failed transactions no longer count towards used block gas, improving block space utilization. ([#26799](https://github.com/ethereum/go-ethereum/pull/26799))
- Faster crawling time for the DNS crawler ([#26685](https://github.com/ethereum/go-ethereum/pull/26685))
- Use the last announced finalized block instead of LES CHT for the ancient limit ([#26685](https://github.com/ethereum/go-ethereum/pull/26685))
- CPU usage metrics (`geth.system/cpu/*`) are more accurate ([#26793](https://github.com/ethereum/go-ethereum/pull/26793))

For a full rundown of the changes please consult the Geth [1.11.3 release milestone](https://github.com/ethereum/go-ethereum/milestone/141?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.2) (2023-02-22)

Geth 1.11.2 (Kite's Nest) is a patch-release, fixing a couple of issues with the 1.11 release family.

- Fix a few small engine API discordances with the spec post-Shanghai ([#26696](https://github.com/ethereum/go-ethereum/pull/26696), [#26722](26722)).
- Fix unmarshalling JSON `null` values as a proper result instead of `nil` ([#26723](https://github.com/ethereum/go-ethereum/pull/26723)).
- Fix `dumpgenesis` which failed due to a bad database key write ([#26747](https://github.com/ethereum/go-ethereum/pull/26747)).
- Fix pending tx filter to return hashes, not full txs by default ([#26757](https://github.com/ethereum/go-ethereum/pull/26757)).
- Fix `eth_feeHistory` to accept decimal blocks again ([#26758](https://github.com/ethereum/go-ethereum/pull/26758)).
- Fix Ubuntu PPA builds after the Go 1.20 fallout.

Feature wise there's one change in this release: the downloader's chain sync messages are aggregated into periodic (8s) outputs instead of a log line for every batch of data imported ([#26676](https://github.com/ethereum/go-ethereum/pull/26676)).

For a full rundown of the changes please consult the Geth [1.11.2 release milestone](https://github.com/ethereum/go-ethereum/milestone/140?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.1) (2023-02-16)

This is a patch-release, fixing a couple of issues with the major release yesterday: 

- Make our MacOS-builds work again ([26653](https://github.com/ethereum/go-ethereum/pull/26653)), 
- Fix some flaws related to withdrawals (Shanghai), which were detected on  [`zhejiang`](https://zhejiang.ethpandaops.io/) testnet ([#26704](https://github.com/ethereum/go-ethereum/pull/26704) [#26707],(https://github.com/ethereum/go-ethereum/pull/26707)). 
- Fix an issue where geth refused to start following a partial datadir wipe [26703](https://github.com/ethereum/go-ethereum/pull/26703)

This version is ready for the Shanghai upgrade on Sepolia. 

----

If have _not_ already upgraded to `v1.11.0`, then you should also read the [release notes](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.0) for that version. 

If you _have_ already upgraded to `v1.11.0`, there is no urgency in upgrading to `v1.11.1`, unless you are directly affected by the issues; e.g. want to use of Sepolia. 

For a full rundown of the changes please consult the Geth [1.11.1 release milestone](https://github.com/ethereum/go-ethereum/milestone/139?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.11.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.0) (2023-02-15)

This is a major release, containing over 300 PRs. Our original intention was to release 1.11 with path-based storage, but eventually we decided it was time to make the release without waiting for PBSS to be merged, so we can get back to a steady release schedule.

We hope to soon follow up the 1.11 release with 1.12, including PBSS.

### Shanghai Upgrade Support

Most of the code for the [Shanghai fork][shanghai] is merged into 1.11, but activation of the fork is not configured. The Shanghai protocol upgrade will contain the ability to make withdrawals from the consensus-layer (beacon chain). Withdrawals are specified in [EIP-4895: Beacon chain push withdrawals as operations](https://eips.ethereum.org/EIPS/eip-4895).

Other features included are [EIP-3855: PUSH0 instruction](https://eips.ethereum.org/EIPS/eip-3855), [EIP-3860: Limit and meter initcode](https://eips.ethereum.org/EIPS/eip-3860) and [EIP-3651: Warm COINBASE](https://eips.ethereum.org/EIPS/eip-3651).

[shanghai]: https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/shanghai.md

### A New Database Backend

[`Pebble`](https://github.com/cockroachdb/pebble/) was added as a database backend to replace good old [`LevelDB`](https://github.com/syndtr/goleveldb).

LevelDB has served us very well over the years, alas, it is a one-person project where the maintainer has signaled that the project is not a priority. We have been forced to consider three options:

1. Stick with goleveldb as long as we can, don't worry about it,
2. Fork goleveldb (or some other database), and maintain it ourselves.
3. Pick a different database, actively maintained by a dedicated team.

Since option one is not really a long-term solution, the choice was between two and three. Maintaining a database is a huge effort, and is not a burden we want to carry. Hence, we have been aiming for option 3, and eventually settled on Pebble.

Pebble is actively maintained by a dedicated team, and is used by other projects. We hope that this is a good long-term bet. It has performed well in our benchmark tests, we are very interested in getting feedback from actual production systems. In order to use `pebble`,

- You need to be on a 64-bit system,
- You also need to resync from scratch (with or without ancients) -- there is no migration functionality,
- And you need to specify `--db.engine=pebble` initially. For subsequent runs, geth should discover `pebble` automatically.

### Removed Features

We have removed support for the `ropsten` and `kiln` test networks. We have also removed libraries for mobile development and the `puppeth` tool.

The `personal` RPC namespace is now deprecated. In order to interact with `personal` APIs, you need to specifically allow it via the `--rpc.enabledeprecatedpersonal` command-line flag.

Support for certain legacy files configuration files was dropped. `geth` now will ignore these datadir files:

- `static-nodes.json`
- `trusted-nodes.json`

If any these are found, an error will be printed to the log. Setups using such files should use to the [TOML configuration-file][tomlfile] instead.

[tomlfile]: https://geth.ethereum.org/docs/fundamentals/config-files

### Backwards-Incompatible Changes

When using geth for mining or as a clique sealer, the `--miner.etherbase` flag now has to be be set explicitly. Previously, Geth would use the 'first' local account as etherbase automatically, but this possibility has been removed in Geth 1.11.

This change does not affect proof-of-stake networks because the fee recipient address is provided by the consenus-layer and not configured in Geth anymore.

Geth's JSON-RPC server has become more strict; the `JSON-RPC` spec requires the version field to be exactly `"jsonrpc": "2.0"`. This is now verified by the server -- a change which is not backwards-compatible with non-conforming client implementations.

If you are building from source, go-ethereum now requires Go version `1.18` or later ([#26160](https://github.com/ethereum/go-ethereum/pull/26160)).

The `callTracer` includes intentional breaking changes. Please refer to the Tracing section of the release notes.

### New Features

[#26149](https://github.com/ethereum/go-ethereum/pull/26149) added an option to direct log output to a file. This feature has been requested a lot. It's sometimes useful to have this available when running geth in an environment that doesn't easily allow redirecting the output.

We have added a method `debug_setTrieFlushInterval` to make it possible to set the trie flush interval via RPC ([#24785](https://github.com/ethereum/go-ethereum/pull/24785)). Essentially, this makes it possible to configure the node to be more or less "archive:ish", and without restarting the node while reconfiguring it.

Geth can now set custom HTTP headers, in particular for two scenarios:

- `geth attach`
- `geth` commands which can use `--remotedb`, e.g `geth db inspect`

The ability to use custom headers is typically useful for connecting to cloud-apis, e.g. providing an infura- or alchemy key, or for that matter access-keys for environments behind cloudflare.

#### Tracing

`callTracer` fields `gasUsed` and `value` have changed in the following cases:

- `gasUsed` of the top call frame now accounts for intrinsic gas and refunds. (https://github.com/ethereum/go-ethereum/pull/26048)
- In case of a DELEGATECALL frame, the value which was previously null is set to the parent call's value. This is done to match EVM semantics. (https://github.com/ethereum/go-ethereum/pull/26632)

For tracing, it is sometimes desirable to capture logs triggered by a trace, when using the `callTracer`. For example: call `USDT.transfer` will trigger a `Transfer(from, to, value)` event. By specifying `{"withLog": true}`, these events will be collected. Some bugs related to tracing and `gasUsed` was fixed.

 For `prestateTracer`, the result will from now on omit empty fields instead of including a zero value (e.g. no more `balance: '0x'`). The prestateTracer now takes an option `diffMode: bool`. In this mode the tracer will output the pre state and post data for the modified parts of state.

[#26241](https://github.com/ethereum/go-ethereum/pull/26241) should make it easier to sign EIP-712 typed data via the accounts.Wallet API, by using the mimetype for typed data.

It is now possible to get the result for multiple tracers in one go via the `muxTracer`.

#### Filter System

Via the filter subscriptions to `newPendingTransactions`, one can now subscribe to the _full_ pending transactions, as opposed to just being notified about hashes.

The keywords `safe` and `finalized` can now also be used as block range specifiers when requesting logs.

## All Changes

### Larger changes

- Added `pebble` as a new database backend. ([#26517](https://github.com/ethereum/go-ethereum/pull/26517), [#26650](https://github.com/ethereum/go-ethereum/pull/26650))
- Make use of golang generics ([#26290](https://github.com/ethereum/go-ethereum/pull/26290)  [#26194](https://github.com/ethereum/go-ethereum/pull/26194), [#26162](https://github.com/ethereum/go-ethereum/pull/26162), [#26159](https://github.com/ethereum/go-ethereum/pull/26159))
- Prepare for upcoming Shanghai hardfork ([#26645](https://github.com/ethereum/go-ethereum/pull/26645), [#26624](https://github.com/ethereum/go-ethereum/pull/26624), [#26554](https://github.com/ethereum/go-ethereum/pull/26554), [#26548](https://github.com/ethereum/go-ethereum/pull/26548),[#26232](https://github.com/ethereum/go-ethereum/pull/26232), [#26591](https://github.com/ethereum/go-ethereum/pull/26591), [#26565](https://github.com/ethereum/go-ethereum/pull/26565), [#26563](https://github.com/ethereum/go-ethereum/pull/26563), [#26555](https://github.com/ethereum/go-ethereum/pull/26555), [#26549](https://github.com/ethereum/go-ethereum/pull/26549), [#26484](https://github.com/ethereum/go-ethereum/pull/26484), [#26474](https://github.com/ethereum/go-ethereum/pull/26474), [#26475](https://github.com/ethereum/go-ethereum/pull/26475), [#23847](https://github.com/ethereum/go-ethereum/pull/23847), [#26458](https://github.com/ethereum/go-ethereum/pull/26458))
- Remove Ropsten support ([#26644](https://github.com/ethereum/go-ethereum/pull/26644))
- Prepare for path-based-storage ([#26637](https://github.com/ethereum/go-ethereum/pull/26637), [#26603](https://github.com/ethereum/go-ethereum/pull/26603), [#26324](https://github.com/ethereum/go-ethereum/pull/26324), [#25532](https://github.com/ethereum/go-ethereum/pull/25532), [#25896](https://github.com/ethereum/go-ethereum/pull/25896))
- Remove mobile packages ([#26599](https://github.com/ethereum/go-ethereum/pull/26599))
- Deprecate `personal` namespace ([#26390](https://github.com/ethereum/go-ethereum/pull/26390))
- internal/debug: add --log.file option ([#26149](https://github.com/ethereum/go-ethereum/pull/26149))
- eth/tracers: add multiplexing tracer ([#26086](https://github.com/ethereum/go-ethereum/pull/26086))
- eth/tracers: add diffMode to prestateTracer ([#25422](https://github.com/ethereum/go-ethereum/pull/25422))

### Minor changes and features

- rpc: remove DecimalOrHex type ([#26629](https://github.com/ethereum/go-ethereum/pull/26629))
- Make abi codec more strict ([#26568](https://github.com/ethereum/go-ethereum/pull/26568))
- core/vm: improve EVM instance reusability ([#26341](https://github.com/ethereum/go-ethereum/pull/26341))
- core: improve ambiguous block validation message ([#26582](https://github.com/ethereum/go-ethereum/pull/26582))
- log: better sanitation ([#26556](https://github.com/ethereum/go-ethereum/pull/26556), [#26630](https://github.com/ethereum/go-ethereum/pull/26630))
- p2p/discover: add more packet information in logs ([#26307](https://github.com/ethereum/go-ethereum/pull/26307))
- common/mclock: add Alarm ([#26333](https://github.com/ethereum/go-ethereum/pull/26333))
- core/rawdb: implement resettable freezer ([#26324](https://github.com/ethereum/go-ethereum/pull/26324))
- signer: enable typed data signing from signer rpc ([#26241](https://github.com/ethereum/go-ethereum/pull/26241))
- rpc: support injecting HTTP headers through context ([#26023](https://github.com/ethereum/go-ethereum/pull/26023))
- improve reading Go runtime metrics ([#25886](https://github.com/ethereum/go-ethereum/pull/25886))
- Changes for Cancun hardfork
  - Implement EIP-1153: Transient Storage ([#26003](https://github.com/ethereum/go-ethereum/pull/26003))
  - Implement `eth/68` to support larger transactions over the wire ([#25980](https://github.com/ethereum/go-ethereum/pull/25980))
 - Add `60s` timeout to graphql queries([#26116](https://github.com/ethereum/go-ethereum/pull/26116))
 - common/types: add `Address.Big` ([#26132](https://github.com/ethereum/go-ethereum/pull/26132))
 - p2p/discover: improve discv5 NODES response packing ([#26033](https://github.com/ethereum/go-ethereum/pull/26033))
- eth/tracers: add withLog to callTracer ([#25991](https://github.com/ethereum/go-ethereum/pull/25991))
- eth/filters, ethclient/gethclient: add fullTx option to pending tx filter ([#25186](https://github.com/ethereum/go-ethereum/pull/25186))
- node: drop support for static & trusted node list files ([#25610](https://github.com/ethereum/go-ethereum/pull/25610))
- accounts/usbwallet: support Ledger Nano S Plus and FTS ([#25933](https://github.com/ethereum/go-ethereum/pull/25933))
- cmd/abigen: change --exc to exclude by type name ([#22620](https://github.com/ethereum/go-ethereum/pull/22620))
- rpc: improve error codes for internal server errors ([#25678](https://github.com/ethereum/go-ethereum/pull/25678))
- rpc: check that "version" is "2.0" in request objects ([#25570](https://github.com/ethereum/go-ethereum/pull/25570))
- node, rpc: add JWT auth support in client ([#24911](https://github.com/ethereum/go-ethereum/pull/24911))
- eth/fetcher: throttle peers which deliver many invalid transactions ([#25573](https://github.com/ethereum/go-ethereum/pull/25573))
- eth/tracers: fix gasUsed for native and JS tracers ([#26048](https://github.com/ethereum/go-ethereum/pull/26048))
- core/vm: set tracer-observable value of a delegatecall to match parent value ([#26632](https://github.com/ethereum/go-ethereum/pull/26632))

### Changes related to testing

- core/vm: add bn256ScalarMul testcase for zero scalar value ([#26607](https://github.com/ethereum/go-ethereum/pull/26607))
- cmd/evm: add blocktest subcommand to evm ([#26526](https://github.com/ethereum/go-ethereum/pull/26526))
- core/state: remove notion of fake storage ([#24916](https://github.com/ethereum/go-ethereum/pull/24916))
- Make it possible to set the trie flush interval via RPC ([#24785](https://github.com/ethereum/go-ethereum/pull/24785))
- cmd/evm: output stateroot in statetest result ([#26297](https://github.com/ethereum/go-ethereum/pull/26297))
- eth/catalyst: make tests less time-sensitive ([#26201](https://github.com/ethereum/go-ethereum/pull/26201))
- rpc, internal/guide: speed up tests a bit ([#26193](https://github.com/ethereum/go-ethereum/pull/26193))
- cmd/evm: slight change in how t8n handles coinbase pre eip-158 ([#26139](https://github.com/ethereum/go-ethereum/pull/26139))
- eth/tracers: remove revertReasonTracer, add revert reason to callTracer ([#25508](https://github.com/ethereum/go-ethereum/pull/25508))

### Bugfixes

- Fix a flaw in snap-sync, where geth would stall state-sync until blocks were fully downloaded ([#26453](https://github.com/ethereum/go-ethereum/pull/26453))
- Fix cornercase shutdown behaviour in freezer, and be more diligent about performing `fsync` on close ([#26485](https://github.com/ethereum/go-ethereum/pull/26485), [#26490](https://github.com/ethereum/go-ethereum/pull/26490)). More fixes to the freezer were implemented in ([#26245](https://github.com/ethereum/go-ethereum/pull/26245)) and  ([#26251](https://github.com/ethereum/go-ethereum/pull/26251))
- rpc: fix off-by-one in ipc endpoint length check ([#26614](https://github.com/ethereum/go-ethereum/pull/26614))
- params: fix timestamp display in fork banner ([#26553](https://github.com/ethereum/go-ethereum/pull/26553))
- console, internal/jsre: fix autocomplete issues ([#26518](https://github.com/ethereum/go-ethereum/pull/26518))
- les/fetcher : fix requestTimer leak ([#26514](https://github.com/ethereum/go-ethereum/pull/26514))
- metrics/influxdb: fix time ticker leaks ([#26507](https://github.com/ethereum/go-ethereum/pull/26507))
- tests: fix DIFFICULTY error in state executor ([#26465](https://github.com/ethereum/go-ethereum/pull/26465))
- core: reset txpool on sethead ([#26392](https://github.com/ethereum/go-ethereum/pull/26392))
- core: fix state flushing for catalyst mode ([#26319](https://github.com/ethereum/go-ethereum/pull/26319))
- graphql, node, rpc: improve HTTP write timeout handling ([#25457](https://github.com/ethereum/go-ethereum/pull/25457))
- include transaction sender in pending-subscription ([#26126](https://github.com/ethereum/go-ethereum/pull/26126))
- common/lru: fix race in lru ([#26164](https://github.com/ethereum/go-ethereum/pull/26164))
- p2p/discover: fix handling of distance 256 in lookupDistances ([#26087](https://github.com/ethereum/go-ethereum/pull/26087))
- Fix trace call for inner reverts ([#25971](https://github.com/ethereum/go-ethereum/pull/25971))
- eth/tracers: fix gasUsed for native and JS tracers ([#26048](https://github.com/ethereum/go-ethereum/pull/26048))
- eth/tracers: fix the issue prestate missing existing contract state ([#25996](https://github.com/ethereum/go-ethereum/pull/25996))
- eth/filters: fix for eth_getLogs failing with finalized- and safe tag  ([#25922](https://github.com/ethereum/go-ethereum/pull/25922))
- eth/traces: add state limit ([#25812](https://github.com/ethereum/go-ethereum/pull/25812))
- ethclient/gethclient: fix bugs in override object encoding ([#25616](https://github.com/ethereum/go-ethereum/pull/25616))
- eth/downloader, les/downloader: fix subtle flaw in queue delivery ([#25861](https://github.com/ethereum/go-ethereum/pull/25861))
- core: fix datarace in txpool, fixes [#25870](https://github.com/ethereum/go-ethereum/pull/25870) and [#25869](https://github.com/ethereum/go-ethereum/pull/25869)  ([#25872](https://github.com/ethereum/go-ethereum/pull/25872))
- eth/catalyst: add locking around newpayload ([#25816](https://github.com/ethereum/go-ethereum/pull/25816))
- eth: fix a rare datarace on CHT challenge reply / shutdown
- node: fix HTTP server always force closing ([#25755](https://github.com/ethereum/go-ethereum/pull/25755))
- eth/protocols/snap: throttle trie heal requests when peers DoS us ([#25666](https://github.com/ethereum/go-ethereum/pull/25666))
- eth/protocols/snap: fix problems due to idle-but-busy peers
- trie: update comments + err check for preimages ([#25672](https://github.com/ethereum/go-ethereum/pull/25672))
- trie: check childrens' existence concurrently for snap heal ([#25694](https://github.com/ethereum/go-ethereum/pull/25694))
- eth, les: rework chain tracer ([#25143](https://github.com/ethereum/go-ethereum/pull/25143))

For a full rundown of the changes please consult the Geth [1.11.0 release milestone](https://github.com/ethereum/go-ethereum/milestone/136?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.26](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.26) (2022-11-03)

Geth v1.10.26 contains backports of bug-fixes from the main branch.

- The JSON-RPC client no longer hangs when invalid batch results are returned by the server. ([#26064](https://github.com/ethereum/go-ethereum/pull/26064))
- A corner-case issue in the filter system is resolved. ([#26054](https://github.com/ethereum/go-ethereum/pull/26054))
- Various improvements to snap sync are included in this release. ([#25831](https://github.com/ethereum/go-ethereum/pull/25831), [#25694](https://github.com/ethereum/go-ethereum/pull/25694), [#25666](https://github.com/ethereum/go-ethereum/pull/25666), [#25651](https://github.com/ethereum/go-ethereum/pull/25651))

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.25) (2022-09-15)

Geth v1.10.25 is a tiny update to flip the mainnet chain configuration to be post-merge. This disables legacy sync and will prevent Geth from even starting sync until a consensus client is attached and sends forkchoice updates. The update prevents bad PoW actors from trying to get the node to - temporarily, but annoyingly - download bad state data.

This release is optional and only affects initial sync. The release was made mostly for completeness' sake rather than out of necessity.

For a full rundown of the changes please consult the Geth [1.10.25 release milestone](https://github.com/ethereum/go-ethereum/milestone/138?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://hub.docker.com/r/ethereum/client-go/).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.24](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.24) (2022-09-14)

Geth v1.10.24 is a small hotfix release for users of the GraphQL APIs. It fixes a single bug where filtering for logs from a single transaction via GraphQL returned logs from the entire block, not just the single transaction. Single transaction log filtering is unavailable via the RPC and does not impact the merge.

**You only need to apply this change if you rely on both GraphQL and single transaction log filtering.**

For a full rundown of the changes please consult the Geth [1.10.24 release milestone](https://github.com/ethereum/go-ethereum/milestone/137?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.23) (2022-08-24)

Geth v1.10.23 is a hotfix release for a pruning regression that was introduced in v1.10.22. For technical details on the bug, please check out the [PR that fixes it](https://github.com/ethereum/go-ethereum/pull/25581).

If anyone updated to v1.10.22 in these past couple of days, there is a fairly high probability that some state data might have gone missing from your node. Doing a full check on the state is possible `geth snapshot traverse-state`, but will likely take a day and the fix is all the same anyway.

To ensure that your node has all the data, please rewind your local chain to a block before you updated (if unsure, just pick a block before the release time) with `debug.setHead("0xblock-number-in-hex")` via the Geth console (on IPC), or `debug_setHead` via JSON RPC (you might need to *temporarilly* expose the `debug` namespace to do that). The brute force alternative of course is to resync after an update, which you can do by deleting your `chaindata` folder (but please leave the `ancient` folder within to keep the blocks).

We apologize for this regression and the headaches fixing it will entail on your side. We've learnt the hard way that there's an untested class of bugs that appear across full sync restarts.

For a full rundown of the changes please consult the Geth [1.10.23 release milestone](https://github.com/ethereum/go-ethereum/milestone/135?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

---

## Original Merge release notes

Geth ~v1.10.22~ v1.10.23 **enables the Merge for the Ethereum mainnet at a Terminal Total Difficulty of** `58_750_000_000_000_000_000_000`.

This TTD is expected to be reached on the 15. September 2022.

#### Merge EIPs
- [EIP-3676](https://eips.ethereum.org/EIPS/eip-3675): Upgrade consensus to Proof-of-Stake
- [EIP-4399](https://eips.ethereum.org/EIPS/eip-4399): Supplant DIFFICULTY opcode with PREVRANDAO

#### Additional notes about the merge changes

- This release configures the Terminal Total Difficulty for mainnet. ([#25528](https://github.com/ethereum/go-ethereum/pull/25528))
- Many engine API issues found by hive have been fixed for this release. ([#25552](https://github.com/ethereum/go-ethereum/pull/25552), [#25423](https://github.com/ethereum/go-ethereum/pull/25423), [#25414](https://github.com/ethereum/go-ethereum/pull/25414), [#25416](https://github.com/ethereum/go-ethereum/pull/25416), [#25428](https://github.com/ethereum/go-ethereum/pull/25428))
- The Goerli testnet is now internally configured as 'successfully merged'. ([#25519](https://github.com/ethereum/go-ethereum/pull/25519), [#24538](https://github.com/ethereum/go-ethereum/pull/24538))

#### JSON-RPC API

- The log filtering system now uses a LRU cache for block logs, speeding up repeated queries for the same block range. The cache size can be configured using the `--cache.blocklogs` command-line flag. ([#25459](https://github.com/ethereum/go-ethereum/pull/25459))
- `eth_createAccessList` is now much faster when no gas limit is provided. ([#25467](https://github.com/ethereum/go-ethereum/pull/25467))
- `eth_feeHistory` now also works with the `finalized` block specifier. ([#25442](https://github.com/ethereum/go-ethereum/pull/25442))
- The built-in callTracer now supports an option `onlyTopCall`. Enabling this option makes the tracer skip internal calls. We added this option to enable use of the `callTracer` to get the return data of reverted transactions. ([#25430](https://github.com/ethereum/go-ethereum/pull/25430))

#### Go-library changes

- Storage of trie node hash preimages is now disabled by default. You can enable it again using the `--cache.preimages` flag. ([#25287](https://github.com/ethereum/go-ethereum/pull/25287), [#25538](https://github.com/ethereum/go-ethereum/pull/25538), [#25533](https://github.com/ethereum/go-ethereum/pull/25533))
- The ethash mining implemenation now removes temporary DAG files, which could be left of disk when geth was interrupted while generating a DAG. ([#25381](https://github.com/ethereum/go-ethereum/pull/25381))
- ethclient now supports the `eth_feeHistory` method. ([#25403](https://github.com/ethereum/go-ethereum/pull/25403))
- The eth wire protocol test suite now supports protocol version eth/67. ([#25306](https://github.com/ethereum/go-ethereum/pull/25306))
- RLP-decoding of trie nodes is ~33% faster due to reduced allocations in the decoder. ([#25357](https://github.com/ethereum/go-ethereum/pull/25357))
- The RPC server supports a new option `ReadHeaderTimeout`. ([#25338](https://github.com/ethereum/go-ethereum/pull/25338))
- Registering of clef ruleset UIs should now work correctly. ([#25455](https://github.com/ethereum/go-ethereum/pull/25455))

#### Build

- Geth binaries in docker are now statically-linked. ([#25492](https://github.com/ethereum/go-ethereum/pull/25492))
- This release is built using Go 1.18.5. ([#25461](https://github.com/ethereum/go-ethereum/pull/25461))

For a full rundown of the original merge release changes please consult the Geth [1.10.22 release milestone](https://github.com/ethereum/go-ethereum/milestone/134?closed=1).

## [v1.10.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.22) (2022-08-22)

### WARNING: This release seems to contain a regression that can corrupt the local state. Please hold on with updating while we investigate the issue!

---

Geth v1.10.22 **enables the Merge for the Ethereum mainnet at a Terminal Total Difficulty of** `58_750_000_000_000_000_000_000`.

This TTD is expected to be reached on the 15. September 2022.

#### Merge EIPs
- [EIP-3676](https://eips.ethereum.org/EIPS/eip-3675): Upgrade consensus to Proof-of-Stake
- [EIP-4339](https://eips.ethereum.org/EIPS/eip-4399): Supplant DIFFICULTY opcode with PREVRANDAO

#### Additional notes about the merge changes

- This release configures the Terminal Total Difficulty for mainnet. ([#25528](https://github.com/ethereum/go-ethereum/pull/25528))
- Many engine API issues found by hive have been fixed for this release. ([#25552](https://github.com/ethereum/go-ethereum/pull/25552), [#25423](https://github.com/ethereum/go-ethereum/pull/25423), [#25414](https://github.com/ethereum/go-ethereum/pull/25414), [#25416](https://github.com/ethereum/go-ethereum/pull/25416), [#25428](https://github.com/ethereum/go-ethereum/pull/25428))
- The Goerli testnet is now internally configured as 'successfully merged'. ([#25519](https://github.com/ethereum/go-ethereum/pull/25519), [#24538](https://github.com/ethereum/go-ethereum/pull/24538))

#### JSON-RPC API

- The log filtering system now uses a LRU cache for block logs, speeding up repeated queries for the same block range. The cache size can be configured using the `--cache.blocklogs` command-line flag. ([#25459](https://github.com/ethereum/go-ethereum/pull/25459))
- `eth_createAccessList` is now much faster when no gas limit is provided. ([#25467](https://github.com/ethereum/go-ethereum/pull/25467))
- `eth_feeHistory` now also works with the `finalized` block specifier. ([#25442](https://github.com/ethereum/go-ethereum/pull/25442))
- The built-in callTracer now supports an option `onlyTopCall`. Enabling this option makes the tracer skip internal calls. We added this option to enable use of the `callTracer` to get the return data of reverted transactions. ([#25430](https://github.com/ethereum/go-ethereum/pull/25430))

#### Go-library changes

- Storage of trie node hash preimages is now disabled by default. You can enable it again using the `--cache.preimages` flag. ([#25287](https://github.com/ethereum/go-ethereum/pull/25287), [#25538](https://github.com/ethereum/go-ethereum/pull/25538), [#25533](https://github.com/ethereum/go-ethereum/pull/25533))
- The ethash mining implemenation now removes temporary DAG files, which could be left of disk when geth was interrupted while generating a DAG. ([#25381](https://github.com/ethereum/go-ethereum/pull/25381))
- ethclient now supports the `eth_feeHistory` method. ([#25403](https://github.com/ethereum/go-ethereum/pull/25403))
- The eth wire protocol test suite now supports protocol version eth/67. ([#25306](https://github.com/ethereum/go-ethereum/pull/25306))
- RLP-decoding of trie nodes is ~33% faster due to reduced allocations in the decoder. ([#25357](https://github.com/ethereum/go-ethereum/pull/25357))
- The RPC server supports a new option `ReadHeaderTimeout`. ([#25338](https://github.com/ethereum/go-ethereum/pull/25338))
- Registering of clef ruleset UIs should now work correctly. ([#25455](https://github.com/ethereum/go-ethereum/pull/25455))

#### Build

- Geth binaries in docker are now statically-linked. ([#25492](https://github.com/ethereum/go-ethereum/pull/25492))
- This release is built using Go 1.18.5. ([#25461](https://github.com/ethereum/go-ethereum/pull/25461))

For a full rundown of the changes please consult the Geth [1.10.22 release milestone](https://github.com/ethereum/go-ethereum/milestone/134?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.21) (2022-07-27)

Geth v1.10.21 is a maintenance release, adding built-in configuration for the merge fork on the Goerli testnet and the `mergeNetsplitBlock` for the Sepolia testnet.

Specifically, this release

- **defines a terminal total difficulty for Goerli as 10_790_000** ([#25324](https://github.com/ethereum/go-ethereum/pull/25324)) and
- **defines the mergeNetsplitBlock as 1735371 for Sepolia**. ([#25372](https://github.com/ethereum/go-ethereum/pull/25372))

### Command changes

- The `--netrestrict` option is now also applied for discv5. ([#25304](https://github.com/ethereum/go-ethereum/pull/25304))
- DNS discovery is now enabled for the Sepolia testnet. ([#25288](https://github.com/ethereum/go-ethereum/pull/25288))
- puppeth can no longer deploy parity and pyethapp because these clients are unmaintained. ([#25329](https://github.com/ethereum/go-ethereum/pull/25329))
- abigen now has a workaround for parameter names which are also Go keywords. ([#25307](https://github.com/ethereum/go-ethereum/pull/25307))
- A few minor regressions in geth CLI argument handling are fixed. ([#25234](https://github.com/ethereum/go-ethereum/pull/25234))

### RPC API changes

- In block-based RPC methods like `eth_getBlockByNumber`, the `safe` block specifier can now be used to refer to the "safe" block post-merge. ([#25165](https://github.com/ethereum/go-ethereum/pull/25165))
- The `baseFee` can now be overridden in block tracing. ([#25219](https://github.com/ethereum/go-ethereum/pull/25219))
- RPC methods returning transaction objects now return the `chainId` for legacy transactions. ([#25244](https://github.com/ethereum/go-ethereum/pull/25244))
- In `eth_sendTransaction`, the `chainId` parameter now verified even for legacy transactions. ([#25157](https://github.com/ethereum/go-ethereum/pull/25157))
- `eth_call`, `eth_estimateGas` no longer add tx fees to the coinbase account. ([#25214](https://github.com/ethereum/go-ethereum/pull/25214))
- A new built-in tracer, `revertReasonTracer`, has been added. ([#25265](https://github.com/ethereum/go-ethereum/pull/25265))

### Merge changes

- The engine API can no longer perform block insertion while the client is snap-syncing. ([#25210](https://github.com/ethereum/go-ethereum/pull/25210))
- When trying to set bad blocks retrieved via sync as canon, the API now returns INVALID. ([#25190](https://github.com/ethereum/go-ethereum/pull/25190))
- The engine API endpoint ('authrpc') is now enabled by default. ([#25152](https://github.com/ethereum/go-ethereum/pull/25152), [#25394](https://github.com/ethereum/go-ethereum/pull/25394))
- Several other engine API bugs found during #TestingTheMerge are fixed. ([#25230](https://github.com/ethereum/go-ethereum/pull/25230), [#25136](https://github.com/ethereum/go-ethereum/pull/25136), [#25236](https://github.com/ethereum/go-ethereum/pull/25236))

### Go Library Changes

- The snap sync implementation has been updated in preparation for 'path-based state storage'. ([#24898](https://github.com/ethereum/go-ethereum/pull/24898))
- The HTTP RPC server will no longer hang on shutdown even with very busy connections. ([#25258](https://github.com/ethereum/go-ethereum/pull/25258))
- Package signer/core/apitypes now provides a function to calculate the EIP-712 typed data hash. ([#25283](https://github.com/ethereum/go-ethereum/pull/25283))

### Build changes

- We have reverted to an older version of goleveldb because recent versions have buggy compaction and manifest handling. ([#25413](https://github.com/ethereum/go-ethereum/pull/25413))
- This release is built with Go 1.18.4 ([#25247](https://github.com/ethereum/go-ethereum/pull/25247), [#25293](https://github.com/ethereum/go-ethereum/pull/25293))
- Release tarballs have proper directory timestamps. ([#25290](https://github.com/ethereum/go-ethereum/pull/25290))

For a full rundown of the changes please consult the Geth [1.10.21 release milestone](https://github.com/ethereum/go-ethereum/milestone/133?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.20) (2022-06-29)

Geth v1.10.20 is a maintenance release, adding built-in configuration for the merge fork on the Sepolia testnet. 

Specifically, this release **defines a terminal total difficulty for Sepolia** ([#25179](https://github.com/ethereum/go-ethereum/pull/25179)).

### Geth command changes

- Geth and most other command-line tools now use a newer version of the command-line argument/flag handling library. There is one new restriction with this change: flags must now be given before other arguments. Very few of Geth's subcommands take arguments, so this is unlikely to cause issues. ([#24751](https://github.com/ethereum/go-ethereum/pull/24751))
- The `geth js` subcommand has been removed. ([#25000](https://github.com/ethereum/go-ethereum/pull/25000))
- The new `--discovery.port` flag allows configuring a separate port for the UDP listener. ([#24979](https://github.com/ethereum/go-ethereum/pull/24979))
- Setting p2p bootstrap nodes in the config file now works even when a pre-defined network is selected on the command-line. ([#25174](https://github.com/ethereum/go-ethereum/pull/25174))

### RPC API changes

- `eth_chainId` now always returns the configured chain ID regardless of sync status. This is a violation of EIP-695, but the previous behavior caused issues with CL clients. ([#25166](https://github.com/ethereum/go-ethereum/pull/25166), [#25168](https://github.com/ethereum/go-ethereum/pull/25168))
- Transaction objects returned by RPC (e.g. from `eth_getTransactionByHash`) now always include the `chainId` field, even for untyped (legacy) transactions. ([#25155](https://github.com/ethereum/go-ethereum/pull/25155))
- The deprecated RPC method `personal_signAndSendTransaction` has been removed. ([#25111](https://github.com/ethereum/go-ethereum/pull/25111))
- Handling of certain reorg corner cases is improved in the Engine API. ([#25187](https://github.com/ethereum/go-ethereum/pull/25187), [#25139](https://github.com/ethereum/go-ethereum/pull/25139))
- A performance regression in JS tracing is resolved. ([#25156](https://github.com/ethereum/go-ethereum/pull/25156))

### Build changes

- Bash and zsh completions are now installed by the geth Ubuntu package. ([#25195](https://github.com/ethereum/go-ethereum/pull/25195), [#25204](https://github.com/ethereum/go-ethereum/pull/25204))

For a full rundown of the changes please consult the Geth 1.10.20 [release milestone](https://github.com/ethereum/go-ethereum/milestone/132?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.19) (2022-06-15)

Geth v1.10.19 is yet another feature release.

**The release contains the Gray Glacier fork definition**. The [Gray Glacier](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-5133.md) fork is a difficulty-bomb postponement, which is expected to go live on Ethereum Mainnet towards the end of June.

In other words: **all users are required to upgrade** before the Gray Glacier hardfork activates at block `15050000` ([#25088](https://github.com/ethereum/go-ethereum/pull/25088))

Changes which may cause incompatibilities:

- The `engine` API is now _only_ available with JWT authentication.
- Geth will refuse to start if legacy receipts are present in the database. The check can be disabled with `--ignore-legacy-receipts`, but we strongly recommend you run the conversion in this case ([#24943](https://github.com/ethereum/go-ethereum/pull/24943)).
- The RPC method `debug_traceCall` will now reject execution against the `pending` block ([#24871](https://github.com/ethereum/go-ethereum/pull/24871)).
- RPC timer metrics have been changed into histograms ([#25044](https://github.com/ethereum/go-ethereum/pull/25044)).

Other changes in this release:

- Updates related to The Merge on Ropsten, which is now a proof-of-stake network ([#25018](https://github.com/ethereum/go-ethereum/pull/25018), [#24975](https://github.com/ethereum/go-ethereum/pull/24975), [#25078](https://github.com/ethereum/go-ethereum/pull/25078))!
- The `debug_traceCall` RPC method now also supports block overrides; making fields like timestamp or the block number settable by the caller ([#24871](https://github.com/ethereum/go-ethereum/pull/24871)).
- A new diagnostic tool, `geth snapshot inspect-account`, allows investigations of snapshot data ([#24765](https://github.com/ethereum/go-ethereum/pull/24765)).
- Fixes and preparatory work related to The Merge ([#24946](https://github.com/ethereum/go-ethereum/pull/24946), [#25006](https://github.com/ethereum/go-ethereum/pull/25006), [#24955](https://github.com/ethereum/go-ethereum/pull/24955), [#24997](https://github.com/ethereum/go-ethereum/pull/24997)).
- Preparatory work for the upcoming path-based trie storage feature ([#24750](https://github.com/ethereum/go-ethereum/pull/24750)).
- Introduce `eth/67` protocol, dropping support for GetNodeData ([#24093](https://github.com/ethereum/go-ethereum/pull/24093)).
- Optimizations related to block processing ([#23500](https://github.com/ethereum/go-ethereum/pull/23500), [#24958](https://github.com/ethereum/go-ethereum/pull/24958), [#24616](https://github.com/ethereum/go-ethereum/pull/24616)).
- Tests/fuzzing improvements ([#25033](https://github.com/ethereum/go-ethereum/pull/25033), [#25038](https://github.com/ethereum/go-ethereum/pull/25038), [#25037](https://github.com/ethereum/go-ethereum/pull/25037), [#25036](https://github.com/ethereum/go-ethereum/pull/25036), [#25016](https://github.com/ethereum/go-ethereum/pull/25016), [#24249](https://github.com/ethereum/go-ethereum/pull/24249) [#24928](https://github.com/ethereum/go-ethereum/pull/24928)).
- Many updates to documentation ([#25086](https://github.com/ethereum/go-ethereum/pull/25086), [#25058](https://github.com/ethereum/go-ethereum/pull/25058), [#25057](https://github.com/ethereum/go-ethereum/pull/25057), [#25056](https://github.com/ethereum/go-ethereum/pull/25056), [#25040](https://github.com/ethereum/go-ethereum/pull/25040), [#25032](https://github.com/ethereum/go-ethereum/pull/25032)).
- Nicer format when showing chain config ([#24904](https://github.com/ethereum/go-ethereum/pull/24904)).

For a full rundown of the changes please consult the Geth 1.10.19 [release milestone](https://github.com/ethereum/go-ethereum/milestone/131?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.18) (2022-05-25)

After a long train of maintenance releases, we are happy to announce this feature release of Geth!

**This release is ready for the Merge transition on the Ropsten testnet**, and will activate the Merge on Ropsten when the testnet reaches a total difficulty of 43531756765713534.

Please ensure you have a beacon chain node configured for the transition. You can find more information about preparing for the Merge in our guide: <https://geth.ethereum.org/docs/interface/merge>.

**Note: the Merge transition on Ropsten was unsuccessful because the configured TD was reached earlier than expected. Please run geth with `--override.terminaltotaldifficulty 100000000000000000000000` on Ropsten for the time being.**

The tracing subsystem has been another focus area of this release cycle. We have replaced the JavaScript interpreter used by the tracing engine with [Goja](github.com/dop251/goja), which offers slightly better performance and implements many ES6 language features. There are minor differences in JS tracing semantics: The built-in functions `memory.slice`, `memory.getUint`, `stack.peek` will now throw an exception and interrupt tracing when accessing out-of-bounds data. They previously returned an empty value for invalid args instead.

This release contains further breaking changes to tracing APIs. Be sure to check the tracing changelog section. Please also report any incompatibilities you encounter.

### Changes related to the Merge

- Ropsten: terminal total difficulty is configured for the Merge. ([#24876](https://github.com/ethereum/go-ethereum/pull/24876))
- In RPC APIs where a block number can be given, you can now use `"finalized"` to refer to the latest finalized block. ([#24282](https://github.com/ethereum/go-ethereum/pull/24282))
- Geth can now serve CL requests to build a new block with very low latency because new blocks are constructed in the background. ([#24866](https://github.com/ethereum/go-ethereum/pull/24866))
- For CL-induced reorgs to a block with unavailable state, Geth now recomputes the state by re-processing ancestor blocks. ([#24613](https://github.com/ethereum/go-ethereum/pull/24613))
- The "engine" API implementation complies with the latest specification version. ([#24802](https://github.com/ethereum/go-ethereum/pull/24802), [#24855](https://github.com/ethereum/go-ethereum/pull/24855), [#24915](https://github.com/ethereum/go-ethereum/pull/24915))
- Several annoyances related to sync after the Merge have been fixed. ([#24691](https://github.com/ethereum/go-ethereum/pull/24691), [#24670](https://github.com/ethereum/go-ethereum/pull/24670), [#24610](https://github.com/ethereum/go-ethereum/pull/24610))

### Geth changes

- Geth's default gas limit target is now 30M. ([#24680](https://github.com/ethereum/go-ethereum/pull/24680))
- Geth now prints a warning when launching on the deprecated Rinkeby testnet. ([#24884](https://github.com/ethereum/go-ethereum/pull/24884))
- The free disk space monitor is more aggressive and the critical level at which it will initiate a shutdown is now set to 256 MB by default. ([#24781](https://github.com/ethereum/go-ethereum/pull/24781))
- Geth now prints a small guide when started in `--dev` mode. ([#24759](https://github.com/ethereum/go-ethereum/pull/24759))
- Geth no longer reports 'Snapshot extension registration failed' as an error. ([#24475](https://github.com/ethereum/go-ethereum/pull/24475))
- The `--eth.requiredblocks` flag was fixed to work correctly. ([#24817](https://github.com/ethereum/go-ethereum/pull/24817))
- Using `geth --dev` with a datadir previously initialized by `geth init` should now work correctly. ([#24693](https://github.com/ethereum/go-ethereum/pull/24693))
- `geth init` will now complain when creating a Clique-based chain without any configured signers. ([#24470](https://github.com/ethereum/go-ethereum/pull/24470))
- The new `geth db check-state-content` command checks integrity of trie nodes in the database. ([#24840](https://github.com/ethereum/go-ethereum/pull/24840))
- `geth snapshot verify-state` now checks for 'dangling' storage entries. ([#24643](https://github.com/ethereum/go-ethereum/pull/24643), [#24677](https://github.com/ethereum/go-ethereum/pull/24677))
- `evm t8n` can now run post-merge state transitions. ([#24546](https://github.com/ethereum/go-ethereum/pull/24546))
- The hex input data of `evm run` is now verified to have even length. ([#24721](https://github.com/ethereum/go-ethereum/pull/24721))
- Using Clef for clique block signing should be fixed. ([#24941](https://github.com/ethereum/go-ethereum/pull/24941))
- The example code that shows how to use Clef from Python works again. ([#24440](https://github.com/ethereum/go-ethereum/pull/24440))

### Core changes

- When generating a state snapshot from the state trie, Geth now ensures that 'dangling' storage entries from previous snapshots are removed from the database. This fixes an issue that could lead to incorrect EVM execution results after snap sync in certain cases. ([#24811](https://github.com/ethereum/go-ethereum/pull/24811))
- The transaction pool now correctly drops the oldest transactions when truncating the queue to stay below the configured global limit. ([#24908](https://github.com/ethereum/go-ethereum/pull/24908))
- The reference tests have been upgraded to version 10.4. ([#24899](https://github.com/ethereum/go-ethereum/pull/24899))
- EVM `MSTORE` is now 75% faster. ([#24847](https://github.com/ethereum/go-ethereum/pull/24847), [#24860](https://github.com/ethereum/go-ethereum/pull/24860))
- The EVM now implements EIP-3855 (PUSH0 instruction). This feature is not yet active in any fork. ([#24039](https://github.com/ethereum/go-ethereum/pull/24039))
- The ancient store ('freezer') implementation is now exported in package ethdb, and can be used independently of the chain database. ([#24684](https://github.com/ethereum/go-ethereum/pull/24684))
- The miner no longer commits the in-progress block to disk when interrupted by a new chain head event. This improves block creation latency. ([#24638](https://github.com/ethereum/go-ethereum/pull/24638))
- The Goerli testnet has new bootstrap nodes. ([#24900](https://github.com/ethereum/go-ethereum/pull/24900))
- An crash in LES ultralight sync is resolved. ([#24641](https://github.com/ethereum/go-ethereum/pull/24641))
- Several data races related to snap sync are fixed in this release. ([#24685](https://github.com/ethereum/go-ethereum/pull/24685))
- The snap protocol server no longer includes superfluous account proofs when a storage response hits the size limit. ([#24885](https://github.com/ethereum/go-ethereum/pull/24885))
- The snap client now de-duplicates trie node heal requests better, sorting them by the requested state trie path. This can slightly reduce the overall amount of data transferred during snap sync. ([#24779](https://github.com/ethereum/go-ethereum/pull/24779))
- The eth block fetcher now disconnects peers that repeatedly fail to reply to header requests. ([#24652](https://github.com/ethereum/go-ethereum/pull/24652))
- The trie implementation can now optionally trace nodes which were deleted/overwritten by state updates. ([#24403](https://github.com/ethereum/go-ethereum/pull/24403))

### Go library changes

- common/compiler: The Solidity and Vyper wrappers have been removed. This is a breaking change. ([#24936](https://github.com/ethereum/go-ethereum/pull/24936))
- `abigen --sol` does not work anymore. ([#24936](https://github.com/ethereum/go-ethereum/pull/24936))
- ethclient: `Client` now has a `PeerCount` method. ([#24849](https://github.com/ethereum/go-ethereum/pull/24849))
- ethclient/gethclient: `GetProof` now also returns storage proofs ([#24697](https://github.com/ethereum/go-ethereum/pull/24697))
- accounts/abi: decoder no longer crashes for invalid struct field names. ([#24932](https://github.com/ethereum/go-ethereum/pull/24932))
- accounts/abi: logs are now decoded correctly when all arguments are indexed. ([#24792](https://github.com/ethereum/go-ethereum/pull/24792))
- accounts/abi: `ParseSelector` now handles tuple arrays. ([#24587](https://github.com/ethereum/go-ethereum/pull/24587))
- signer/fourbyte: 4byte signatures have been updated. ([#22865](https://github.com/ethereum/go-ethereum/pull/22865), [#24842](https://github.com/ethereum/go-ethereum/pull/24842))
- mobile: `Receipt.EncodeJSON` now actually returns JSON instead of RLP. ([#24701](https://github.com/ethereum/go-ethereum/pull/24701))
- accounts/abi/bind: contract calls on the `pending` block now correctly return the `eth_call` RPC error, if any. ([#24649](https://github.com/ethereum/go-ethereum/pull/24649))
- core/types: the `miner` field is now optional when decoding block headers from JSON ([#24666](https://github.com/ethereum/go-ethereum/pull/24666))
- ethdb/remotedb: it is now possible to attach a chain database via RPC. This feature is meant to be used for debugging. ([#24905](https://github.com/ethereum/go-ethereum/pull/24905), [#24836](https://github.com/ethereum/go-ethereum/pull/24836))

### Tracing changes

- The JavaScript tracing environment now uses the Goja interpreter instead of Duktape. There should be no noticable differences in semantics resulting from this change. ([#23773](https://github.com/ethereum/go-ethereum/pull/23773), [#24934](https://github.com/ethereum/go-ethereum/pull/24934))
- In JS tracers, the `log.memory` object now has a `length` method. ([#24887](https://github.com/ethereum/go-ethereum/pull/24887))
- **API change**: In all tracers (JS, native, structlog), memory content is now captured before the EVM expands it. Previously, tracing would see memory after it had already been affected by the current instruction. ([#24867](https://github.com/ethereum/go-ethereum/pull/24867))
- **API change**: `debug_traceTransaction`, in the default structured-logging mode, now returns the refund counter for every EVM execution step. ([#24567](https://github.com/ethereum/go-ethereum/pull/24567))
- **API change**: in structlog steps, the fields `memory` and `returnData` will only be present when they contain non-empty data. ([#24547](https://github.com/ethereum/go-ethereum/pull/24547))
- Tracing now takes refunds and access list costs into account. This fixes inaccuracies: in JS tracers, the `intrinsicGas` property is now correct even when there are refunds. In the built-in "prestate" tracer, the sender balance added to the result should now be the true balance at the beginning of the transaction. ([#24510](https://github.com/ethereum/go-ethereum/pull/24510))
- In native (Go) tracers, the txhash/blockhash being traced is now available. ([#24679](https://github.com/ethereum/go-ethereum/pull/24679))

### RPC / GraphQL changes

- The GraphQL API now supports retrieving the binary representation of consensus objects. ([#24816](https://github.com/ethereum/go-ethereum/pull/24816), [#24738](https://github.com/ethereum/go-ethereum/pull/24738))
- In GraphQL queries, values of type `Long` (representing a block number, for example) can now be set using a query variable. ([#24864](https://github.com/ethereum/go-ethereum/pull/24864))
- Error messages printed/returned when accessing missing block logs are improved. ([#24617](https://github.com/ethereum/go-ethereum/pull/24617))
- `debug_testSignCliqueBlock` is not available anymore. ([#24837](https://github.com/ethereum/go-ethereum/pull/24837))
- The new `debug_getRawReceipts` RPC method returns the binary representation of block receipts. ([#24773](https://github.com/ethereum/go-ethereum/pull/24773))
- The new `debug_dbGet` method can be used to read from the chain database. ([#24739](https://github.com/ethereum/go-ethereum/pull/24739))

### Build changes

- This release is built with Go 1.18.1 ([#24689](https://github.com/ethereum/go-ethereum/pull/24689))
- Our PPA now publishes .deb packages for Ubuntu 22.04 LTS. ([#24813](https://github.com/ethereum/go-ethereum/pull/24813))
- Incremental builds of Docker images of Geth should be faster. ([#24796](https://github.com/ethereum/go-ethereum/pull/24796))
- Code generation tools are now tracked by go.mod, making `go generate` more deterministic. ([#24682](https://github.com/ethereum/go-ethereum/pull/24682))
- go-ethereum now uses the toolchain feature 'embed' instead of go-bindata. It is no longer necessary to run `go generate` after changing certain assets such as JS files. ([#24744](https://github.com/ethereum/go-ethereum/pull/24744))
- go-ethereum now takes advantage of several standard library features available in Go 1.16 and later. ([#24886](https://github.com/ethereum/go-ethereum/pull/24886), [#24869](https://github.com/ethereum/go-ethereum/pull/24869), [#24890](https://github.com/ethereum/go-ethereum/pull/24890), [#24861](https://github.com/ethereum/go-ethereum/pull/24861), [#24835](https://github.com/ethereum/go-ethereum/pull/24835), [#24633](https://github.com/ethereum/go-ethereum/pull/24633))
- Dependency conflicts related to github.com/btcsuite/btcd v0.22.0 are fixed. ([#24700](https://github.com/ethereum/go-ethereum/pull/24700), [#24939](https://github.com/ethereum/go-ethereum/pull/24939))

For a full rundown of the changes please consult the Geth 1.10.18 [release milestone](https://github.com/ethereum/go-ethereum/milestone/130?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.17) (2022-03-29)

This is a maintenance release. This release contains a lot of work in preparation for The Merge, and work for an upcoming change to the way state is stored in go-ethereum. 

This release also adds a new tool to convert 'legacy' receipts into a newer format. During startup, geth will check the database and tell you if you need to perform the conversion. Converting receipts is only needed if geth's `ancients` database has not been resynced from scratch during the last couple of years. It is recommended to back-up your receipts freezer table (`ancients/receipts*`) before performing the conversion.

Compatibility note about `core/types`: For optimization purposes, `types.Header` and other types in this package now implement the `rlp.Encoder` interface. This change can cause incompatibilities because the new method is implemented with pointer receiver. Attempting to RLP-encode unadressable (i.e. non-pointer) values of type `Header` does not work anymore and will result in an error.

### Change Log

- A lot of work towards "The Merge" (TM) was performed ([#24574](https://github.com/ethereum/go-ethereum/pull/24574), [#24569](https://github.com/ethereum/go-ethereum/pull/24569), [#24550](https://github.com/ethereum/go-ethereum/pull/24550), [#24548](https://github.com/ethereum/go-ethereum/pull/24548), [#24506](https://github.com/ethereum/go-ethereum/pull/24506), [#24545](https://github.com/ethereum/go-ethereum/pull/24545), [#23982](https://github.com/ethereum/go-ethereum/pull/23982), [#24522](https://github.com/ethereum/go-ethereum/pull/24522), [#24364](https://github.com/ethereum/go-ethereum/pull/24364))
- A lot optimizations for RLP encoding and package trie were added (roughly 20-30% improvement) ([#24126](https://github.com/ethereum/go-ethereum/pull/24126), [#24425](https://github.com/ethereum/go-ethereum/pull/24425), [#24420](https://github.com/ethereum/go-ethereum/pull/24420), [#24251](https://github.com/ethereum/go-ethereum/pull/24251))
- Preparatory work for an upcoming for state layout change ([#24460](https://github.com/ethereum/go-ethereum/pull/24460), [#23954](https://github.com/ethereum/go-ethereum/pull/23954), [#24486](https://github.com/ethereum/go-ethereum/pull/24486), [#24391](https://github.com/ethereum/go-ethereum/pull/24391), [#24392](https://github.com/ethereum/go-ethereum/pull/24392))
- GraphQL: fee history API methods were added, along with nonce for pending accounts ([#24452](https://github.com/ethereum/go-ethereum/pull/24452))
- The non-cgo fallback secp256k1 crypto library was updated and is now ~25% faster ([#24533](https://github.com/ethereum/go-ethereum/pull/24533)) 
- Support for signing nested types via `clef` ([#24407](https://github.com/ethereum/go-ethereum/pull/24407))
- Receipt converter tool ([#24028](https://github.com/ethereum/go-ethereum/pull/24028))
- Our builds were updated to use Go 1.18

For a full rundown of the changes please consult the Geth 1.10.17 [release milestone](https://github.com/ethereum/go-ethereum/milestone/129?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).


## [v1.10.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.16) (2022-02-16)

The focus of this release is bugfixes.

#### Bugfixes

- Block tracing via `debug.traceBlockByHash` has sometimes produced inconsistent/corrupt results. Fixed via ([#24286](https://github.com/ethereum/go-ethereum/pull/24286)).
- The `--whitelist` CLI parameter functionality was broken in `v1.10.14`, and is fixed in this release ([#24210](https://github.com/ethereum/go-ethereum/pull/24210)).
- A bug was introduced, and subsequently fixed, which could cause data corruption during mining ([#24349](https://github.com/ethereum/go-ethereum/pull/24349)).
- When signing complex datatypes in EIP712-type data, the signing-hash was incorrect. Fixed via ([#24220](https://github.com/ethereum/go-ethereum/pull/24220)).
- Evm execution times exported via `metrics`, were sometimes incorrect. Fixed in ([#24304](https://github.com/ethereum/go-ethereum/pull/24304)).
- Range prover edgecases found and fixed ([#24266](https://github.com/ethereum/go-ethereum/pull/24266), [#24257](https://github.com/ethereum/go-ethereum/pull/24257)).
- Fix an error related to `HTTP2` handling ([#24292](https://github.com/ethereum/go-ethereum/pull/24292)).
- A lot of spleling-mistkaes and issues related to correctness were fixed ([#24194](https://github.com/ethereum/go-ethereum/pull/24194), [#24196](https://github.com/ethereum/go-ethereum/pull/24196), [#24198](https://github.com/ethereum/go-ethereum/pull/24198), [#24205](https://github.com/ethereum/go-ethereum/pull/24205), [#24207](https://github.com/ethereum/go-ethereum/pull/24207), [#24244](https://github.com/ethereum/go-ethereum/pull/24244), [#24270](https://github.com/ethereum/go-ethereum/pull/24270), [#24271](https://github.com/ethereum/go-ethereum/pull/24271), [#24224](https://github.com/ethereum/go-ethereum/pull/24224), [#24372](https://github.com/ethereum/go-ethereum/pull/24372), [#24323](https://github.com/ethereum/go-ethereum/pull/24323), [#24289](https://github.com/ethereum/go-ethereum/pull/24289), [#24263](https://github.com/ethereum/go-ethereum/pull/24263) and [#24211](https://github.com/ethereum/go-ethereum/pull/24211)).

#### New features

- Work on The Merge includes support for `RANDOM` opcode ([#24141](https://github.com/ethereum/go-ethereum/pull/24141)) and various other internal refactorings ([#24328](https://github.com/ethereum/go-ethereum/pull/24328), [#24280](https://github.com/ethereum/go-ethereum/pull/24280), [#24236](https://github.com/ethereum/go-ethereum/pull/24236), [#23256](https://github.com/ethereum/go-ethereum/pull/23256)).
-  The `devp2p` binary now supports doing `snap/v1` protocol testing against a remote node, which can be used for Hive-testing ([#24276](https://github.com/ethereum/go-ethereum/pull/24276)).
- New diagnostic command to show database metadata ([#23900](https://github.com/ethereum/go-ethereum/pull/23900))
- `ethclient` support for `CallContractAtHash` ([#24355](https://github.com/ethereum/go-ethereum/pull/24355)).
- Support chainId for GnosisSafeTx ([#24231](https://github.com/ethereum/go-ethereum/pull/24231)).


#### Performance

- Tracing was improved by making the `prestate` tracer be a native tracer ([#24320](https://github.com/ethereum/go-ethereum/pull/24320), [#24268](https://github.com/ethereum/go-ethereum/pull/24268)).
- Potentially reduce database allocations in some cases ([#24117](https://github.com/ethereum/go-ethereum/pull/24117)).
- Add a set of cross-client external benchmarks ([#24050](https://github.com/ethereum/go-ethereum/pull/24050)).
- Improve transaction indexing performance ([#24197](https://github.com/ethereum/go-ethereum/pull/24197)).


For a full rundown of the changes please consult the Geth 1.10.16 [release milestone](https://github.com/ethereum/go-ethereum/milestone/128?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.15) (2022-01-05)

This release resolves a few regressions introduced by the previous release. Most importantly, it fixes an issue that could cause peer-to-peer 'eth' connections to lock up. 

Please upgrade ASAP if you are running geth v1.10.12 / .13 / .14.

- A hang in ancient data serving caused by double-locking is fixed. (#24189)
- A crash in the LES server related to reorg handling is resolved. (#24189)
- The SyncProgress method of ethclient.Client works again. (#24199)
- Several inconsistencies in the GraphQL API are also fixed in this release. (#24190, #24188, #24191)

For a full rundown of the changes please consult the Geth 1.10.15 [release milestone](https://github.com/ethereum/go-ethereum/milestone/127?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.14) (2021-12-23)

The focus of this release is bug fixes and performance improvements.

We are especially pleased to announce that this release contains a prototype implementation of the PoW to PoS transition (a.k.a. 'The Merge'). As of this version, Geth is compatible with the Kintsugi testnet spec v3.

We would also like to thank [Team Ipsilon][ti] for their development of the EVM optimizations included in this release. EVM bytecode evaluation is now ~20% faster.

[ti]: https://notes.ethereum.org/@ipsilon/about

#### Geth changes

- A regression in txpool limit handling is resolved. This affects the `--pricelimit` option, which has been reverted to work exactly as it did in geth v1.10.12. ([#24080](https://github.com/ethereum/go-ethereum/pull/24080))
- Geth can now handle the transition from PoW to PoS. ([#23761](https://github.com/ethereum/go-ethereum/pull/23761))
- In the JavaScript console, long-running JS computation (i.e. for/while loops) can now be interrupted with Ctrl-C. ([#23387](https://github.com/ethereum/go-ethereum/pull/23387))
- A corner-case issue in the transaction hash indexer is resolved. ([#24024](https://github.com/ethereum/go-ethereum/pull/24024))
- Unclean shutdown markers are now updated regularly and report more accurate geth startup/shutdown times. ([#24077](https://github.com/ethereum/go-ethereum/pull/24077))
- In log messages related to RPC method invocations, the key "t" is now called "duration" to prevent a name clash when using the JSON output format. ([#24112](https://github.com/ethereum/go-ethereum/pull/24112))

#### RPC API changes

- The `engine` APIs (enabled in `geth --catalyst` mode) are now up-to-date for Kintsugi testnet v3. ([#23984](https://github.com/ethereum/go-ethereum/pull/23984), [#24067](https://github.com/ethereum/go-ethereum/pull/24067), [#24075](https://github.com/ethereum/go-ethereum/pull/24075))
- A panic in the `clique_getSigner` RPC method is resolved. ([#23961](https://github.com/ethereum/go-ethereum/pull/23961))

#### Go library changes

- The EVM implementation has been cleaned up and interpreter loop performance is improved. ([#24120](https://github.com/ethereum/go-ethereum/pull/24120), [#24048](https://github.com/ethereum/go-ethereum/pull/24048), [#24085](https://github.com/ethereum/go-ethereum/pull/24085), [#24026](https://github.com/ethereum/go-ethereum/pull/24026), [#24031](https://github.com/ethereum/go-ethereum/pull/24031), [#24040](https://github.com/ethereum/go-ethereum/pull/24040), [#23970](https://github.com/ethereum/go-ethereum/pull/23970), [#23952](https://github.com/ethereum/go-ethereum/pull/23952), [#23974](https://github.com/ethereum/go-ethereum/pull/23974), [#23977](https://github.com/ethereum/go-ethereum/pull/23977), [#23967](https://github.com/ethereum/go-ethereum/pull/23967), [#24066](https://github.com/ethereum/go-ethereum/pull/24066))
- In preparation for EIP-3670, the EVM now recognizes the INVALID opcode 0xFE. ([#24017](https://github.com/ethereum/go-ethereum/pull/24017))
- Internal opcode names have been modernized to match Solidity: SHA3 is now KECCAK256, SUICIDE is now SELFDESTRUCT. ([#23976](https://github.com/ethereum/go-ethereum/pull/23976), [#24022](https://github.com/ethereum/go-ethereum/pull/24022), [#24016](https://github.com/ethereum/go-ethereum/pull/24016))
- Generating Go/Java bindings for contracts with struct-typed constructor parameters now works correctly. ([#23940](https://github.com/ethereum/go-ethereum/pull/23940))
- Built-in EVM trace loggers have moved from core/vm to a dedicated package. ([#23892](https://github.com/ethereum/go-ethereum/pull/23892))
- EIP-712 (typed data signing) structs have moved from signer/core to package signer/core/apitypes. ([#24029](https://github.com/ethereum/go-ethereum/pull/24029))

#### Networking

- The eth protocol implementation now uses request IDs (added by eth/66) internally. ([#23576](https://github.com/ethereum/go-ethereum/pull/23576))
- Hashing of eth response data now uses multiple threads, improving sync performance. ([#24032](https://github.com/ethereum/go-ethereum/pull/24032))
- The now-unused 2GB fast sync bloom filter has been removed. ([#24047](https://github.com/ethereum/go-ethereum/pull/24047))
- Serving ancient headers to other peers has been optimized. ([#23105](https://github.com/ethereum/go-ethereum/pull/23105))
- The discv4 test suite is more robust and logs received packets better. ([#23966](https://github.com/ethereum/go-ethereum/pull/23966))
- There are now fuzz tests for the snap protocol message handler. ([#23957](https://github.com/ethereum/go-ethereum/pull/23957))

For a full rundown of the changes please consult the Geth 1.10.14 [release milestone](https://github.com/ethereum/go-ethereum/milestone/126?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.13) (2021-11-24)

Geth v1.10.13 is a scheduled maintenance release. It introduces a few polishes, though nothing major. Fixes wise, it does address a few thorns that affect a small portion of our users.

New features: 

- Retrospectively enforce account nonces to be less than `2^64-1` ([#23853](https://github.com/ethereum/go-ethereum/pull/23853)).
- Configurable genesis gas limit in `dev` mode via `dev.gaslimit` ([#23686](https://github.com/ethereum/go-ethereum/pull/23686)).
- Sanitize history config fields in the GPO when using toml ([#23886](https://github.com/ethereum/go-ethereum/pull/23886)).
- Add support for custom freezer paths in `db inspect` ([#23946](https://github.com/ethereum/go-ethereum/pull/23946)).
- Create `evm b11r` to build and seal blocks from json ([#23843](https://github.com/ethereum/go-ethereum/pull/23843)).
- Extend `evm t8n` to support signing unprotected txs ([#23937](https://github.com/ethereum/go-ethereum/pull/23937)).
- Polish `evm t8n` to have more meaningful CLI flags ([#23934](https://github.com/ethereum/go-ethereum/pull/23934)).
- Implement the 4byte tracer natively in Go ([#23882](https://github.com/ethereum/go-ethereum/pull/23882), [#23916](https://github.com/ethereum/go-ethereum/pull/23916)).
- Use faster freezer scanning when reiniting leveldb ([#23612](https://github.com/ethereum/go-ethereum/pull/23612)).
- Expose the `gasUsed` field in the `evm` command ([#23919](https://github.com/ethereum/go-ethereum/pull/23919)).
- Improve error messages in the freezer ([#23901](https://github.com/ethereum/go-ethereum/pull/23901)).

New fixes:

- Fix price filtering in tx pool to prevent low price legacy transaction from spamming the pool ([#23855](https://github.com/ethereum/go-ethereum/pull/23855)).
- Fix log retrievals for users with very old archive nodes having legacy database formats ([#23879](https://github.com/ethereum/go-ethereum/pull/23879)).
- Fix a snap sync issue where a malicious response could crash the syncing node ([#23960](https://github.com/ethereum/go-ethereum/pull/23960)).
- Fix a data race in the simulated backed's gas price suggestion ([#23898](https://github.com/ethereum/go-ethereum/pull/23898)).
- Fix `receiptsRoot` field name in the `evm` command output ([#23924](https://github.com/ethereum/go-ethereum/pull/23924)).
- Fix `setHead` when pointing it back to the genesis ([#23949](https://github.com/ethereum/go-ethereum/pull/23949)).
- Fix transaction sender recovery in `ethclient` ([#23877](https://github.com/ethereum/go-ethereum/pull/23877)).
- Fix DNS discovery entry TTLs on Clouflare ([#23885](https://github.com/ethereum/go-ethereum/pull/23885)).
- Fix `intrinsicGas` output in the `t9n` tool ([#23889](https://github.com/ethereum/go-ethereum/pull/23889)).

For a full rundown of the changes please consult the Geth 1.10.13 [release milestone](https://github.com/ethereum/go-ethereum/milestone/125?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.12) (2021-11-08)

Geth v1.10.12 is a scheduled maintenance release, but also contains some significant features!

**The release enables the [Arrow Glacier hard-fork](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/arrow-glacier.md), scheduled approximately for the 8th of December. The sole change is to [postpone the difficulty-bomb](https://eips.ethereum.org/EIPS/eip-4345) until summer 2022, by which time hopefully The Merge will have happened.**

The release also ships support for a new PoW testnet called *Sepolia*. This testnet was dreamed up during the merge interop in Athens and it's purpose is to replace Ropsten after the merge as the main cross client testnet. You can access it via `geth --sepolia`. Being a PoW testnet, it's possible to mine it for Ether to use as test funds. 

Lastly, the release also contains a brand new call tracer implemented in Go, which should be significantly (**2.5x**) faster than the one currently used. You can use the new tracer via `debug.traceTransaction("0xhash", {tracer: "callTracer"})`. The original JavaScript tracer is still available for fallback purposes called `callTracerLegacy`. The latter will be dropped if nobody reports issues with the native one.

Improvements:

- Implement the Arrow Glacier hard fork and schedule ([#23810](https://github.com/ethereum/go-ethereum/pull/23810)).
- Bake in support for the Sepolia PoW test network ([#23730](https://github.com/ethereum/go-ethereum/pull/23730)).
- Switch the call tracer to a fast native Go implementation ([#23867](https://github.com/ethereum/go-ethereum/pull/23867), [#23708](https://github.com/ethereum/go-ethereum/pull/23708)).
- Optimize nonce handling performance in the txpool ([#22231](https://github.com/ethereum/go-ethereum/pull/22231)).
- Support password protected SSH key files in `puppeth` ([#22148](https://github.com/ethereum/go-ethereum/pull/22148)).
- Optimize request/response matching in RPC batch queries ([#23856](https://github.com/ethereum/go-ethereum/pull/23856)).
- Support transferring snapshots via `geth db export snapshot` ([#22931](https://github.com/ethereum/go-ethereum/pull/22931)).
- Read chain data atomically from ancients/leveldb, avoiding an extra read ([#23566](https://github.com/ethereum/go-ethereum/pull/23566)).
- Improve the `hexutil` package's big-int encoding performance by 50% ([#23780](https://github.com/ethereum/go-ethereum/pull/23780))
- Remove the `xgo` cross compiler as docker auto-build limits killed it ([#23800](https://github.com/ethereum/go-ethereum/pull/23800)).
- Support invalid RLP blobs (at least fail gracefully) in the state `t8n` tool ([#23771](https://github.com/ethereum/go-ethereum/pull/23771)).

Bug-fixes:

- Fix a crash in LES serving code ([#23865](https://github.com/ethereum/go-ethereum/pull/23865)).
- Fix a data race in the miner's receipt copying code ([#23835](https://github.com/ethereum/go-ethereum/pull/23835)).
- Fix a missing snapshot error after recovering from a crash ([#23496](https://github.com/ethereum/go-ethereum/pull/23496)).
- Fix a memory leak in Clique if the network temporarilly halts ([#23861](https://github.com/ethereum/go-ethereum/pull/23861)).
- Fix a crash if the disk gets full during ethash DAG generation ([#23799](https://github.com/ethereum/go-ethereum/pull/23799)).
- Fix chain tracing to not go OOM during long running sessions ([#23736](https://github.com/ethereum/go-ethereum/pull/23736)).
- Fix the simulated backend to allow running EIP-1559 transactions ([#23838](https://github.com/ethereum/go-ethereum/pull/23838), [#23840](https://github.com/ethereum/go-ethereum/pull/23840)).
- Fix an RPC crash when getting the signer of an empty Clique chain ([#23832](https://github.com/ethereum/go-ethereum/pull/23832)).
- Fix the total difficulty number of nil-diff genesis blocks in the database ([#23793](https://github.com/ethereum/go-ethereum/pull/23793)).
- Fix a crash in `abigen` generated code if backend header retrieval fails ([#23781](https://github.com/ethereum/go-ethereum/pull/23781)).

For a full rundown of the changes please consult the Geth 1.10.12 [release milestone](https://github.com/ethereum/go-ethereum/milestone/124?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.11) (2021-10-20)

Geth v1.10.11 is another bug fix release, fixing an issue with sender not being recovered on pending transactions, and fixing a data corruption issue. 

#### Changes in this release

- For pending transactions returned by RPC, the sender address is again reported correctly. This was broken in the previous release. ([#23765](https://github.com/ethereum/go-ethereum/pull/23765))
- The rlpdump command can now turn structured text into RLP. ([#23745](https://github.com/ethereum/go-ethereum/pull/23745))
- A database corruption issue caused by the snapshot system is resolved. ([#23635](https://github.com/ethereum/go-ethereum/pull/23635))
- The evm tool's t9n mode performs even stricter transaction validation. ([#23743](https://github.com/ethereum/go-ethereum/pull/23743))
- You can now use line editing at the puppeth prompt. ([#23718](https://github.com/ethereum/go-ethereum/pull/23718))
- The `geth db` subcommands now accept (non-hex) string keys. ([#23744](https://github.com/ethereum/go-ethereum/pull/23744))


For a full rundown of the changes please consult the Geth 1.10.11 [release milestone](https://github.com/ethereum/go-ethereum/milestone/123?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.10) (2021-10-15)

Geth v1.10.10 is another bug fix release.

#### Geth changes

- Geth is much less likely to crash during shutdown, especially when mining is active. ([#23435](https://github.com/ethereum/go-ethereum/pull/23435), [#21992](https://github.com/ethereum/go-ethereum/pull/21992), [#22853](https://github.com/ethereum/go-ethereum/pull/22853))
- The new `--rpc.evmtimeout` flag allows setting the internal timeout for `eth_call`. The default timeout is still 5s. ([#23645](https://github.com/ethereum/go-ethereum/pull/23645))
- The geth console supports some ECMAScript 6 features like arrow functions, typed arrays and let bindings ([#23721](https://github.com/ethereum/go-ethereum/pull/23721))
- The console no longer crashes when trying to complete on properties with value 'null' or 'undefined'. ([#23701](https://github.com/ethereum/go-ethereum/pull/23701))
- The evm debugging/testing tool now validates transaction gas limits in 't9n' mode. ([#23694](https://github.com/ethereum/go-ethereum/pull/23694))

#### RPC API changes

- A regression in the JS-based call tracer is resolved. ([#23667](https://github.com/ethereum/go-ethereum/pull/23667))
- The new `debug_getAccessibleState` RPC method finds a block number at which full state is available. ([#23646](https://github.com/ethereum/go-ethereum/pull/23646))
- The new `debug_getHeaderRlp` RPC method fetches RLP-encoded headers from the database. ([#23670](https://github.com/ethereum/go-ethereum/pull/23670), [#23677](https://github.com/ethereum/go-ethereum/pull/23677))
- The sender address is once again returned correctly for very old Frontier-era transactions. ([#23683](https://github.com/ethereum/go-ethereum/pull/23683))

#### Go library changes

- For contract calls using accounts/abi/bind, a regression that could lead to incorrect gas estimation is fixed. ([#23719](https://github.com/ethereum/go-ethereum/pull/23719))
- Package accounts/abi now has basic support for Solidity error types. ([#23161](https://github.com/ethereum/go-ethereum/pull/23161))
- Miner stress test tools work again (they were broken in the previous release) ([#23699](https://github.com/ethereum/go-ethereum/pull/23699))
- The transaction recipient address stored in types.Transaction is now truly independent of the address pointer passed to the constructor. ([#23376](https://github.com/ethereum/go-ethereum/pull/23376))
- The Receipt type now implements encoding.BinaryMarshaler, like Transaction ([#22806](https://github.com/ethereum/go-ethereum/pull/22806))
- TxPool.Pending no longer returns an error ([#23720](https://github.com/ethereum/go-ethereum/pull/23720))

#### Build

- As a workaround for tracing issues on Alpine Linux, we now set the C stack size to 8MB for release builds. ([#23676](https://github.com/ethereum/go-ethereum/pull/23676))
- Go module vendoring issues related to github.com/karalable/usb are finally resolved. ([#23684](https://github.com/ethereum/go-ethereum/pull/23684))
- This release is built with Go 1.17.2. ([#23698](https://github.com/ethereum/go-ethereum/pull/23698))

For a full rundown of the changes please consult the Geth 1.10.10 [release milestone](https://github.com/ethereum/go-ethereum/milestone/122?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.9) (2021-09-29)

Geth v1.10.9 is a maintenance release containing mostly bug fixes.

Chain tracing has received quite a bit of attention during this release cycle. JS-based tracing now supports additional callbacks for entry and exit of contract calls, improving performance if processing individual opcodes is not needed.

#### Geth command changes

- The 'evm' command has a new subcommand for testing tx decoding. ([#23494](https://github.com/ethereum/go-ethereum/pull/23494))
- 'evm t8n' now calculates and returns block difficulty. ([#23353](https://github.com/ethereum/go-ethereum/pull/23353), [#23507](https://github.com/ethereum/go-ethereum/pull/23507))
- Legacy flags `--rpc`, `--rpcaddr`, `--rpcport`, etc. are no longer supported. ([#23358](https://github.com/ethereum/go-ethereum/pull/23358))
- Legacy debugging flags `--pprofport`, `--pprofaddr`, etc. are also no longer supported. ([#23368](https://github.com/ethereum/go-ethereum/pull/23368))
- When initializing Clique-based private networks, zero-length extradata in genesis.json now prints an error message instead of crashing. ([#23538](https://github.com/ethereum/go-ethereum/pull/23538))

#### Go library changes

- Contract bindings created by accounts/abi/bind now validate log event signatures. This prevents accidentally decoding events with the wrong signature. ([#23230](https://github.com/ethereum/go-ethereum/pull/23230))
- A crash in accounts/abi when decoding struct-typed Solidity return values is resolved. ([#23573](https://github.com/ethereum/go-ethereum/pull/23573))
- Writes to the ancient database are now batched internally for improved performance. ([#23462](https://github.com/ethereum/go-ethereum/pull/23462))
- Multiple data races in transaction pool code are fixed. ([#23474](https://github.com/ethereum/go-ethereum/pull/23474))
- Comprehensive benchmarks for RLP encoding/decoding of consensus types have been added. ([#23190](https://github.com/ethereum/go-ethereum/pull/23190))
- RLP encoding of slices and arrays is slightly faster. ([#23467](https://github.com/ethereum/go-ethereum/pull/23467))
- `rpc.BlockNumber` now implements encoding.TextMarshaler. ([#23324](https://github.com/ethereum/go-ethereum/pull/23324))
- The `Account` type has been moved from package core/state to core/types. ([#23567](https://github.com/ethereum/go-ethereum/pull/23567))
- For crypto/cloudflare/bn256 EC curve, in-place addition and unmarshalling now works correctly. ([#23419](https://github.com/ethereum/go-ethereum/pull/23419))
- A very rare crash in the background 'bloombits' indexer is resolved. ([#23437](https://github.com/ethereum/go-ethereum/pull/23437))

#### RPC/GraphQL changes

- JS tracing of EVM execution now provides additional callbacks for call entry/exit. Using these callbacks instead of 'step' can yield a 10-100x tracing speedup if you don't need to process every VM opcode. ([#23087](https://github.com/ethereum/go-ethereum/pull/23087))
- The '4byte' built-in tracer now uses enter/exit. ([#23622](https://github.com/ethereum/go-ethereum/pull/23622))
- A state database corruption bug caused by tracer re-execution of old blocks is resolved. ([#23632](https://github.com/ethereum/go-ethereum/pull/23632))
- The new `debug_intermediateRoots` method computes per-transaction state roots of a block. ([#23594](https://github.com/ethereum/go-ethereum/pull/23594))
- EVM memory and return data are no longer captured by default when tracing. ([#23558](https://github.com/ethereum/go-ethereum/pull/23558))
- EVM execution is now aborted on the server side when tracing is interrupted. ([#23580](https://github.com/ethereum/go-ethereum/pull/23580))
- Broken WebSocket connections are now detected better and their subscriptions report an error instead of hanging indefinitely. ([#23556](https://github.com/ethereum/go-ethereum/pull/23556))
- `personal_sendTransaction` now supports both "input" and "data" arguments, just like `eth_sendTransaction`. ([#23476](https://github.com/ethereum/go-ethereum/pull/23476))
- Log filtering performance is improved. ([#23147](https://github.com/ethereum/go-ethereum/pull/23147))
- Transaction access lists returned by GraphQL are now correct. ([#23650](https://github.com/ethereum/go-ethereum/pull/23650))
- The `debug_stacks` method now supports an optional filter expression. ([#23605](https://github.com/ethereum/go-ethereum/pull/23605))
- For clique blocks returned by RPC, the "miner" field once again contains the actual block coinbase field instead of the derived block signer. This fixes a regression where clients would no longer be able to verify the block seal signature. ([#23466](https://github.com/ethereum/go-ethereum/pull/23466))

#### Networking

- The eth/65 peer-to-peer protocol is no longer supported. Geth only supports eth/66 as of this release. ([#23456](https://github.com/ethereum/go-ethereum/pull/23456))
- The cross-client eth protocol tests suite better distinguishes eth/65 and eth/66. ([#23568](https://github.com/ethereum/go-ethereum/pull/23568))
- ENR sequence numbers are now initialized as a timestamp. This prevents issues when the p2p nodes database is dropped/re-created while keeping the nodekey the same. ([#19903](https://github.com/ethereum/go-ethereum/pull/19903))
- Several data races are resolved in packages p2p and p2p/enode. ([#23434](https://github.com/ethereum/go-ethereum/pull/23434))
- Note: to simplify the ongoing rewrite of eth/downloader, the package has been duplicated temporarily. The additional copy will be removed later. ([#23561](https://github.com/ethereum/go-ethereum/pull/23561))

#### Build

- This release is built with Go 1.17. ([#23464](https://github.com/ethereum/go-ethereum/pull/23464), [#23465](https://github.com/ethereum/go-ethereum/pull/23465), [#23468](https://github.com/ethereum/go-ethereum/pull/23468))
- 32 bit builds of Geth should be fully functional again. ([#23543](https://github.com/ethereum/go-ethereum/pull/23543), [#23542](https://github.com/ethereum/go-ethereum/pull/23542))
- We no longer publish .deb packages for Ubuntu 20.10 Gorilla because this version is not supported by Launchpad anymore. ([#23470](https://github.com/ethereum/go-ethereum/pull/23470))
- The 'node' package no longer depends on wallet backends. Specifically, this removes the dependencies on libusb for contract bindings and other uses of the go-ethereum library. If you are using package node, you must now register required account manager backends individually. ([#23019](https://github.com/ethereum/go-ethereum/pull/23019))
- The 'metrics' package, and many packages that depend on it can now be compiled for WebAssembly. ([#23449](https://github.com/ethereum/go-ethereum/pull/23449))
- EVM performance tests no longer run on CI. ([#23304](https://github.com/ethereum/go-ethereum/pull/23304))

For a full rundown of the changes please consult the Geth 1.10.9 [release milestone](https://github.com/ethereum/go-ethereum/milestone/121?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.8) (2021-08-24)

Geth v1.10.8 is a pre-announced ***hotfix release to patch a vulnerability in the EVM*** (`CVE-2021-39137`).

The exact attack vector will be provided at a later date to give node operators and dependent downstream projects time to update their nodes and software. All Geth versions supporting the London hard fork are vulnerable (the bug is older than London), so all users should update.

Credits for the discovery go to @guidovranken (working for [Sentnl](https://sentnl.io/) during an audit of the [Telos EVM](https://www.telos.net/evm)) and reported via bounty@ethereum.org.

Beside the fix, we're merged in a few tiny polishes and fixes. For a rundown, please consult the Geth 1.10.8 [release milestone](https://github.com/ethereum/go-ethereum/milestone/120?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.7) (2021-08-12)

Geth v1.10.7 is a maintenance release, mostly focusing on a few post-London polishes.

**A few important notes to keep in mind:**

- **The return type for `oldestBlock` in `eth_feeHistory` was changed from decimal to hex.** This is to conform to the updated spec that was released after Geth's London hard-fork release was already made. The input `blockCount` parameter was also updated, but there Geth will accept both hex and decimal to keep backward compatibility.
- **The `-miner.gastarget` CLI flag was deprecated and is a noop.** This flag is already a noop for networks running the London hard-fork, since it London miners only take into account the `-miner.gaslimit` flag. For non-London private networks and Geth forks, this might result in a gas bump depending on how the miners are configured.
- Docker builds were changed from DockerHub Automated Builds to offsite builds and manual pushes to DockerHub. At the same time, we've added support for multi-arch images, the original tags being the metadata image, linking a `-amd64` and a `-arm64` tags together. No changes are needed for docker users, but keep us posted if something strange happens. On the upside, Geth now has official `arm64` docker images too.

Changes made:

- Change the `oldestBlock` return type in `eth_feeHistory` to hex, accept both decimal and hex as the block count ([#23239](https://github.com/ethereum/go-ethereum/pull/23239), [#23363](https://github.com/ethereum/go-ethereum/pull/23363)). 
- Cap max usable gas in `eth_estimateGas` better for 1559 transactions ([#23309](https://github.com/ethereum/go-ethereum/pull/23309)).
- When deploying multiple contracts via abigen, only parse the ABI once ([#22583](https://github.com/ethereum/go-ethereum/pull/22583)).
- Return `maxFeePerGas` for the `gasPrice` of pending transactions ([#23345](https://github.com/ethereum/go-ethereum/pull/23345)).
- Check cached blocks too when attempting to retrieve a header ([#23299](https://github.com/ethereum/go-ethereum/pull/23299)).
- Reject transactions imitated from non EOA accounts ([#23303](https://github.com/ethereum/go-ethereum/pull/23303)).
- Reduce allocations a bit while CPU mining ethash ([#23199](https://github.com/ethereum/go-ethereum/pull/23199)).
- Deprecate the `-miner.gastarget` CLI flag ([#23213](https://github.com/ethereum/go-ethereum/pull/23213)).
- Switch over to manual docker pushes ([#23373](https://github.com/ethereum/go-ethereum/pull/23373)).

Bugs fixed:

- Fix a `nil` pointer panic for certain abigen generated code due to missing context initialization ([#23188](https://github.com/ethereum/go-ethereum/pull/23188)).
- Fix ` nil` pointer panic in certain automatic access list generation RPC API calls ([#23225](https://github.com/ethereum/go-ethereum/pull/23225)).
- Fix a regression that prevented `clef` from signing a legacy transaction ([#23274](https://github.com/ethereum/go-ethereum/pull/23274)).
- Fix a permission error during snapshot based pruning on Windows ([#23370](https://github.com/ethereum/go-ethereum/pull/23370)).
- Fix the marshaling of errors from the tracers ([#23292](https://github.com/ethereum/go-ethereum/pull/23292)).

For a full rundown of the changes please consult the Geth 1.10.7 [release milestone](https://github.com/ethereum/go-ethereum/milestone/119?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.6) (2021-07-22)

Geth v1.10.6 is a hotfix release. This resolves a consensus failure on the Ropsten testnet.

**Users of Geth on the Ethereum mainnet must upgrade to this release before the London hard-fork activates to remain in consensus.** Reminder: the London hard fork is scheduled to occur at block #12965000 on mainnet (~ August 4th, 2021).

#### More information about the Ropsten incident

During testing of the London hardfork on Ropsten, a consensus failure occurred in block 10679538, leading to a network split between OpenEthereum/Besu and Geth/Nethermind. The block contained a transaction from an account with enough funds to cover the effective fee, but too little funds for the transaction's maximum gas price. EIP-1559 mandates that such transactions should be rejected. Geth's implementation of EIP-1559 did not perform the check correctly and accepted the transaction.

For more information see [PR #23244](https://github.com/ethereum/go-ethereum/pull/23244) and the [post-mortem writeup](https://notes.ethereum.org/@timbeiko/ropsten-postmortem).

#### Other changes in this release

- Compatibility with old receipt formats stored in the database is restored. This fixes a regression introduced in Geth v1.10.4 for people with very old chain databases. ([#23247](https://github.com/ethereum/go-ethereum/pull/23247))
- A regression for `eth_sendTransaction` in light client mode is fixed. ([#23215](https://github.com/ethereum/go-ethereum/pull/23215))
- The Go API function `Node.Close()` has been fixed to stop the WebSocket server correctly ([#23211](https://github.com/ethereum/go-ethereum/pull/23211))

For a full rundown of the changes please consult the Geth 1.10.6 [release milestone](https://github.com/ethereum/go-ethereum/milestone/118?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.5) (2021-07-14)

**Geth v1.10.5 enables the London hard fork for the Ethereum mainnet at block `#12965000`**, estimated for the 4th of August, 2021. Reiterating the London hardfork summary from our previous release (note, extended):

#### London Fork EIPs

- [EIP-1559]: Fee market change for ETH 1.0 chain ([#22837](https://github.com/ethereum/go-ethereum/pull/22837), [#22888](https://github.com/ethereum/go-ethereum/pull/22888), [#22970](https://github.com/ethereum/go-ethereum/pull/22970))
- [EIP-3198]: BASEFEE opcode (included in EIP-1559 changes)
- [EIP-3529]: Reduction in refunds ([#22733](https://github.com/ethereum/go-ethereum/pull/22733))
- [EIP-3541]: Reject new contracts starting with the 0xEF byte ([#22809](https://github.com/ethereum/go-ethereum/pull/22809))
- [EIP-3554]: Difficulty Bomb Delay to December 1st 2021 ([#22840](https://github.com/ethereum/go-ethereum/pull/22840), [#22870](https://github.com/ethereum/go-ethereum/pull/22870))

#### Additional notes about the London changes

- This release contains mainnet activation block number for the London hard fork. ([#23176](https://github.com/ethereum/go-ethereum/pull/23176))
- As with all previous fork-related releases, we have added an override flag which can be used to set the activation block. This flag is temporary and will be removed some time after the fork has successfully activated on mainnet. ([#22822](https://github.com/ethereum/go-ethereum/pull/22822), [#22972](https://github.com/ethereum/go-ethereum/pull/22972))
- The Geth transaction pool has been redesigned in order to handle the new fee market created by EIP-1559. Our new pool design attempts to cater to the demands of users—timely inclusion of transactions—as well as allowing efficient ordering of transactions by their effective mining reward. You can read more about how the new pool functions in the [transaction pool design document]. ([#22898](https://github.com/ethereum/go-ethereum/pull/22898))
- **For miners:** The transaction selection algorithm provided by Geth specifically picks transactions with the highest effective reward. If a minimum price is configured using the `--miner.gasprice` command-line flag, transactions providing less miner tips will not be included in blocks. ([#22896](https://github.com/ethereum/go-ethereum/pull/22896), [#22995](https://github.com/ethereum/go-ethereum/pull/22995))

  EIP-1559 also changes the gas limit voting system. After the London fork, the block gas amount available for transactions is adjusted based on demand. The block capacity is called the *gas target*, and EIP-1559 defines this target as half the gas limit.

  To ensure that the gas available for transactions is the same as before the fork, the gas limit is doubled at the fork block. If you are using the `--miner.gaslimit` flag to participate in voting, you need to double the value of this flag to keep voting for the same value as before. You can use the `miner_setGasLimit` RPC API to update the target without downtime, but be aware that this does not survive a restart. The previous `--miner.gastarget` flag is deprecated post London and its value will be ignored.

  Example: You are using `--miner.gaslimit` to vote for a limit of 20M, and the actual block gas limit is 20M. When London activates, the block gas limit will adjust to 40M, but you will still be voting it down towards to 20M if you keep using the same `--miner.gaslimit` setting. So at some point after the fork, you need to double your `--miner.gaslimit` value to ensure the gas limit stays at 40M gas.
- **For wallet providers:** The default transaction price calculation algorithm for EIP-1559 (`eth_maxPriorityFeePerGas`) follows the old mechanism, setting the `max priority fee` to the `effective price` paid on the network minus the current `base fee`; and setting the `max fee` to the `priority fee + 2x base fee`. This ensures that the total price paid per gas remains the same after the London transition if no explicit user involvement has been made.

  Alternatively, Geth exposes a new [`eth_feeHistory(blocks, head, percentiles)`](https://github.com/ethereum/eth1.0-specs/blob/b4ebe3d6056a2f5edb6f9411da58e042f7a95d2a/json-rpc/spec.json#L200) API endpoint which allows the user to query recent statistical infos about the amount of tips paid to miners and fees burned by transactions. Wallets are recommended to use this endpoint to give users multiple fee options to choose from ([#23033](https://github.com/ethereum/go-ethereum/pull/23033)).

- **Note for JSON-RPC users:** `eth_sendTransaction` and `eth_fillTransaction` will create EIP-1559 transactions by default after the fork has activated.
- **Note for users of Go/Java/ObjC contract bindings:** `accounts/abi/bind` will create EIP-1559 transactions automatically after the fork. To take advantage of EIP-1559 in Go applications, please remember to update the go-ethereum module dependency to v1.10.4 or newer in your application's `go.mod` file. ([#23038](https://github.com/ethereum/go-ethereum/pull/23038))
- **Note for users of ethclient:** If you send transactions using the ethclient package and want to take advantage of the new fee model provided by EIP-1559, you must adapt your code to create transactions using `types.NewTx(&types.DynamicFeeTx{...})`. In order to know whether the fork has activated and the new transaction type can be used yet, simply check whether the `BaseFee` field of the latest block header is non-nil.

Other changes in this release:

- Expose contextual infos (block/tx hash, tx index) into the transaction tracer ([#23104](https://github.com/ethereum/go-ethereum/pull/23104), [#23108](https://github.com/ethereum/go-ethereum/pull/23108)).
- Implement fee history API for better 1559 transaction price suggestions ([#23033](https://github.com/ethereum/go-ethereum/pull/23033), [#23178](https://github.com/ethereum/go-ethereum/pull/23178)).
- Implement `clique_getSinger` API for deriving the miner on Clique neworks ([#22987](https://github.com/ethereum/go-ethereum/pull/22987)).
- Implement `txpool_contentFrom` API for retrieving txs of a single account ([#22992](https://github.com/ethereum/go-ethereum/pull/22992)).
- Implement `miner_setGasPrice` API modify the mining gas limit on the fly ([#23134](https://github.com/ethereum/go-ethereum/pull/23134)).
- Create new `gethclient` package for accessing Geth specific RPC APIs. ([#22977](https://github.com/ethereum/go-ethereum/pull/22977)).
- Increase the downloader's scratch space to better saturate fast links ([#23074](https://github.com/ethereum/go-ethereum/pull/23074)).
- Introduce a mechanism to deprecate config file fields without errors ([#23118](https://github.com/ethereum/go-ethereum/pull/23118)).
- Remove the notions of a block hash from the state db ([#23126](https://github.com/ethereum/go-ethereum/pull/23126)).
- Improve opcode tracing speed by around 80% ([#23016](https://github.com/ethereum/go-ethereum/pull/23016)).
- Sanity check the length of the baseFee field ([#23171](https://github.com/ethereum/go-ethereum/pull/23171)).
- Shorter shutdown time for the trie syncer ([#23020](https://github.com/ethereum/go-ethereum/pull/23020)).
- Avoid some memory allocations in Clique ([#23149](https://github.com/ethereum/go-ethereum/pull/23149)).
- Better build constraints for the fuzzers ([#23089](https://github.com/ethereum/go-ethereum/pull/23089), [#23137](https://github.com/ethereum/go-ethereum/pull/23137)).
- Alternate builders for docker images ([#23069](https://github.com/ethereum/go-ethereum/pull/23069), [#23078](https://github.com/ethereum/go-ethereum/pull/23078), [#23082](https://github.com/ethereum/go-ethereum/pull/23082), [#23083](https://github.com/ethereum/go-ethereum/pull/23083)).
- Remove `make` as a Dockerfile dependency ([#23167](https://github.com/ethereum/go-ethereum/pull/23167)).
- Remove the deprecated `LogforStorage` type ([#23173](https://github.com/ethereum/go-ethereum/pull/23173)).

And of course, the various fixes:

- Fix a panic in the access list creation RPC API ([#23133](https://github.com/ethereum/go-ethereum/pull/23133)).
- Fix transaction queries in GraphQL when backed by a light client ([#23052](https://github.com/ethereum/go-ethereum/pull/23052)).
- Fix the tracer to correctly decide if a contract is a precompile or not ([#23097](https://github.com/ethereum/go-ethereum/pull/23097)).
- Fix transaction submission for the `personal` namespace post 1559 ([#23179](https://github.com/ethereum/go-ethereum/pull/23179)).
- Fix an ethstats regression that caused transaction counts to not report ([#23159](https://github.com/ethereum/go-ethereum/pull/23159)).
- Fix a compatibility issue between old Geth nodes and new abigen code ([#23102](https://github.com/ethereum/go-ethereum/pull/23102)).
- Fix a light client hang if the chain is reverted to before the trusted CHT ([#23162](https://github.com/ethereum/go-ethereum/pull/23162)).
- Fix incorrect file permissions for the transaction pool local journal ([#23090](https://github.com/ethereum/go-ethereum/pull/23090)).
- Fix a context error when calling certain APIs with invalid params ([#23062](https://github.com/ethereum/go-ethereum/pull/23062)).
- Fix `puppeth` dashboard caused by an updated base image ([#23168](https://github.com/ethereum/go-ethereum/pull/23168)).
- Fix a shutdown hang in light client mode ([#23139](https://github.com/ethereum/go-ethereum/pull/23139)).

For a full rundown of the changes please consult the Geth 1.10.5 [release milestone](https://github.com/ethereum/go-ethereum/milestone/117?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

[transaction pool design document]: https://gist.github.com/zsfelfoldi/9607ad248707a925b701f49787904fd6
[EIP-1559]: https://eips.ethereum.org/EIPS/eip-1559
[EIP-3198]: https://eips.ethereum.org/EIPS/eip-3198
[EIP-3529]: https://eips.ethereum.org/EIPS/eip-3529
[EIP-3541]: https://eips.ethereum.org/EIPS/eip-3541
[EIP-3554]: https://eips.ethereum.org/EIPS/eip-3554

## [v1.10.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.4) (2021-06-17)

Geth v1.10.4 is a feature release and adds compatibility with the upcoming London hard fork. For more information about the content and scheduling of the fork, please check the [London specification document].

After a long R&D process, and extensive testing, we are pleased to announce that Geth v1.10.4 enables [snap sync] by default. At the time of this release, geth is able to fully synchronize the Ethereum mainnet in approximately 7 hours (using AWS i3.2xlarge with NVMe SSD).

#### London Fork EIPs

- [EIP-1559]: Fee market change for ETH 1.0 chain ([#22837](https://github.com/ethereum/go-ethereum/pull/22837), [#22888](https://github.com/ethereum/go-ethereum/pull/22888), [#22970](https://github.com/ethereum/go-ethereum/pull/22970))
- [EIP-3198]: BASEFEE opcode (included in EIP-1559 changes)
- [EIP-3529]: Reduction in refunds ([#22733](https://github.com/ethereum/go-ethereum/pull/22733))
- [EIP-3541]: Reject new contracts starting with the 0xEF byte ([#22809](https://github.com/ethereum/go-ethereum/pull/22809))
- [EIP-3554]: Difficulty Bomb Delay to December 1st 2021 ([#22840](https://github.com/ethereum/go-ethereum/pull/22840), [#22870](https://github.com/ethereum/go-ethereum/pull/22870))

#### Additional notes about the London changes

- This release contains testnet activation block numbers for the London hard fork. The numbers are: Ropsten - 10499401, Goerli - 5062605, Rinkeby - 8897988. ([#23041](https://github.com/ethereum/go-ethereum/pull/23041))
- This release does not contain automatic activation of the fork on mainnet. A release containing the mainnet activation block will be provided later, following the ACD decision.
- As with all previous fork-related releases, we have added an override flag which can be used to set the activation block. This flag is temporary and will be removed some time after the fork has successfully activated on mainnet. ([#22822](https://github.com/ethereum/go-ethereum/pull/22822), [#22972](https://github.com/ethereum/go-ethereum/pull/22972))
- The Geth transaction pool has been redesigned in order to handle the new fee market created by EIP-1559. Our new pool design attempts to cater to the demands of users—timely inclusion of transactions—as well as allowing efficient ordering of transactions by their effective mining reward. You can read more about how the new pool functions in the [transaction pool design document]. ([#22898](https://github.com/ethereum/go-ethereum/pull/22898))
- **For miners:** The transaction selection algorithm provided by geth specifically picks transactions with the highest effective reward. If a minimum price is configured using the `--miner.gasprice` command-line flag, transactions providing less fees will not be included in blocks. ([#22896](https://github.com/ethereum/go-ethereum/pull/22896), [#22995](https://github.com/ethereum/go-ethereum/pull/22995))

  EIP-1559 also changes the gas limit voting system. After the London fork, the block gas amount available for transactions is adjusted based on demand. The block capacity is called the *gas target*, and EIP-1559 defines this target as half the gas limit.

  To ensure that the gas available for transactions is the same as before the fork, the gas limit is doubled at the fork block. If you are using the `--miner.gaslimit` flag to participate in voting, you need to double the value of this flag to keep voting for the same value as before.

  Example: You are using `--miner.gaslimit` to vote for a limit of 20M, and the actual block gas limit is 20M. When London activates, the block gas limit will adjust to 40M, but you will still be voting it down towards to 20M if you keep using the same `--miner.gaslimit` setting. So at some point after the fork, you need to double your `--miner.gaslimit` value to ensure the gas limit stays at 40M gas.
- **Note for JSON-RPC users:** `eth_sendTransaction` and `eth_fillTransaction` will create EIP-1559 transactions by default after the fork has activated.
- **Note for users of Go/Java/ObjC contract bindings:** `accounts/abi/bind` will create EIP-1559 transactions automatically after the fork. To take advantage of EIP-1559 in Go applications, please remember to update the go-ethereum module dependency to v1.10.4 in your application's `go.mod` file. ([#23038](https://github.com/ethereum/go-ethereum/pull/23038))
- **Note for users of ethclient:** If you send transactions using the ethclient package and want to take advantage of the new fee model provided by EIP-1559, you must adapt your code to create transactions using `types.NewTx(&types.DynamicFeeTx{...})`. In order to know whether the fork has activated and the new transaction type can be used yet, simply check whether the `BaseFee` field of the latest block header is non-nil.

#### Command changes

- For private networks and future public testnets, the initial EIP-1559 basefee can also be set in genesis.json using the `baseFeePerGas` key. ([#23013](https://github.com/ethereum/go-ethereum/pull/23013), [#23039](https://github.com/ethereum/go-ethereum/pull/23039))
- The Clef tool also supports signing EIP-1559 transactions. ([#22966](https://github.com/ethereum/go-ethereum/pull/22966))
- `geth --dev` no longer allocates excessive amounts of memory on startup. ([#22949](https://github.com/ethereum/go-ethereum/pull/22949))
- The geth `--ethstats` option now supports special characters such as `@` in the node name portion of the URL, specifically so you can put your Twitter handle in there. ([#21640](https://github.com/ethereum/go-ethereum/pull/21640))
- `geth db inspect` now properly counts internal config data stored in leveldb instead of warning about it being 'unaccounted'. ([#22978](https://github.com/ethereum/go-ethereum/pull/22978))
- There is a new `geth snapshot dump` command for debugging purposes. ([#22795](https://github.com/ethereum/go-ethereum/pull/22795))
- Geth can no longer read databases created before April 2019. ([#22852](https://github.com/ethereum/go-ethereum/pull/22852))
- The `--jspath` flag can now expand `~` to the home directory. ([#22900](https://github.com/ethereum/go-ethereum/pull/22900))
- Puppeth can no longer deploy the wallet web page for you. ([#22940](https://github.com/ethereum/go-ethereum/pull/22940))
- The evm tool now exits in error if JSON data provided on stdin is invalid. ([#22871](https://github.com/ethereum/go-ethereum/pull/22871))

#### RPC/GraphQL changes

- JSON-RPC and GraphQL APIs have been extended for EIP-1559, following the [official OpenRPC specification]. ([#22964](https://github.com/ethereum/go-ethereum/pull/22964), [#23010](https://github.com/ethereum/go-ethereum/pull/23010), [#23028](https://github.com/ethereum/go-ethereum/pull/23028), [#23027](https://github.com/ethereum/go-ethereum/pull/23027), [#23050](https://github.com/ethereum/go-ethereum/pull/23050))
- You can now configure the lower bound of gas prices returned by `eth_gasPrice` using the new `--gpo.ignoreprice` command-line flag of geth. ([#22752](https://github.com/ethereum/go-ethereum/pull/22752))
- JS tracing via `debug_traceTransaction` and `debug_traceBlockByNumber` will no longer crash geth if the tracer function result object exceeds duktape's JSON object limits. ([#22857](https://github.com/ethereum/go-ethereum/pull/22857))
- The internal argument object representations of `eth_call`, `eth_sendTransaction`, `eth_estimateGas` have been unified because their fields are very similar. This change should not lead to any differences in behavior, but do let us know via GitHub issues if you find any new argument handling bugs in those methods. ([#22718](https://github.com/ethereum/go-ethereum/pull/22718), [#22942](https://github.com/ethereum/go-ethereum/pull/22942))
- The 'catalyst' API handler now properly reverts the effects of failed transactions when creating a block. ([#22989](https://github.com/ethereum/go-ethereum/pull/22989))

#### Networking

- Snap sync is enabled by default! ([#22973](https://github.com/ethereum/go-ethereum/pull/22973))
- Snap sync now tracks peer latency/capacity and adjusts request sizes accordingly.
  ([#22876](https://github.com/ethereum/go-ethereum/pull/22876), [#22943](https://github.com/ethereum/go-ethereum/pull/22943))
- The RLPx wire protocol implementation has been optimized, reducing memory allocations and system call load. ([#22899](https://github.com/ethereum/go-ethereum/pull/22899))
- The eth protocol cross-client test suite has been further extended and is more reliable. ([#22843](https://github.com/ethereum/go-ethereum/pull/22843), [#22535](https://github.com/ethereum/go-ethereum/pull/22535), [#22957](https://github.com/ethereum/go-ethereum/pull/22957))
- Spurious warning logs about failure to 'unregister' eth peers are gone now. ([#22908](https://github.com/ethereum/go-ethereum/pull/22908))
- DNS discovery no longer crashes when geth is started and immediately shut down again. ([#22906](https://github.com/ethereum/go-ethereum/pull/22906))
- A long-standing issue in the validation of eth/64 fork IDs is resolved. ([#22879](https://github.com/ethereum/go-ethereum/pull/22879))

#### Go library changes

- Package rlp now supports optional struct fields. This feature was added to simplify the EIP-1559 implementation, but is also generally useful in other contexts. ([#22832](https://github.com/ethereum/go-ethereum/pull/22832), [#22842](https://github.com/ethereum/go-ethereum/pull/22842))
- RLP encoding/decoding has been optimized for reduced memory allocations and better performance under high concurrency. ([#22858](https://github.com/ethereum/go-ethereum/pull/22858), [#22902](https://github.com/ethereum/go-ethereum/pull/22902), [#22924](https://github.com/ethereum/go-ethereum/pull/22924), [#22927](https://github.com/ethereum/go-ethereum/pull/22927), [#22841](https://github.com/ethereum/go-ethereum/pull/22841))
- On shutdown, the database now waits for background ancient store writes to finish, fixing database corruption issues related to a gap in the chain between ancients and leveldb. ([#22878](https://github.com/ethereum/go-ethereum/pull/22878))
- The consensus test runner and ethereum/tests submodule reference have been updated for London. ([#22976](https://github.com/ethereum/go-ethereum/pull/22976), [#23047](https://github.com/ethereum/go-ethereum/pull/23047))
- Mining stress tests have been fixed up and extended for EIP-1559. ([#22919](https://github.com/ethereum/go-ethereum/pull/22919), [#22930](https://github.com/ethereum/go-ethereum/pull/22930))
- The `elliptic.Curve` implementation provided by crypto/secp256k1 is now more correct regarding the point at infinity. This change is not relevant for go-ethereum itself because the affected curve operations are not used, but may be an improvement for alternative/experimental uses of the secp256k1 package. ([#22621](https://github.com/ethereum/go-ethereum/pull/22621))
- Users of `SimulatedBackend` in package accounts/abi/bind/backends can now simulate blockchain reorgs using the new `Fork` method. ([#22624](https://github.com/ethereum/go-ethereum/pull/22624))
- A minor correctness issue in transaction pool size accounting is resolved. ([#22933](https://github.com/ethereum/go-ethereum/pull/22933))
- The `Compact` method of database tables created by `rawdb.NewTable` now applies the key prefix of the table correctly. ([#22911](https://github.com/ethereum/go-ethereum/pull/22911))
- `Commit` of `trie.Database` would no longer writes state key preimages twice. ([#23001](https://github.com/ethereum/go-ethereum/pull/23001))
- The clique consensus engine now checks common invariants on block headers, matching ethash engine behavior. ([#22836](https://github.com/ethereum/go-ethereum/pull/22836))
- Errors are now checked when creating the state root of a genesis block. ([#22780](https://github.com/ethereum/go-ethereum/pull/22780))
- The core/asm EVM assembler no longer treats numbers starting with `00` as hexadecimal. ([#22883](https://github.com/ethereum/go-ethereum/pull/22883))

For a full rundown of the changes please consult the Geth 1.10.4 [release milestone](https://github.com/ethereum/go-ethereum/milestone/116?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).


[London specification document]: https://github.com/ethereum/eth1.0-specs/blob/master/network-upgrades/mainnet-upgrades/london.md
[snap sync]: https://blog.ethereum.org/2021/03/03/geth-v1-10-0/#snap-sync
[official OpenRPC specification]: https://playground.open-rpc.org/?uiSchema%5BappBar%5D%5Bui:splitView%5D=false&schemaUrl=https://raw.githubusercontent.com/ethereum/eth1.0-specs/master/json-rpc/spec.json&uiSchema%5BappBar%5D%5Bui:input%5D=false
[transaction pool design document]: https://gist.github.com/zsfelfoldi/9607ad248707a925b701f49787904fd6
[EIP-1559]: https://eips.ethereum.org/EIPS/eip-1559
[EIP-3198]: https://eips.ethereum.org/EIPS/eip-3198
[EIP-3529]: https://eips.ethereum.org/EIPS/eip-3529
[EIP-3541]: https://eips.ethereum.org/EIPS/eip-3541
[EIP-3554]: https://eips.ethereum.org/EIPS/eip-3554

## [v1.10.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.3) (2021-05-05)

Geth v1.10.3 is a maintenance release, containing bug fixes and performance improvements.

The performance of the snapshot system has been a big focus in this release cycle.
Generating a snapshot after a snap sync is approximately 10 times faster.

#### Geth command changes

- Several race conditions in database access are resolved, reducing the potential of database corruption ([#22739](https://github.com/ethereum/go-ethereum/pull/22739), [#22728](https://github.com/ethereum/go-ethereum/pull/22728))
- Large numbers in log messages now have thousands separators ([#22665](https://github.com/ethereum/go-ethereum/pull/22665), [#22679](https://github.com/ethereum/go-ethereum/pull/22679))
- Geth no longer warns about database upgrades when starting with an empty database ([#22803](https://github.com/ethereum/go-ethereum/pull/22803))
- The new `geth db freezer-index` debugging command prints the contents of a freezer table ([#22633](https://github.com/ethereum/go-ethereum/pull/22633))
- `geth --dev --datadir ...` works again ([#22738](https://github.com/ethereum/go-ethereum/pull/22738))
- Geth now includes the experimental `--catalyst` mode for eth2 merge testing ([#22641](https://github.com/ethereum/go-ethereum/pull/22641), [#22697](https://github.com/ethereum/go-ethereum/pull/22697), [#22770](https://github.com/ethereum/go-ethereum/pull/22770))
- Puppeth now supports using ssh-agent for authentication ([#22634](https://github.com/ethereum/go-ethereum/pull/22634))

#### Go library changes

- State snapshot generator performance is much improved in this release ([#22667](https://github.com/ethereum/go-ethereum/pull/22667), [#22470](https://github.com/ethereum/go-ethereum/pull/22470), [#22504](https://github.com/ethereum/go-ethereum/pull/22504))
- It is no longer possible to upgrade snapshot databases generated by pre-Berlin geth ([#22663](https://github.com/ethereum/go-ethereum/pull/22663))
- The RPC client now returns non-2xx HTTP responses as `rpc.HTTPError` ([#22677](https://github.com/ethereum/go-ethereum/pull/22677))
- The ethash engine now performs less database lookups when verifying uncle headers ([#21467](https://github.com/ethereum/go-ethereum/pull/21467))
- `trie.StackTrie` has been refactored to improve API semantics. StackTrie methods previously took ownership of key/value byte slices passed to it, which was unintuitive for calling code ([#22673](https://github.com/ethereum/go-ethereum/pull/22673), [#22686](https://github.com/ethereum/go-ethereum/pull/22686), [#22685](https://github.com/ethereum/go-ethereum/pull/22685))

#### RPC/GraphQL API changes

- The gas price oracle deals much better with blocks containing transactions of very low price. Such transactions are typically generated by miners using MEV techniques ([#22722](https://github.com/ethereum/go-ethereum/pull/22722))
- `eth_hashrate` works again ([#22765](https://github.com/ethereum/go-ethereum/pull/22765))
- `debug_traceCall` now supports state overrides like `eth_call` ([#22245](https://github.com/ethereum/go-ethereum/pull/22245))
- EVM Tracing now reports correct gas costs for EIP-2929 state access ([#22702](https://github.com/ethereum/go-ethereum/pull/22702))
- Clef and the external signer account backend now support signing of EIP-2930 access list transactions ([#22585](https://github.com/ethereum/go-ethereum/pull/22585))

#### Networking

- Support for eth/64 has been removed. The minimum protocol version is now eth/65 ([#22636](https://github.com/ethereum/go-ethereum/pull/22636))
- The core of snap sync has been re-architected to allow for dynamic request sizes ([#22668](https://github.com/ethereum/go-ethereum/pull/22668), [#22777](https://github.com/ethereum/go-ethereum/pull/22777))
- Several other correctness issues in the snap sync client are resolved ([#22678](https://github.com/ethereum/go-ethereum/pull/22678), [#22789](https://github.com/ethereum/go-ethereum/pull/22789), [#22760](https://github.com/ethereum/go-ethereum/pull/22760), [#22762](https://github.com/ethereum/go-ethereum/pull/22762), [#22761](https://github.com/ethereum/go-ethereum/pull/22761))
- DNS discovery for the snap protocol now uses the eth protocol node list. The snap-specific node list will be retired later. This is possible because more than 75% of all eth nodes support serving snap ([#22808](https://github.com/ethereum/go-ethereum/pull/22808))
- For eth and snap, the protocol handlers now report additional metrics about response latency ([#22608](https://github.com/ethereum/go-ethereum/pull/22608), [#22751](https://github.com/ethereum/go-ethereum/pull/22751), [#22753](https://github.com/ethereum/go-ethereum/pull/22753)). A Grafana dashboard incorporating the new metrics is available [here](https://gist.github.com/karalabe/1e26f9ea5c842fb118584edadc454e18).
- Several new tests have been added in the cross-client eth protocol test suite. The tests are now more reliable and run as part of pull request CI on Travis ([#22698](https://github.com/ethereum/go-ethereum/pull/22698), [#22630](https://github.com/ethereum/go-ethereum/pull/22630), [#22757](https://github.com/ethereum/go-ethereum/pull/22757), [#22749](https://github.com/ethereum/go-ethereum/pull/22749), [#22754](https://github.com/ethereum/go-ethereum/pull/22754), [#22801](https://github.com/ethereum/go-ethereum/pull/22801))
- The discv5 message handler now reflects IPv4-in-IPv6 addresses correctly when handling PING ([#22703](https://github.com/ethereum/go-ethereum/pull/22703))
- The DNS node list tools in cmd/devp2p now support setting a size limit for node lists. This was added because the list of mainnet snap protocol nodes overflowed our AWS Route53 account ([#22694](https://github.com/ethereum/go-ethereum/pull/22694), [#22695](https://github.com/ethereum/go-ethereum/pull/22695))

#### Build

- The Windows build environment has been cleaned up and updated to use GCC 10 ([#22811](https://github.com/ethereum/go-ethereum/pull/22811), [#22788](https://github.com/ethereum/go-ethereum/pull/22788), [#22804](https://github.com/ethereum/go-ethereum/pull/22804), [#22821](https://github.com/ethereum/go-ethereum/pull/22821))
- The crypto/bn256 and crypto/bls12381 packages are now fuzz-tested against gnark-crypto ([#22755](https://github.com/ethereum/go-ethereum/pull/22755))
- go-ethereum now builds correctly on OpenBSD/arm64 ([#22693](https://github.com/ethereum/go-ethereum/pull/22693))

For a full rundown of the changes please consult the Geth 1.10.3 [release milestone](https://github.com/ethereum/go-ethereum/milestone/115?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://hub.docker.com/r/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.2) (2021-04-08)

Geth v1.10.2 is a maintenance release, containing bug fixes and a few minor new features.

#### Geth command changes

- Mining work package notifications can now be changed to contain the complete pending block header instead of a work package array. To enable this functionality, use the `--miner.notify.full` flag ([#22558](https://github.com/ethereum/go-ethereum/pull/22558))
- Geth will now open chain databases in read-only mode when performing commands that should not modify the database. This fixes several cases where indexing operations and snapshot generation would run as part of commands like `geth inspect` ([#22498](https://github.com/ethereum/go-ethereum/pull/22498))
- Geth now supports JSON log output using the `--log.json` flag ([#22341](https://github.com/ethereum/go-ethereum/pull/22341))
- The `geth copydb` command has been removed because it had been broken for quite a while ([#22501](https://github.com/ethereum/go-ethereum/pull/22501))
- The new `geth db dumptrie` command shows the state tree entries of an account ([#22563](https://github.com/ethereum/go-ethereum/pull/22563))
- When exporting a chain segment using `geth export`, the end block is now validated against the chain head to prevent fatal error at the end of export ([#22387](https://github.com/ethereum/go-ethereum/pull/22387))
- `geth snapshot` commands now support setting the freezer directory using the `--datadir.ancient` flag ([#22486](https://github.com/ethereum/go-ethereum/pull/22486))
- `geth snapsnot prune-state` now considers pruning successful before performing a long-running leveldb compaction. This avoids issues when the compaction process is interrupted by an abnormal exit ([#22579](https://github.com/ethereum/go-ethereum/pull/22579))
- `geth dumpgenesis` and the `geth db` commands now support the `--datadir` flag and the network selection flags (`--rinkeby`, etc.) ([#22406](https://github.com/ethereum/go-ethereum/pull/22406), [#22407](https://github.com/ethereum/go-ethereum/pull/22407))

#### RPC/GraphQL API changes

- The new `eth_createAccessList` RPC method allows auto-creating access lists for EIP-2718 transactions ([#22604](https://github.com/ethereum/go-ethereum/pull/22604))
- RPC methods `ethash_submitHashrate` and `miner_hashrate` have been renamed from their previous incorrect spelling `*hashRate` ([#22604](https://github.com/ethereum/go-ethereum/pull/22604))
- The GraphQL API now supports EIP-2718 access list transactions ([#22491](https://github.com/ethereum/go-ethereum/pull/22491))
- `eth_chainId` now supports chain IDs larger than 64 bits ([#22243](https://github.com/ethereum/go-ethereum/pull/22243))
- The `admin_startRPC` and `admin_stopRPC` methods have been renamed to `(start|stop)HTTP` ([#22461](https://github.com/ethereum/go-ethereum/pull/22461))
- In LES server RPC APIs, the node ID can now be supplied as an `enode://` URL or 32-byte hex ID ([#22423](https://github.com/ethereum/go-ethereum/pull/22423))
- Several bugs related to interactions between chain tracing and the snapshot system are resolved ([#22473](https://github.com/ethereum/go-ethereum/pull/22473), [#22333](https://github.com/ethereum/go-ethereum/pull/22333), [#22629](https://github.com/ethereum/go-ethereum/pull/22629))
- Tracing now reports correct gas amounts for EIP-2718 transactions ([#22480](https://github.com/ethereum/go-ethereum/pull/22480))

#### Go library changes

- TransactOpts of accounts/abi/bind now support the NoSend option, which prevents sending the transaction. This can be used with Go contract bindings to 'dry-run' the binding ([#22446](https://github.com/ethereum/go-ethereum/pull/22446))
- The Ledger USB wallet backend now supports EIP-712 'typed data signing' ([#22378](https://github.com/ethereum/go-ethereum/pull/22378))
- Several critical issues related to state snapshot handling are fixed in this release ([#22540](https://github.com/ethereum/go-ethereum/pull/22540), [#22582](https://github.com/ethereum/go-ethereum/pull/22582), [#22551](https://github.com/ethereum/go-ethereum/pull/22551))
- Background transaction indexing no longer fails for EIP-2718 transactions ([#22457](https://github.com/ethereum/go-ethereum/pull/22457))
- The InfluxDB metrics reporter no longer reports spurious zero values when a histogram has not been updated since the last report ([#22590](https://github.com/ethereum/go-ethereum/pull/22590))
- LES metrics have been renamed to avoid issues in the Prometheus reporter ([#22459](https://github.com/ethereum/go-ethereum/pull/22459))
- The ethstats client no longer crashes when Geth exits immediately after starting ([#22587](https://github.com/ethereum/go-ethereum/pull/22587))
- A rare crash in RPC client subscription handling has been fixed ([#22597](https://github.com/ethereum/go-ethereum/pull/22597))

#### Networking

- Several bugs in the implementation of snap sync are fixed in this release, but it is still considered experimental and not yet enabled by default ([#22596](https://github.com/ethereum/go-ethereum/pull/22596), [#22513](https://github.com/ethereum/go-ethereum/pull/22513), [#22553](https://github.com/ethereum/go-ethereum/pull/22553), [#22591](https://github.com/ethereum/go-ethereum/pull/22591))
- When metrics are enabled, the 'eth' protocol handler now measures the latency of message handling ([#22581](https://github.com/ethereum/go-ethereum/pull/22581), [#22586](https://github.com/ethereum/go-ethereum/pull/22586))
- The DNS discovery client was fixed to avoid hot-spinning when a DNS node trees contains many invalid/incompatible ENRs ([#22566](https://github.com/ethereum/go-ethereum/pull/22566))
- The DNS discovery deployer tool has been updated to resolve some long-standing bugs. Publishing of large trees is now much more efficient because needless updates are avoided, and better batching means the deployer waits less for the server. The tool now uses the latest AWS and CloudFlare SDK versions. ([#22572](https://github.com/ethereum/go-ethereum/pull/22572), [#22538](https://github.com/ethereum/go-ethereum/pull/22538), [#22537](https://github.com/ethereum/go-ethereum/pull/22537), [#22588](https://github.com/ethereum/go-ethereum/pull/22588), [#22360](https://github.com/ethereum/go-ethereum/pull/22360))
- The branch factor of DNS discovery trees has been reduced to ensure that all TXT records fit into the 512-byte limit of DNS/UDP packets ([#22533](https://github.com/ethereum/go-ethereum/pull/22533))
- `devp2p nodeset filter -les-server` was fixed to deal with the new format of the "les" ENR entry ([#22565](https://github.com/ethereum/go-ethereum/pull/22565))
- The cross-client test suite of the 'eth' protocol has been improved a bit, and now skips eth/66 tests when the node under test does not support protocol version 66 ([#22460](https://github.com/ethereum/go-ethereum/pull/22460), [#22508](https://github.com/ethereum/go-ethereum/pull/22508), [#22482](https://github.com/ethereum/go-ethereum/pull/22482), [#22474](https://github.com/ethereum/go-ethereum/pull/22474))
- LES connection pre-negotiation via UDP now works correctly ([#22451](https://github.com/ethereum/go-ethereum/pull/22451))

For a full rundown of the changes please consult the Geth 1.10.2 [release milestone](https://github.com/ethereum/go-ethereum/milestone/114?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.1) (2021-03-08)

Geth v1.10.1 is a minor release with the sole purpose of enabling the [***Berlin hard-fork***](https://github.com/ethereum/eth1.0-specs/blob/master/network-upgrades/berlin.md)! This hard-fork takes a step towards making opcodes fairer and lays the groundwork to new types of transactions, with lots of interesting features to be built on top.

The Ethereum Foundation will have a dedicated blog post for Berlin. The essential parts from Geth's perspective is that v1.10.1 is ***required*** for Berlin on all testnets and the mainnet too. Below you can find the fork blocks for the different networks and their expected schedules. Please ensure you are upgraded well in advance of the forks to ensure a smooth transition.	

- ***Ropsten*** 9,812,189 (10 Mar 2021)	
- ***Goerli*** 4,460,644 (17 Mar 2021)	
- ***Rinkeby*** 8,290,928 (24 Mar 2021)	
- ***Mainnet*** 12,244,000 (14 Apr 2021)	

For a full rundown of the changes please consult the Geth 1.10.1 [release milestone](https://github.com/ethereum/go-ethereum/milestone/113?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.10.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.0) (2021-03-03)

After three months of development, we are proud to announce the Geth 1.10.0 release, which is the first release in the 1.10 series.

There are a lot of new features in this release. While we review the highlights and list individual changes in this overview, we also invite you to read the [Geth 1.10 release blog post](https://blog.ethereum.org/2021/03/03/geth-v1-10-0/), which explains the changes in more detail.

### State snapshot system

In this release, the new snapshot mechanism is enabled by default. Snapshots provide O(1) access to state during EVM execution and also serve as the backbone of the snap sync and state pruning features. Note: should you run into any issues with snapshots, they can still be disabled using the `--snapshot=false` command-line flag. ([#22280](https://github.com/ethereum/go-ethereum/pull/22280), [#22177](https://github.com/ethereum/go-ethereum/pull/22177), [#22288](https://github.com/ethereum/go-ethereum/pull/22288))

### Snap sync

This is a new sync mode, which is a replacement for 'fast sync'. In snap sync, the node downloads Ethereum state data much more efficiently than fast sync ever could. With snap sync, we can also finally provide a progress indicator for the state download. Since this is a new feature, and few peers will support snap sync initially, snap sync is not yet enabled by default. We will make it the default in a couple of weeks. ([#21482](https://github.com/ethereum/go-ethereum/pull/21482), [#22171](https://github.com/ethereum/go-ethereum/pull/22171), [#22235](https://github.com/ethereum/go-ethereum/pull/22235), [#22272](https://github.com/ethereum/go-ethereum/pull/22272), [#22334](https://github.com/ethereum/go-ethereum/pull/22334))

### Berlin fork support

As of Geth 1.10.0, all EIPs scheduled for the Berlin hard fork are supported in Geth. However, due to ongoing debates about the content and timing of the fork, this release does not activate Berlin at any block number.

The following Berlin EIPs were already implemented in Geth 1.9.x:

- EIP-2929: gas cost increases for state access opcodes
- EIP-2315: simple subroutines for the EVM

In Geth 1.10.0, support for the following EIPs has been added:

- EIP-2565: modexp precompile gas cost changes ([#22213](https://github.com/ethereum/go-ethereum/pull/22213))
- EIP-2718: typed transaction envelope ([#21502](https://github.com/ethereum/go-ethereum/pull/21502))
- EIP-2930: access list transactions ([#21502](https://github.com/ethereum/go-ethereum/pull/21502))

We have also updated to the latest consensus tests. ([#22009](https://github.com/ethereum/go-ethereum/pull/22009), [#22290](https://github.com/ethereum/go-ethereum/pull/22290))

### Offline state pruning

We have finally added a way to remove old state from the database. Using the `geth snapshot prune-state` command, you can instruct geth to rebuild the database from the stored snapshot while discarding any data that isn't part of the snapshot window of 128 blocks. Note that this feature is experimental. The pruning process takes a lot of time and geth cannot be used while it is running. We hope to improve this in future releases. ([#21724](https://github.com/ethereum/go-ethereum/pull/21724), [#22386](https://github.com/ethereum/go-ethereum/pull/22386), [#22291](https://github.com/ethereum/go-ethereum/pull/22291), [#22294](https://github.com/ethereum/go-ethereum/pull/22294))

### Database changes

Geth 1.10.0 contains some changes which remove unnecessary data in the blockchain database. In particular, Geth no longer keeps transaction inclusion info for all transactions, and instead limits the storage of inclusion records to one year. For application developers, this change means that very old transactions can no longer be accessed by hash. Note: if you would like to
disable this behavior and keep inclusion information for all historical transactions, you can re-enable indexing using the `--txlookuplimit=0` command-line flag. ([#22293](https://github.com/ethereum/go-ethereum/pull/22293), [#22419](https://github.com/ethereum/go-ethereum/pull/22419))

Storing trie key preimages is now disabled by default. This data is mostly used for contract debugging, e.g. in remix. You can re-enable storing preimages using the `--cache.preimages` flag. ([#22350](https://github.com/ethereum/go-ethereum/pull/22350))

### eth/66 protocol

Geth now supports eth protocol version 66, which adds request IDs. While the new protocol version is supported on the server side, Geth does not use request IDs yet. ([#22241](https://github.com/ethereum/go-ethereum/pull/22241))

We have also added a cross-client test suite for the new protocol version in the `devp2p` tool. ([#22363](https://github.com/ethereum/go-ethereum/pull/22363))

### les/4 protocol

Geth 1.10.0 updates the light client protocol to version 4. The new protocol version uses the eth2 Discovery v5 DHT, has better support for servers which can't serve old transactions, and adds support for EIP-2364 ForkID in the handshake. ([#21909](https://github.com/ethereum/go-ethereum/pull/21909), [#22321](https://github.com/ethereum/go-ethereum/pull/22321), [#22357](https://github.com/ethereum/go-ethereum/pull/22357), [#22343](https://github.com/ethereum/go-ethereum/pull/22343), [#22125](https://github.com/ethereum/go-ethereum/pull/22125), [#22347](https://github.com/ethereum/go-ethereum/pull/22347), [#21940](https://github.com/ethereum/go-ethereum/pull/21940), [#22349](https://github.com/ethereum/go-ethereum/pull/22349), [#21930](https://github.com/ethereum/go-ethereum/pull/21930))

### Geth command changes

- We have fully removed many deprecated command-line flags. ([#22263](https://github.com/ethereum/go-ethereum/pull/22263))
- Geth will now terminate gracefully when the disk is almost full, to avoid database corruption. ([#22103](https://github.com/ethereum/go-ethereum/pull/22103))
- Geth now tracks unclean shutdowns and warns you about them when starting back up. ([#21893](https://github.com/ethereum/go-ethereum/pull/21893))
- Whisper-related flags have been removed. ([#22421](https://github.com/ethereum/go-ethereum/pull/22421))
- USB hardware wallet support is now off by default in geth. It can be enabled using the `--usb` flag. ([#21984](https://github.com/ethereum/go-ethereum/pull/21984), [#22130](https://github.com/ethereum/go-ethereum/pull/22130))
- Using the new `--log.json` flag, you can instruct geth to output machine-readable logs. ([#22207](https://github.com/ethereum/go-ethereum/pull/22207))
- Geth now provides some low-level leveldb commands for debugging. ([#22014](https://github.com/ethereum/go-ethereum/pull/22014))
- Geth now has a `--mainnet` flag, which is useful for scripting. ([#21932](https://github.com/ethereum/go-ethereum/pull/21932))
- Geth has a new flag `--light.nosyncserve` to enable serving light clients before sync completes. This is meant to be used for testing. ([#22250](https://github.com/ethereum/go-ethereum/pull/22250))

### RPC changes

- Non-EIP155 transactions (i.e. transactions which are not replay-protected) are now rejected by the RPC API. Note: you can disable this restriction using the `--rpc.allow-unprotected-txs` command-line flag. ([#22339](https://github.com/ethereum/go-ethereum/pull/22339))
- More transaction details are logged when submitting through RPC. ([#22170](https://github.com/ethereum/go-ethereum/pull/22170))
- It is now possible to configure the JSON-RPC server on a custom path prefix. ([#22184](https://github.com/ethereum/go-ethereum/pull/22184))
- `eth_chainID` now returns an error when the chain has not activated EIP-155 yet. ([#21686](https://github.com/ethereum/go-ethereum/pull/21686))
- The WebSocket transport now limits RPC messages to 15MB. They were previously limited to 5MB. ([#22385](https://github.com/ethereum/go-ethereum/pull/22385))
- The 'bad blocks' tracker now persists some recently seen bad blocks, so they can still
  be accessed after a restart. ([#21827](https://github.com/ethereum/go-ethereum/pull/21827))
  
### GraphQL changes

We have made several backwards-incompatible changes to GraphQL APIs to better match the specification. In cases where the specification was vague, we have coordinated with the Besu development team to match their implementation.

- Receipt `status` is now returned as an integer instead of a hex string. ([#22187](https://github.com/ethereum/go-ethereum/pull/22187))
- `estimateGas` and `cumulativeGas` queries now return an integer instead of a hex string. ([#22126](https://github.com/ethereum/go-ethereum/pull/22126))
- The `gasLimit` and `gasUsed` fields in responses are now integers instead of hex strings. ([#21883](https://github.com/ethereum/go-ethereum/pull/21883))
- Retrieving blocks by number now works correctly. ([#22153](https://github.com/ethereum/go-ethereum/pull/22153))

### Go API changes

- We have added new constructor functions for transactions in package core/types: `types.NewTx` and `types.SignNewTx`. These functions allow creating EIP-2930 access list transactions from Go code. ([#21502](https://github.com/ethereum/go-ethereum/pull/21502))
- The `eth.Config` type has moved to a new package eth/ethconfig. ([#22205](https://github.com/ethereum/go-ethereum/pull/22205))
- `ethconfig.Config` has a new field `SyncFromCheckpoint`, which instructs geth to start syncing at an arbitrary checkpoint ([#22123](https://github.com/ethereum/go-ethereum/pull/22123))
- The event package provides a new helper, `event.ResubscribeErr` for improved logging of subscription errors. ([#22191](https://github.com/ethereum/go-ethereum/pull/22191))
- The `consensus.Engine` interface no longer provides the `VerifySeal` method. ([#22274](https://github.com/ethereum/go-ethereum/pull/22274))
- The `rpc.Client.ShhSubscribe` method is now deprecated. ([#22239](https://github.com/ethereum/go-ethereum/pull/22239))
- Package accounts/keystore now uses github.com/google/uuid to generate key UUIDs. This affects the type definition of `keystore.Key` because the type of the `Id` field is now different. We believe this change won't cause any issues because key UUIDs are not used often. ([#22217](https://github.com/ethereum/go-ethereum/pull/22217))

### Build changes

- We have updated many external dependencies to newer versions. ([#22216](https://github.com/ethereum/go-ethereum/pull/22216), [#22227](https://github.com/ethereum/go-ethereum/pull/22227), [#22134](https://github.com/ethereum/go-ethereum/pull/22134))
- go-ethereum no longer depends on github.com/wsddn/go-ecdh ([#22212](https://github.com/ethereum/go-ethereum/pull/22212))
- go-ethereum no longer depends on github.com/aristanetworks/goarista ([#22211](https://github.com/ethereum/go-ethereum/pull/22211))
- Automated builds now use Go 1.16 ([#22351](https://github.com/ethereum/go-ethereum/pull/22351))
- Linux builders now use Ubuntu 18.04 Bionic ([#22369](https://github.com/ethereum/go-ethereum/pull/22369))
- The mobile framework is now built against NDK version r21e. ([#22368](https://github.com/ethereum/go-ethereum/pull/22368), [#22373](https://github.com/ethereum/go-ethereum/pull/22373))

### Optimizations

This section lists miscellaneous optimizations which were applied during the 1.10.0 development cycle.

- core: speed up header import ([#21967](https://github.com/ethereum/go-ethereum/pull/21967))
- core/state: convert prefetcher to concurrent per-trie loader ([#21047](https://github.com/ethereum/go-ethereum/pull/21047))
- core: switch to github.com/holiman/bloomfilter/v2 ([#22044](https://github.com/ethereum/go-ethereum/pull/22044))
- core/state/snapshot: write snapshot generator in batch ([#22163](https://github.com/ethereum/go-ethereum/pull/22163))
- core/state/snapshot: merge loops for better performance ([#22160](https://github.com/ethereum/go-ethereum/pull/22160))
- eth/downloader: optimize to avoid a copy in state sync hashing ([#22035](https://github.com/ethereum/go-ethereum/pull/22035))
- eth/protocols/snap: speed up hash checks ([#22023](https://github.com/ethereum/go-ethereum/pull/22023))
- eth: optimize tx broadcast mechanism ([#22176](https://github.com/ethereum/go-ethereum/pull/22176))
- consensus/ethash: implement faster difficulty calculators ([#21976](https://github.com/ethereum/go-ethereum/pull/21976))
- core/txpool: remove "local" notion from the txpool price heap ([#21478](https://github.com/ethereum/go-ethereum/pull/21478))
- eth, core: speed up some tests ([#22000](https://github.com/ethereum/go-ethereum/pull/22000))

### Bug fixes

This section lists miscellaneous bug fixes and changes which were applied during the 1.10.0 development cycle.

- accounts/abi/bind: fixed unpacking error ([#22230](https://github.com/ethereum/go-ethereum/pull/22230))
- accounts/abi/bind: fix error-handling in wrappers for functions returning structs ([#22005](https://github.com/ethereum/go-ethereum/pull/22005))
- cmd/abigen: clarify abigen alias flag usage ([#21875](https://github.com/ethereum/go-ethereum/pull/21875))
- cmd/clef: don't check file permissions on windows ([#22251](https://github.com/ethereum/go-ethereum/pull/22251))
- cmd/devp2p: fix documentation for eth-test ([#22298](https://github.com/ethereum/go-ethereum/pull/22298))
- cmd/faucet: fix nonce-gap problem ([#22145](https://github.com/ethereum/go-ethereum/pull/22145))
- cmd/faucet: fix websocket race regression after switching to gorilla ([#22136](https://github.com/ethereum/go-ethereum/pull/22136))
- cmd/faucet: sort requests by newest first ([#22018](https://github.com/ethereum/go-ethereum/pull/22018))
- cmd/faucet: support v1.1 Twitter API in faucet, fix puppeth ([#22107](https://github.com/ethereum/go-ethereum/pull/22107))
- cmd/faucet: switch Facebook auth over to mobile site ([#22137](https://github.com/ethereum/go-ethereum/pull/22137))
- cmd/faucet: update the embedded website asset ([#22169](https://github.com/ethereum/go-ethereum/pull/22169))
- cmd/faucet: use Twitter API instead of scraping webpage ([#21850](https://github.com/ethereum/go-ethereum/pull/21850))
- cmd/geth: CLI help fixes ([#22220](https://github.com/ethereum/go-ethereum/pull/22220))
- cmd/geth: dump config for metrics ([#22083](https://github.com/ethereum/go-ethereum/pull/22083))
- cmd/geth: fix js unclean shutdown ([#22302](https://github.com/ethereum/go-ethereum/pull/22302))
- cmd/utils: add workaround for FreeBSD statfs quirk ([#22310](https://github.com/ethereum/go-ethereum/pull/22310))
- cmd/utils: avoid making console preloads absolute ([#22109](https://github.com/ethereum/go-ethereum/pull/22109))
- common/compiler: fix parsing of solc output with solidity v.0.8.0 ([#22092](https://github.com/ethereum/go-ethereum/pull/22092))
- consensus: refactor FinalizeAndAssemble to use Finalize ([#21993](https://github.com/ethereum/go-ethereum/pull/21993))
- core/state/snapshot: ensure Cap retains a min number of layers ([#22331](https://github.com/ethereum/go-ethereum/pull/22331))
- core/state: copy the snap when copying the state ([#22340](https://github.com/ethereum/go-ethereum/pull/22340))
- core/state: fix panic in state dumping ([#22225](https://github.com/ethereum/go-ethereum/pull/22225))
- core: fix temp memory blowup caused by defers holding on to state ([#22319](https://github.com/ethereum/go-ethereum/pull/22319))
- core: reset to genesis when middle block is missing ([#22135](https://github.com/ethereum/go-ethereum/pull/22135))
- core: reset txpool state on sethead ([#22247](https://github.com/ethereum/go-ethereum/pull/22247))
- ethclient: better test suite for ethclient package ([#22127](https://github.com/ethereum/go-ethereum/pull/22127))
- eth/downloader: enhanced test cases for downloader queue ([#22114](https://github.com/ethereum/go-ethereum/pull/22114))
- eth/filters: fix potential deadlock in filter timeout loop ([#22178](https://github.com/ethereum/go-ethereum/pull/22178))
- eth/protocols/eth: fix slice resize flaw ([#22181](https://github.com/ethereum/go-ethereum/pull/22181))
- eth/protocols/snap: track reverts when peer rejects request ([#22016](https://github.com/ethereum/go-ethereum/pull/22016))
- eth/tracers: fix unigram tracer ([#22248](https://github.com/ethereum/go-ethereum/pull/22248))
- eth/tracers: include intrinsic gas in calltracer, expose for all tracers ([#22038](https://github.com/ethereum/go-ethereum/pull/22038))
- eth: improve transaction broadcaset log messages ([#22146](https://github.com/ethereum/go-ethereum/pull/22146))
- internal/ethapi: restore net_version RPC method ([#22061](https://github.com/ethereum/go-ethereum/pull/22061))
- les: don't drop sentTo for normal cases ([#22048](https://github.com/ethereum/go-ethereum/pull/22048))
- metrics: fix cast omission in cpu_syscall.go ([#22262](https://github.com/ethereum/go-ethereum/pull/22262))
- miner, test: fix potential goroutine leak ([#21989](https://github.com/ethereum/go-ethereum/pull/21989))
- miner: avoid sleeping in miner ([#22108](https://github.com/ethereum/go-ethereum/pull/22108))
- node: always show websocket url in logs ([#22307](https://github.com/ethereum/go-ethereum/pull/22307))
- p2p/dnsdisc: fix hot-spin when all trees are empty ([#22313](https://github.com/ethereum/go-ethereum/pull/22313))
- rlp: handle case of normal EOF in Stream.readFull ([#22336](https://github.com/ethereum/go-ethereum/pull/22336))
- tests/fuzzers/abi: better test generation ([#22158](https://github.com/ethereum/go-ethereum/pull/22158))
- tests/fuzzers/abi: fixed one-off panic with int.Min64 value ([#22233](https://github.com/ethereum/go-ethereum/pull/22233))
- tests/fuzzers/les: add fuzzer for les server handler ([#22282](https://github.com/ethereum/go-ethereum/pull/22282))
- tests/fuzzers: added consensys/gurvy library to bn256 differential fuzzer ([#21812](https://github.com/ethereum/go-ethereum/pull/21812))
- tests/fuzzers: fix abi fuzzer ([#22199](https://github.com/ethereum/go-ethereum/pull/22199))
- tests/fuzzers: fix false positive in bitutil fuzzer ([#22076](https://github.com/ethereum/go-ethereum/pull/22076))
- trie: fix bloom crash on fast sync restart ([#22332](https://github.com/ethereum/go-ethereum/pull/22332))
- trie: fix range prover ([#22210](https://github.com/ethereum/go-ethereum/pull/22210))

For a full rundown of the changes please consult the Geth 1.10.0 [release milestone](https://github.com/ethereum/go-ethereum/milestone/112?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.25) (2020-12-11)

Geth v1.9.25 is a maintenance release.

Notable changes in this release:

- Geth has a new subcommand, `geth version-check`, which displays known security issues ([#21859](https://github.com/ethereum/go-ethereum/pull/21859))
- The geth `--ws.origins` flag now supports more expressive origin rules ([#21481](https://github.com/ethereum/go-ethereum/pull/21481))
- Recording of trie key preimages can now be disabled using the `--cache.preimages` flag ([#21402](https://github.com/ethereum/go-ethereum/pull/21402))
- The accounts/abi/bind package now offers replay-protected transaction signing ([#21356](https://github.com/ethereum/go-ethereum/pull/21356))
- The GraphQL API now always returns status code 400 if there is an error processing the query ([#21882](https://github.com/ethereum/go-ethereum/pull/21882))
- The `devp2p nodeset filter` command can now find snap-enabled nodes ([#21950](https://github.com/ethereum/go-ethereum/pull/21950))
- The eth protocol test suite has been extended with tests for transaction announcements and malicious announce behavior ([#21857](https://github.com/ethereum/go-ethereum/pull/21857), [#21792](https://github.com/ethereum/go-ethereum/pull/21792))
- Support for 'retesteth' has been removed from geth since it is no longer used for tests. Its replacement, the `evm t8n` tool, was released in Geth v1.9.16 ([#21861](https://github.com/ethereum/go-ethereum/pull/21861))
- We now offer signify/minisign signatures for Geth binary downloads as an alternative to PGP. This is experimental, and not yet advertised on the downloads page ([#21798](https://github.com/ethereum/go-ethereum/pull/21798))

Bug fixes:

- A crash in LES server handling of the GetProofsV2 message is resolved. See [CVE-2020-26264 advisory](https://github.com/ethereum/go-ethereum/security/advisories/GHSA-r33q-22hv-j29q) for more information ([#21896](https://github.com/ethereum/go-ethereum/pull/21896))
- The LES server no longer locks up during geth shutdown ([#21927](https://github.com/ethereum/go-ethereum/pull/21927))
- Clef now correctly derives accounts for Ledger Live devices ([#21757](https://github.com/ethereum/go-ethereum/pull/21757))
- The faucet now ignores URL query parameters in Facebook post URLs ([#21838](https://github.com/ethereum/go-ethereum/pull/21838))
- Light client peer discovery now uses DNS ([#21906](https://github.com/ethereum/go-ethereum/pull/21906))
- `go mod vendor` of go-ethereum should now work ([#21735](https://github.com/ethereum/go-ethereum/pull/21735))
- The peer connection acceptor doesn't hot-spin anymore when geth runs out of file descriptors ([#21878](https://github.com/ethereum/go-ethereum/pull/21878))
- Using the `reexec` option for tracing RPC methods no longer crashes the RPC handler ([#21830](https://github.com/ethereum/go-ethereum/pull/21830))
- `common.Hash` and `common.Address` now print as hex when using `fmt.Println` ([#21834](https://github.com/ethereum/go-ethereum/pull/21834))
- A rare deadlock in Discovery v5 message dispatch is fixed ([#21858](https://github.com/ethereum/go-ethereum/pull/21858))
- Failures in internal CPU metrics collection no longer crash geth ([#21864](https://github.com/ethereum/go-ethereum/pull/21864))
- In Go contract bindings generated by abigen, the `Raw` field of parsed events is now set correctly ([#21807](https://github.com/ethereum/go-ethereum/pull/21807))

For a full rundown of the changes please consult the Geth 1.9.25 [release milestone](https://github.com/ethereum/go-ethereum/milestone/111?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.24](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.24) (2020-11-12)

Geth v1.9.24 is a security release. It is built with Go v1.15.5, fixing CVE-2020-28362, which has a critical impact for Ethereum. This release also contains a fix for a [consensus issue related to mining](https://github.com/ethereum/go-ethereum/pull/21793), which would have triggered a chain split on January 1st 2021. 

**We recommend everyone to upgrade to this release or rebuild with Go 1.15.5.**

**Although we publish pre-built binaries for many platforms, certain systems may not have Go 1.15.5 available yet. Notably, our official Docker images will most probably not use Go 1.15.5 due to the base image not being updated yet. Please check the end of the release notes on how you can build your custom Docker image with Go 1.15.5.**

If you are building geth from source, please ensure you are building with Go v1.15.5 or above. We do recommend using the latest Geth version, but if you are not mining and cannot upgrade to geth v1.9.24, please rebuild your current version with Go v1.15.5.

Other changes in this release:

- Add YOLOv2 testnet definition ([#21509](https://github.com/ethereum/go-ethereum/pull/21509), [#21745](https://github.com/ethereum/go-ethereum/pull/21745), [#21747](https://github.com/ethereum/go-ethereum/pull/21747), [#21749](https://github.com/ethereum/go-ethereum/pull/21749)).
- Include the `RETURNDATA` field to VM traces ([#21715](https://github.com/ethereum/go-ethereum/pull/21715)).
- Add fuzz targets for the new stack trie code ([#21799](https://github.com/ethereum/go-ethereum/pull/21799)).
- Small optimizations in transaction root derivation ([#21728](https://github.com/ethereum/go-ethereum/pull/21728)).
- Further polishes on the black-box `eth` protocol tester ([#21782](https://github.com/ethereum/go-ethereum/pull/21782)).
- Maintain one more snapshot diff layer in preparation of pruning ([#21730](https://github.com/ethereum/go-ethereum/pull/21730)).
- Prevent serving snap sync data while snapshot not fully generated ([#21682](https://github.com/ethereum/go-ethereum/pull/21682)).
- Implement TAP output for p2p protocol test suites ([#21760](https://github.com/ethereum/go-ethereum/pull/21760)).
- Improve snapshot recovery to allow resuming generation after a crash ([#21594](https://github.com/ethereum/go-ethereum/pull/21594), [#21775](https://github.com/ethereum/go-ethereum/pull/21775)).

Fixed bugs in this release:

- Fix ethash mining DAG generation for >4GB files ([#21793](https://github.com/ethereum/go-ethereum/pull/21793), [#21803](https://github.com/ethereum/go-ethereum/pull/21803)).
- Fix a snapshot data corruption if it crashes mid generation ([#21804](https://github.com/ethereum/go-ethereum/pull/21804)).
- Fix transaction indexing to better support graceful shutdowns ([#21331](https://github.com/ethereum/go-ethereum/pull/21331)).
- Fix a regression that cause the console to terminate on Ctrl+C ([#21660](https://github.com/ethereum/go-ethereum/pull/21660)).
- Fix an RPC crash caused by an invalid AccountRange request ([#21710](https://github.com/ethereum/go-ethereum/pull/21710)).
- Fix a peer disconnection issue between unsynced LES servers ([#21761](https://github.com/ethereum/go-ethereum/pull/21761)).
- Fix an `abigen` regression that silently discarded an EVM error ([#21743](https://github.com/ethereum/go-ethereum/pull/21743)).
- Fix Ledger version parsing to correctly detect data-sign support ([#21733](https://github.com/ethereum/go-ethereum/pull/21733)).

For a full rundown of the changes please consult the Geth 1.9.24 [release milestone](https://github.com/ethereum/go-ethereum/milestone/110?closed=1)

---

You can use the following Dockerfile to build a custom Geth image with Go 1.15.5 while upstream base images get pushed (they usually take quite a few hours):

```Dockerfile
# Build Geth in a stock Go builder container
FROM golang:1.15-alpine as builder

RUN apk add --no-cache make gcc musl-dev linux-headers git bash

# Temporarily pull a custom Go bundle
ADD https://golang.org/dl/go1.15.5.src.tar.gz /tmp/go.tar.gz
RUN (cd /tmp && tar -xf go.tar.gz)
RUN (cd /tmp/go/src && ./make.bash)
ENV PATH="/tmp/go/bin:${PATH}"

ADD . /go-ethereum
RUN cd /go-ethereum && make geth

# Pull Geth into a second stage deploy alpine container
FROM alpine:latest

RUN apk add --no-cache ca-certificates
COPY --from=builder /go-ethereum/build/bin/geth /usr/local/bin/

EXPOSE 8545 8546 30303 30303/udp
ENTRYPOINT ["geth"]
```

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.23) (2020-10-15)

Geth v1.9.23 is a maintenance release containing security fixes. This update is recommended for all users.

Security issues fixed in this release:

- Mining no longer stops due to sync after the first successful sync round ([#21701](https://github.com/ethereum/go-ethereum/pull/21701))
- Peer-to-peer client names are now truncated in logs to prevent log spam ([#21698](https://github.com/ethereum/go-ethereum/pull/21698))

Other changes in this release:

- go-ethereum now implements Node Discovery Protocol v5.1 ([#21647](https://github.com/ethereum/go-ethereum/pull/21647))
- The cmd/faucet utility now uses DNS discovery to find LES servers ([#21636](https://github.com/ethereum/go-ethereum/pull/21636))
- Various issues with web3.js console functions are resolved ([#21639](https://github.com/ethereum/go-ethereum/pull/21639),                                             [#21608](https://github.com/ethereum/go-ethereum/pull/21608), [#21629](https://github.com/ethereum/go-ethereum/pull/21629))
- HTTP/WebSocket upgrade negotiation is more robust ([#21646](https://github.com/ethereum/go-ethereum/pull/21646))
- The 'eth' peer-to-peer protocol test suite now works with more client implementations ([#21615](https://github.com/ethereum/go-ethereum/pull/21615))
- TxPool error handling for invalid transactions is improved ([#21683](https://github.com/ethereum/go-ethereum/pull/21683))
- It is now possible to create BigInt objects from Java using the mobile framework ([#21597](https://github.com/ethereum/go-ethereum/pull/21597))
- The mobile framework build now includes geth-sources.jar, enabling JavaDoc auto-completion ([#21596](https://github.com/ethereum/go-ethereum/pull/21596))
- The Görli testnet bootnode list has been updated ([#21659](https://github.com/ethereum/go-ethereum/pull/21659))
- Clef: the new `account_signGnosisSafeTx` API method helps with transaction signing using Gnosis Safe ([#21593](https://github.com/ethereum/go-ethereum/pull/21593))
- Clef: `account_list` requests now work even when when no wallets are present ([#21677](https://github.com/ethereum/go-ethereum/pull/21677))

Optimizations:

- The new StackTrie implementation is now used for the tx and receipt root hash calculation ([#21407](https://github.com/ethereum/go-ethereum/pull/21407), [#21699](https://github.com/ethereum/go-ethereum/pull/21699), [#21643](https://github.com/ethereum/go-ethereum/pull/21643), [#21692](https://github.com/ethereum/go-ethereum/pull/21692))
- The bloom filter implementation in core/types is now faster and more correct ([#21624](https://github.com/ethereum/go-ethereum/pull/21624))
- The bloombits trie generator is also much faster ([#21625](https://github.com/ethereum/go-ethereum/pull/21625))
- Block header hashes are cached more aggressively in the downloader ([#21678](https://github.com/ethereum/go-ethereum/pull/21678))

For a full rundown of the changes please consult the Geth 1.9.23 [release milestone](https://github.com/ethereum/go-ethereum/milestone/109?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.22) (2020-09-28)

Geth v1.9.22 is our usual maintenance release, fixing a few bugs and adding some minor features:

- Add `gpo.maxprice` CLI flag to override the upper limit of the automatic gas price suggester ([#21531](https://github.com/ethereum/go-ethereum/pull/21531)).
- Add `BlockNumber` method to `ethclient` to support retrieving the head block's number ([#21500](https://github.com/ethereum/go-ethereum/pull/21500)).
- Extend the call tracer to include additional contextual fields for `SELFDESTRUCT` ([#21549](https://github.com/ethereum/go-ethereum/pull/21549), [#21564](https://github.com/ethereum/go-ethereum/pull/21564)).
- Unexpose port 8547 from docker images as GraphQL was merged with HTTP RPC ([#21556](https://github.com/ethereum/go-ethereum/pull/21556)).
- Support retroactively setting Petersburg if it coincides with Constantinople ([#21473](https://github.com/ethereum/go-ethereum/pull/21473)).
- Support gas estimation against arbitrary blocks, not just pending ([#21545](https://github.com/ethereum/go-ethereum/pull/21545). [#21601](https://github.com/ethereum/go-ethereum/pull/21601)).
- Extend database inspection results with item counters too beside size ([#21495](https://github.com/ethereum/go-ethereum/pull/21495)).
- Dynamically move fast pivot point even during chain sync phase ([#21529](https://github.com/ethereum/go-ethereum/pull/21529)).
- Start implementing network protocol testers ([#21598](https://github.com/ethereum/go-ethereum/pull/21598), [#21603](https://github.com/ethereum/go-ethereum/pull/21603), [#21604](https://github.com/ethereum/go-ethereum/pull/21604)).
- Support flexible range proofs for snap sync ([#21484](https://github.com/ethereum/go-ethereum/pull/21484). [#21199](https://github.com/ethereum/go-ethereum/pull/21199), [#21250](https://github.com/ethereum/go-ethereum/pull/21250)).
- Extract `rlpx` into it's own package for easier protocol tests ([#21464](https://github.com/ethereum/go-ethereum/pull/21464)).
- Minor API polishes in the Java mobile framework ([#21580](https://github.com/ethereum/go-ethereum/pull/21580)).
- Add fuzzer suite for ABI encoding and decoding ([#21217](https://github.com/ethereum/go-ethereum/pull/21217)).

Bugfixes:

- Print warning if Whisper is present in the `config.toml` instead of rejecting ([#21544](https://github.com/ethereum/go-ethereum/pull/21544)).
- Handle miner suspends/resumption due to sync more gracefully ([#21536](https://github.com/ethereum/go-ethereum/pull/21536), [#21547](https://github.com/ethereum/go-ethereum/pull/21547)).
- Fix a light client regression that crashed the node after a sync failure ([#21537](https://github.com/ethereum/go-ethereum/pull/21537)).

For a full rundown of the changes please consult the Geth 1.9.22 [release milestone](https://github.com/ethereum/go-ethereum/milestone/108?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.21) (2020-09-09)

Geth v1.9.21 is a regular maintenance release, the highlights being the removal of whisper, better call tracing and multiple memory stability fixes during fast sync to both stabilize usage as well as to fix a memory leak that lead to crashers before.

- Remove Whisper as promised a couple months ago ([#21487](https://github.com/ethereum/go-ethereum/pull/21487), [#21526](https://github.com/ethereum/go-ethereum/pull/21526), [#21527](https://github.com/ethereum/go-ethereum/pull/21527))!
- Minor user experience polishes around legacy Ledger derivation paths ([#21517](https://github.com/ethereum/go-ethereum/pull/21517)).
- Implement arbitrary call tracing via `debug_traceCall` on top of arbitrary blocks ([#21338](https://github.com/ethereum/go-ethereum/pull/21338)).
- Expose internal transaction revertals and revert reason in the `call_tracer` tracer ([#21387](https://github.com/ethereum/go-ethereum/pull/21387)). 
- Cap the number of in-memory trie nodes during fast sync, fix crasher memory leak ([#21491](https://github.com/ethereum/go-ethereum/pull/21491)).
- Prepare the trie syncer for path-based operation to support the upcoming `snap` sync ([#21504](https://github.com/ethereum/go-ethereum/pull/21504)).
- Limit the cached data in the downloader more aggressively to avoid memory fluctuations ([#21366](https://github.com/ethereum/go-ethereum/pull/21366)).
- Fix the simulated chain to not allow changing block timestamps if transactions were included ([#21334](https://github.com/ethereum/go-ethereum/pull/21334)).
- Fix an ABI parser issue around tuples ([#21501](https://github.com/ethereum/go-ethereum/pull/21501)).

For a full rundown of the changes please consult the Geth 1.9.21 [release milestone](https://github.com/ethereum/go-ethereum/milestone/107?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.20) (2020-08-25)

Geth v1.9.20 is another maintenance release containing bug fixes and security enhancements. This update is recommended for all users.

***Please note that reverting to Geth v1.9.19 or prior after upgrading to v1.9.20 is not possible without a resync because the blockchain database layout has changed.***

New Features:

- You can now query the Chain ID using GraphQL ([#21451](https://github.com/ethereum/go-ethereum/pull/21451))
- The `evm` command can now appends the TX hash to state transition output files ([#21406](https://github.com/ethereum/go-ethereum/pull/21406))

Bug Fixes & Optimizations:

- Blockchain rewinding and chain repair, i.e. using the `SetHead` operation, now behave
  correctly in all cases. An extensive test suite for chain repair has been added. ([#21409](https://github.com/ethereum/go-ethereum/pull/21409), [#21409](https://github.com/ethereum/go-ethereum/pull/21409))
- Discovery DHT bootstrapping now works properly in very small private networks. ([#21396](https://github.com/ethereum/go-ethereum/pull/21396))
- Contract code is now stored separately from state tree data in LevelDB. This change will
  help with future database upgrades. ([#21080](https://github.com/ethereum/go-ethereum/pull/21080))
- Two additional metrics counters for blockchain reorgs have been added. ([#21420](https://github.com/ethereum/go-ethereum/pull/21420))
- Metrics collection is now lock-free, causing less lock contention among peer connections ([#21446](https://github.com/ethereum/go-ethereum/pull/21446), [#21470](https://github.com/ethereum/go-ethereum/pull/21470))
- The GetNodeData operation in the eth peer-to-peer protocol now uses the fast-sync bloom filter
  to speed up request processing. ([#21445](https://github.com/ethereum/go-ethereum/pull/21445))
- Importing block data during fast-sync now performs significantly fewer database reads. ([#21468](https://github.com/ethereum/go-ethereum/pull/21468))

Build Changes:

- This release is built with the new and shiny Go 1.15 compiler version ([#21466](https://github.com/ethereum/go-ethereum/pull/21466))
- The Ubuntu PPA now contains builds for Ubuntu 20.10 Groovy Gorilla. Ubuntu 19.04 Disco
  Dingo is no longer supported. ([#21461](https://github.com/ethereum/go-ethereum/pull/21461))

For a full rundown of the changes please consult the Geth 1.9.20 [release milestone](https://github.com/ethereum/go-ethereum/milestone/106?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.19) (2020-08-11)

Geth v1.9.19 is our regular biweekly maintenance release. Apart from the numerous bug-fixes however, it also ships a more deterministic transaction sort order during mining (FIFO). The goal is to reduce front-runner spam which abused miner randomness for transactions at the same price level.

- Sort same-priced transactions by arrival time during mining to reduce front-running ([#21358](https://github.com/ethereum/go-ethereum/pull/21358)).
- Persist the trie read cache on shutdown to speed up warmup time after a reboot ([#20391](https://github.com/ethereum/go-ethereum/pull/20391)).
- Swap out the block fetcher of `les/x` to use the same mechanism as `eth/6x` ([#20692](https://github.com/ethereum/go-ethereum/pull/20692)).
- Support setting custom HTTP headers on RCP clients e.g. for authentication ([#21392](https://github.com/ethereum/go-ethereum/pull/21392)).
- Refactor node lifecycle management, merge GraphQL onto HTTP endpoint ([#21105](https://github.com/ethereum/go-ethereum/pull/21105)).
- Print `enode` ID of a node too when doing an ENR dump via `devp2p` ([#21270](https://github.com/ethereum/go-ethereum/pull/21270)).
- Trim the build paths to avoid leaking builder infos into the binaries ([#21374](https://github.com/ethereum/go-ethereum/pull/21374)).
- Limit concurrent UPnP requests to avoid crashing certain routers ([#21390](https://github.com/ethereum/go-ethereum/pull/21390)).

Bugfixes:

- Fix a potential data corruption by leaking out txpool internals into the miner ([#21159](https://github.com/ethereum/go-ethereum/pull/21159)).
- Avoid a light server doing EVM calls during handshake while it's syncing ([#21425](https://github.com/ethereum/go-ethereum/pull/21425)).
- Fix the trie clean cache size calculation if snapshots are disabled ([#21416](https://github.com/ethereum/go-ethereum/pull/21416)).
- Revert an optimization regression in the JUMPDEST analysis ([#21411](https://github.com/ethereum/go-ethereum/pull/21411)).
- Fix EIP 712 structured data signing corner cases ([#21306](https://github.com/ethereum/go-ethereum/pull/21306), [#21307](https://github.com/ethereum/go-ethereum/pull/21307)).
- Fix round-trip time calculation in the downloader ([#21427](https://github.com/ethereum/go-ethereum/pull/21427), [#21429](https://github.com/ethereum/go-ethereum/pull/21429)).
- Fix a panic in ethstats on the Görli stats page ([#21404](https://github.com/ethereum/go-ethereum/pull/21404), [#21434](https://github.com/ethereum/go-ethereum/pull/21434)).
- Fix a hang in the initial fast-sync's state sync phase ([#21433](https://github.com/ethereum/go-ethereum/pull/21433)).
- Fix graceful LES server shutdown ([#21426](https://github.com/ethereum/go-ethereum/pull/21426)).

***Note, if you were using GraphQL previously, it was moved to the HTTP RPC endpoint. The old `--graphql.host` and `--graphql.port` flags will not work any more. You might need to adjust your TOML config files accordingly too.***

For a full rundown of the changes please consult the Geth 1.9.19 [release milestone](https://github.com/ethereum/go-ethereum/milestone/105?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.18) (2020-07-27)

Geth v1.9.18 is a bugfix release, fixing an occasional fast sync hang in the throttling mechanism (among other improvements):

- Memory allocation micro-optimizations to improve raw EVM number crunching by 5% ([#21336](https://github.com/ethereum/go-ethereum/pull/21336)).
- Fix a regression that made previously persisted `--dev` chains unable to load back up ([#21352](https://github.com/ethereum/go-ethereum/pull/21352)).
- Support configurable developer account (and passphrase) in `--dev` mode ([#21301](https://github.com/ethereum/go-ethereum/pull/21301)).
- Fix downloader throttling that degraded sync and occasionally locked it up ([#21263](https://github.com/ethereum/go-ethereum/pull/21263)).
- Fix local `gomobile` building and fix iOS framework builds ([#21361](https://github.com/ethereum/go-ethereum/pull/21361), [#21362](https://github.com/ethereum/go-ethereum/pull/21362)).
- Fix stale transaction eviction bug, stabilizing pool churn ([#21300](https://github.com/ethereum/go-ethereum/pull/21300)).

For a full rundown of the changes please consult the Geth 1.9.18 [release milestone](https://github.com/ethereum/go-ethereum/milestone/104?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.17) (2020-07-20)

Geth v1.9.17 is a small maintenance release (trying to get back onto the biweekly schedule), though it does pack a few punches as well!

- Enable historical garbage collection for light clients ([#19570](https://github.com/ethereum/go-ethereum/pull/19570)).
- Apply `--rpc.txfeecap` to a few missed endpoints ([#21231](https://github.com/ethereum/go-ethereum/pull/21231)).
- Drastically reduce allocations in the transaction pool ([#21328](https://github.com/ethereum/go-ethereum/pull/21328)).
- Drastically reduce allocations on certain EVM opcodes ([#21222](https://github.com/ethereum/go-ethereum/pull/21222)).
- Raise the default gas limit in `--dev` mode to 12 million ([#21323](https://github.com/ethereum/go-ethereum/pull/21323)).
- Fix ethstats reconnect issue and fix constant Görli drops ([#21347](https://github.com/ethereum/go-ethereum/pull/21347)).
- Fix gas estimation if `balance / price` overflown `uint64` ([#21346](https://github.com/ethereum/go-ethereum/pull/21346)).

For a full rundown of the changes please consult the Geth 1.9.17 [release milestone](https://github.com/ethereum/go-ethereum/milestone/103?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.16) (2020-07-10)

Geth v1.9.16 is another maintenance release containing a couple minor new features, bug fixes, and block processing optimizations.

This release adds the `--rpc.txfeecap` geth option, which limits transaction fees to a set value. This limit applies to transactions sent via `eth_sendTransaction`. The default limit is 1 ether. ([#21212](https://github.com/ethereum/go-ethereum/pull/21212))

The default value for `--rpc.gascap` is now 25M gas. It previously defaulted to unlimited gas. This applies to `eth_call` and will reject calls which request more gas than the cap. ([#21229](https://github.com/ethereum/go-ethereum/pull/21229))

Minor new features:

- You can now run the metrics HTTP server on a separate endpoint. ([#21290](https://github.com/ethereum/go-ethereum/pull/21290))
- Protocol message metrics now count the number of messages in addition to bandwidth. ([#21256](https://github.com/ethereum/go-ethereum/pull/21256))
- The `geth import` command now exits with status 1 if errors have occurred. ([#21244](https://github.com/ethereum/go-ethereum/pull/21244))
- The `debug_traceTransaction` RPC method now includes read storage entries in structlog output. ([#21204](https://github.com/ethereum/go-ethereum/pull/21204))
- cmd/evm: add state transition tool for testing ([#20958](https://github.com/ethereum/go-ethereum/pull/20958))
- cmd/devp2p: the devp2p tool now contains a test suite for Discovery v4 ([#21163](https://github.com/ethereum/go-ethereum/pull/21163))
- cmd/devp2p: the new `devp2p key` command family provides node key management tools ([#21202](https://github.com/ethereum/go-ethereum/pull/21202))
- cmd/ethkey: you can now use the `--passwordfile` option with the `ethkey generate` command ([#21183](https://github.com/ethereum/go-ethereum/pull/21183))

Optimizations:

- The EVM now uses the 'uint256' library, improving the performance of math-heavy calls. ([#20787](https://github.com/ethereum/go-ethereum/pull/20787), [#21206](https://github.com/ethereum/go-ethereum/pull/21206))
- The LES server no longer queries the checkpoint contract for every peer connection, reducing CPU usage. ([#21285](https://github.com/ethereum/go-ethereum/pull/21285))
- The gas price oracle now requires significantly less bandwidth when using the light client. ([#20409](https://github.com/ethereum/go-ethereum/pull/20409))
- The RLP encoder allocates a lot less, especially for `big.Int`, `[N]byte` and list-heavy data structures. ([#21291](https://github.com/ethereum/go-ethereum/pull/21291), [#21274](https://github.com/ethereum/go-ethereum/pull/21274))
- The github.com/golang/snappy dependency has been updated and includes a couple new new optimizations, including an assembly language implementation for arm64. ([#21237](https://github.com/ethereum/go-ethereum/pull/21237), [#21304](https://github.com/ethereum/go-ethereum/pull/21304))
- crypto, core/types: less allocations when hashing and tx handling ([#21265](https://github.com/ethereum/go-ethereum/pull/21265))
- trie: reduce allocations in insertPreimage ([#21261](https://github.com/ethereum/go-ethereum/pull/21261))
- crypto/secp256k1: enable 128-bit int code and endomorphism optimization ([#21203](https://github.com/ethereum/go-ethereum/pull/21203))
- core/state: avoid escape analysis fault when accessing cached state ([#21192](https://github.com/ethereum/go-ethereum/pull/21192))
  
Bug fixes:

- A long standing bug related to internal fast sync restarts is resolved. ([#21260](https://github.com/ethereum/go-ethereum/pull/21260))
- Account management methods no longer crash when the external signer is enabled. ([#21279](https://github.com/ethereum/go-ethereum/pull/21279))
- `eth_call` now defaults to the gas cap value instead of MaxInt64. This avoids warnings for calls with unspecified gas. ([#21284](https://github.com/ethereum/go-ethereum/pull/21284))
- Geth now builds and runs on DragonflyBSD ([#21275](https://github.com/ethereum/go-ethereum/pull/21275), [#21241](https://github.com/ethereum/go-ethereum/pull/21241))
- Package ethclient and the accounts/* package tree no longer depend on the RPC API implementation package. ([#21319](https://github.com/ethereum/go-ethereum/pull/21319))
- eth/downloader: fixes data race between synchronize and other methods ([#21201](https://github.com/ethereum/go-ethereum/pull/21201))
- utils: fix ineffectual miner config flags ([#21271](https://github.com/ethereum/go-ethereum/pull/21271))
- eth: returned revert reason in traceTx ([#21195](https://github.com/ethereum/go-ethereum/pull/21195))
- core/vm: fix incorrect computation of BLS discount ([#21253](https://github.com/ethereum/go-ethereum/pull/21253))
- eth: don't block if transaction broadcast loop fails ([#21255](https://github.com/ethereum/go-ethereum/pull/21255))
- core/rawdb: fix high memory usage in freezer ([#21243](https://github.com/ethereum/go-ethereum/pull/21243))
- core/rawdb: swap tailId and itemOffset for deleted items in freezer ([#21220](https://github.com/ethereum/go-ethereum/pull/21220))
- cmd, node: dump empty value config ([#21296](https://github.com/ethereum/go-ethereum/pull/21296))
- internal/web3ext: add missing params to debug.accountRange ([#21208](https://github.com/ethereum/go-ethereum/pull/21208))
- accounts/abi: make GetType public again ([#21157](https://github.com/ethereum/go-ethereum/pull/21157))

For a full rundown of the changes please consult the Geth 1.9.16 [release milestone](https://github.com/ethereum/go-ethereum/milestone/102?closed=1)

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.15) (2020-06-08)

Geth v1.9.15 is a maintenance release containing bug fixes as well as implementations of all EIPs currently scheduled for the upcoming Berlin fork. A *temporary* test network for these EIPs has also been launched at https://yolonet.xyz/ and can be joined via Geth with `--yolov1` flag.

The minimum Go version required to build `go-ethereum` is now Go 1.13.

The `eth_call`, `eth_estimateGas` and `eth_sendTransaction` RPC methods now return the revert reason as a JSON-RPC error when the contract executes REVERT. The returned error includes the decoded reason string in the error message, and also makes the raw REVERT data available in the error's `"data"` field:

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": 3,
    "message": "execution reverted: some error",
    "data": "0x08c379a000000000000000000..."
  }
}
```

Other improvements and fixes in this release:

- The LES 'server pool' was rewritten and can now use DNS discovery to find light servers ([#20758](https://github.com/ethereum/go-ethereum/pull/20758)).
- Argument checking for natively-implemented console functions is improved ([#21081](https://github.com/ethereum/go-ethereum/pull/21081), [#21160](https://github.com/ethereum/go-ethereum/pull/21160)).
- Geth no longer hangs during shutdown while waiting for synced blocks to import ([#21114](https://github.com/ethereum/go-ethereum/pull/21114)).
- The RPC client now sends WebSocket ping frames when connection is idle ([#21142](https://github.com/ethereum/go-ethereum/pull/21142)).
- Uses of the `gosigar` library have been replaced by `gopsutil`. This restores building Geth on `darwin` without cgo and makes automatic database cache size selection work on OpenBSD ([#21041](https://github.com/ethereum/go-ethereum/pull/21041)).
- Prometheus metrics exporting no longer sends duplicate type definitions ([#21068](https://github.com/ethereum/go-ethereum/pull/21068)).
- Password input prompts in Clef now work when stdin is not a TTY ([#20960](https://github.com/ethereum/go-ethereum/pull/20960)).
- Internal sync error reporting has been made more detailed ([#21067](https://github.com/ethereum/go-ethereum/pull/21067)).
- EVM JUMPDEST checks are a bit faster in certain cases ([#21123](https://github.com/ethereum/go-ethereum/pull/21123)).
- Fix the `puppeth` faucet's tweet retrieval endpoint ([#21172](https://github.com/ethereum/go-ethereum/pull/21172)).

The following core EIPs are implemented in this release:

- EIP-2537: Precompile for BLS12-381 curve operations ([#21018](https://github.com/ethereum/go-ethereum/pull/21018)).
- EIP-2315: JUMPSUB for the EVM ([#20619](https://github.com/ethereum/go-ethereum/pull/20619)).

Please note that the EIP implementations are not yet scheduled to activate and will receive further testing and development before the fork.

For a full rundown of the changes please consult the Geth 1.9.15 [release milestone](https://github.com/ethereum/go-ethereum/milestone/101?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.14) (2020-05-13)

Geth v1.9.14 is a regular maintenance release, but is also packs a punch with some interesting new features!

Ethereum mainnet currently contains over 700M transactions. Each full node maintains a search index, stating that transactions with `hash H` is stored in `block B`. This allows you to look up an arbitrary transaction from the past (at a significant storage cost). But how often do you look up transactions from years ago?

Geth v1.9.14 ships a `--txlookuplimit` flag, which specifies the number of recent blocks you want to maintain the search index for (by default it's `0` = since genesis). At its most extreme, you can set it to `1`, to prune all past indexes. At the time of release, this reduces your LevelDB SSD footprint by 32GB! You can modify this flag at will, Geth will unindex/reindex in the background based on the current setting. *If you unindex a lot of transactions, you might need to compact your database to reclaim the space immediately via `debug.chaindbCompact()`.*

Deleting transaction indexes locally is fine since they are not used in consensus nor in synchronization, so network health is unaffected. Light servers for now do need to maintain the full index since light clients rely on them. Huge props to @rjl493456442 and @holiman for this work ([#20302](https://github.com/ethereum/go-ethereum/pull/20302)).

![txlookuplimit](https://user-images.githubusercontent.com/129561/81791915-07bc0780-9510-11ea-9a65-896e95620fd6.png)


In the current release, we've reworked gas estimation so that reverting transactions will give you a proper error, bubbling up the EVM revert reason ([#20830](https://github.com/ethereum/go-ethereum/pull/20830)). The release also renamed most of the RPC API related flags to make them consistent with our namespace style, but don't worry, the deprecated flags will keep working for the foreseeable future ([#20935](https://github.com/ethereum/go-ethereum/pull/20935)).

A rundown of shipped features:

- Implement background transaction indexing and transaction index deletion ([#20302](https://github.com/ethereum/go-ethereum/pull/20302)).
- Improve gas estimation to return the failure/revert reason too if available ([#20830](https://github.com/ethereum/go-ethereum/pull/20830)).
- Make light client chain selection logic similar to full nodes on Clique PoA ([#20931](https://github.com/ethereum/go-ethereum/pull/20931)).
- Drop the `--override.istanbul` and `--override.muirglacier` flags ([#20942](https://github.com/ethereum/go-ethereum/pull/20942)).
- Rename a significant number of flags to have consistent namespaces ([#20935](https://github.com/ethereum/go-ethereum/pull/20935)).
- Optimize the ABI decoder to only calculate method IDs once ([#20895](https://github.com/ethereum/go-ethereum/pull/20895)).
- Remove legacy v5 discovery bootnodes used by LES ([#20949](https://github.com/ethereum/go-ethereum/pull/20949)).
- Various ABI package refactors and cleanups ([#21009](https://github.com/ethereum/go-ethereum/pull/21009), [#21022](https://github.com/ethereum/go-ethereum/pull/21022)).
- Support overloaded struct fields in the ABI parser ([#21060](https://github.com/ethereum/go-ethereum/pull/21060)).
- Implement account and storage trie range proofs ([#20908](https://github.com/ethereum/go-ethereum/pull/20908)).
- Implement snapshot storage iteration ([#20971](https://github.com/ethereum/go-ethereum/pull/20971)).

A rundown of fixed bugs:

- In case of a corrupted database, fail block processing, don't report bad block ([#21039](https://github.com/ethereum/go-ethereum/pull/21039)).
- Fix a fast sync regression that didn't properly abort on premature shutdown ([#20988](https://github.com/ethereum/go-ethereum/pull/20988)).
- Fix the logger to properly escape all special characters in context fields ([#20987](https://github.com/ethereum/go-ethereum/pull/20987)).
- Fix the snapshot journal reloading to not lose deleted account markers ([#21003](https://github.com/ethereum/go-ethereum/pull/21003)).
- Fix a trie data race when accessing preimages through the RPC APIs ([#20923](https://github.com/ethereum/go-ethereum/pull/20923)).
- Fix the TCP P2P dialer to reject enode IDs that advertise TCP port 0 ([#21008](https://github.com/ethereum/go-ethereum/pull/21008)).
- Fix gas estimation to cap max attempted usage at account balance ([#21043](https://github.com/ethereum/go-ethereum/pull/21043)).
- Fix the Java `abigen` code generator to support `void` return types ([#21002](https://github.com/ethereum/go-ethereum/pull/21002)).
- Fix the memory capper to be more aggressive on 32bit platforms ([#21028](https://github.com/ethereum/go-ethereum/pull/21028)).
- Fix a data race in key import if the same is imported concurrently ([#20915](https://github.com/ethereum/go-ethereum/pull/20915)).
- Fix a data race in the snapshot iteration (upcoming protocol) ([#20948](https://github.com/ethereum/go-ethereum/pull/20948)).
- Fix a freezer termination annoyance that printed warnings ([#21010](https://github.com/ethereum/go-ethereum/pull/21010)).
- Fix the state dumper to also include the zero address ([#21038](https://github.com/ethereum/go-ethereum/pull/21038)).
- Fix an `ethstats` regression that was leaking tickers ([#21071](https://github.com/ethereum/go-ethereum/pull/21071)).
- Fix receipt loss in empty Clique blocks after a crash ([#21045](https://github.com/ethereum/go-ethereum/pull/21045)).
- Fix a memory leak in the snapshot storage iteration ([#21036](https://github.com/ethereum/go-ethereum/pull/21036)).
- Fix accidental snapshot creation on archive nodes ([#21025](https://github.com/ethereum/go-ethereum/pull/21025)).
- Fix startup parameters of Windows `.lnk` files ([#21055](https://github.com/ethereum/go-ethereum/pull/21055)).

For a full rundown of the changes please consult the Geth 1.9.14 [release milestone](https://github.com/ethereum/go-ethereum/milestone/100?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.13) (2020-04-16)

Geth v1.9.13 is a scheduled maintenance release, focusing on polishes and fixes. The highlight of the release is that we've finally merged support for dynamic state snapshots, something we've been working on for over half a year. For now it's not yet enabled by default, but we're hoping for big things to be built on top of this.

This release also switches over to Go 1.14.2 (the first version in the 1.14.x family that is stable for Ethereum), resulting in about 10% block processing speedup.

A summary of the features we've been working on:

- Implement dynamic state snapshots (behind `--snapshot` for now) ([#20152](https://github.com/ethereum/go-ethereum/pull/20152)).
- Bump the propagated transaction size limit to 128KB, up from 64KB ([#20835](https://github.com/ethereum/go-ethereum/pull/20835)).
- Support running the HTTP and WebSocket RPC on the same port ([#20810](https://github.com/ethereum/go-ethereum/pull/20810)).
- Support keeping the `ethash` caches and DAG forcefully in RAM ([#20484](https://github.com/ethereum/go-ethereum/pull/20484)).
- Deprecate `--testnet` in favor of `--ropsten`, but keep it for now ([#20852](https://github.com/ethereum/go-ethereum/pull/20852)).
- Add a `newaccount` command to Clef, mostly for tutorial purposes ([#20782](https://github.com/ethereum/go-ethereum/pull/20782)).
- Expose individual metrics for every RPC method call type ([#20847](https://github.com/ethereum/go-ethereum/pull/20847)).
- Add support for exposing/exporting metrics from `geth import` runs ([#20738](https://github.com/ethereum/go-ethereum/pull/20738)).
- Add `debug_accountRange` RPC API to iterate over the accounts ([#19645](https://github.com/ethereum/go-ethereum/pull/19645)).
- Write up the documentation for the `checkpoint-admin` command ([#20697](https://github.com/ethereum/go-ethereum/pull/20697)).
- Change DNS discovery record TTLs to saner values ([#20801](https://github.com/ethereum/go-ethereum/pull/20801), [#20819](https://github.com/ethereum/go-ethereum/pull/20819), [#20820](https://github.com/ethereum/go-ethereum/pull/20820)).
- General cleanups in the `ecies` crypto package used by `RLPx`([#20836](https://github.com/ethereum/go-ethereum/pull/20836)).

And a summary of the bugs we've been fixing:

- Improve node shutdown, avoiding a crash in fast sync ([#20695](https://github.com/ethereum/go-ethereum/pull/20695)).
- Fix filtering for event topics consisting of negative integers ([#20865](https://github.com/ethereum/go-ethereum/pull/20865)).
- Fix and clean up database iteration with prefixes and start keys ([#20808](https://github.com/ethereum/go-ethereum/pull/20808)).
- Fix an issue in `eth_call` that changed the balance of the caller ([#20783](https://github.com/ethereum/go-ethereum/pull/20783)).
- Fix a crash in initial fast sync when also mining at the same time ([#20780](https://github.com/ethereum/go-ethereum/pull/20780)).
- Fix the RPC startup logs to display the actual port if requested `0` ([#20789](https://github.com/ethereum/go-ethereum/pull/20789)).
- Fix a light client deadlock that caused the testnet faucets to freeze up ([#20828](https://github.com/ethereum/go-ethereum/pull/20828)).
- Fix a data race in the ancient database between retrieval and shutdown ([#20919](https://github.com/ethereum/go-ethereum/pull/20919)).
- Fix an annoying `duktape` build warning that many believed was an error ([#20777](https://github.com/ethereum/go-ethereum/pull/20777)).
- Fix an iteration issue in `rawdb` tables that returned wrong keys (unused code) ([#20703](https://github.com/ethereum/go-ethereum/pull/20703)).
- Fix an RPC regression in the `clique` namespace that broke default arguments ([#20781](https://github.com/ethereum/go-ethereum/pull/20781)).
- Fix an annoyance that rejected `--rpcapi=rpc` (not that you would need to use this) ([#20776](https://github.com/ethereum/go-ethereum/pull/20776)).
- Fix an iOS build issue caused by CPU metrics relying on an unavailable kernel header ([#20816](https://github.com/ethereum/go-ethereum/pull/20816)).

For a full rundown of the changes please consult the Geth 1.9.13 [release milestone](https://github.com/ethereum/go-ethereum/milestone/99?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.12) (2020-03-16)

Geth v1.9.12 is a small bugfix release, mostly to keep a semi-regular schedule.

**One small breaking change in the release is that `eth_call` will *not* default to your first account any more if you don't explicitly specify a sender. This was done to avoid the same input behaving differently in different environments. You should never do `eth_call` without explicitly setting a sender in the first place.**

Changes:

- Default to the zero (`0x00...0`) account for `eth_call` sender if none was specified ([#20702](https://github.com/ethereum/go-ethereum/pull/20702)).
- Add missing `CallOpts.SetFrom` for mobile to permit setting a sender on calls ([#20721](https://github.com/ethereum/go-ethereum/pull/20721)).
- Decouple constants in two EIPs applied together in Istanbul ([#20646](https://github.com/ethereum/go-ethereum/pull/20646)).

Fixes:

- Fix a console regression that lost support for `escape` and `unescape` ([#20700](https://github.com/ethereum/go-ethereum/pull/20700)).
- Fix failing CI runs due to randomness in tx fetcher scenario tests ([#20712](https://github.com/ethereum/go-ethereum/pull/20712)).
- Fix a goroutine leak in transaction propagation ([#20762](https://github.com/ethereum/go-ethereum/pull/20762)).
- Fix a possible data race in the downloader ([#20690](https://github.com/ethereum/go-ethereum/pull/20690)).

The release also includes a few changes towards supporting Go 1.14, but the switch-over is postponed until Go 1.14.1 is released due to an issue in Go around faulty Linux kernel detection.

For a full rundown of the changes please consult the Geth 1.9.12 [release milestone](https://github.com/ethereum/go-ethereum/milestone/98?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.11) (2020-02-18)

Geth v1.9.11 was planned to be our next regular maintenance release, but enough niceties have piled up for it to become a feature release. This is also the reason why we delayed it by two weeks compared to our regular schedule.

The release ships three important new features:

 * [DNS-based peer discovery](https://eips.ethereum.org/EIPS/eip-1459) is now enabled in Geth ([#20592](https://github.com/ethereum/go-ethereum/pull/20592), [#20660](https://github.com/ethereum/go-ethereum/pull/20660)). From now on Geth nodes have two independent mechanisms to find peers. The DNS lists serve as a fallback mechanism when peers cannot be found through the DHT. 

   DNS-based discovery is a centralized mechanism, but we have tried to make the operation of this mechanism as transparent and permissionless as possible. The public lists used by default are generated by crawling the discovery DHT. At this time, there are ~1150 publicly routed Ethereum mainnet nodes in the default list. Our public lists also serve the Ropsten, Goerli and Rinkeby test networks. Nodes running any Ethereum client which implements [EIP-868](https://eips.ethereum.org/EIPS/eip-868) and [EIP-2124](https://eips.ethereum.org/EIPS/eip-2124) will appear in the public lists automatically.

   You can disable the use of DNS-based discovery using the `--discovery.dns ""` flag combination. 

   If you want to create a DNS-based node list for your private or public network, please check out our [DNS Discovery Setup guide](https://geth.ethereum.org/docs/developers/dns-discovery-setup). We hope that organizations other than Ethereum Foundation will provide public lists in the future and will happily integrate those lists into the default one using the 'link' feature of EIP-1459.

 * Transaction announcements via `eth/65` ([EIP 2464](https://eips.ethereum.org/EIPS/eip-2464)) is now implemented and Geth<->Geth connections should use significantly less bandwidth ([#20234](https://github.com/ethereum/go-ethereum/pull/20234)) for exchanging transactions. Final numbers are anybody's guess though, as we need to wait for widespread network deployment. This feature depends on the [`eth/64`](https://eips.ethereum.org/EIPS/eip-2364) and [`eth/65`](https://eips.ethereum.org/EIPS/eip-2464) protocol updates, which are not yet supported in all Ethereum client implementations. While connections between compatible clients will use the new protocol, geth will remain compatible with `eth/63` until the new protocol versions are sufficiently adopted by the public network.

 * The JavaScript engine underlying the Geth console and Clef rule engine was switched from [Otto](https://github.com/robertkrimen/otto) to [Goja](https://github.com/dop251/goja), which should bring it up to ECMAScript 5.1+ compliance. Not your latest and greatest .js environment of course, but significantly better and faster than before ([#20470](https://github.com/ethereum/go-ethereum/pull/20470), [#20599](https://github.com/ethereum/go-ethereum/pull/20599)).

Minor features and fixes contained in the release:

 * Shave a few milliseconds off of each block via internal trie optimizations ([#20481](https://github.com/ethereum/go-ethereum/pull/20481), [#20488](https://github.com/ethereum/go-ethereum/pull/20488)).
 * Optimize EVM `BLOCKHASH` opcode execution to handle the worst case better ([#20589](https://github.com/ethereum/go-ethereum/pull/20589)).
 * Check for RPC API namespace availability to detect typos in `--rpcapi` &co ([#20597](https://github.com/ethereum/go-ethereum/pull/20597)).
 * Add `geth dumpgenesis` to print the full genesis and chain config of a node ([#20191](https://github.com/ethereum/go-ethereum/pull/20191)).
 * Revert pending block number reporting during block retrieval ([#20616](https://github.com/ethereum/go-ethereum/pull/20616)).
 * Fix a JavaScript tracer panic when accessing illegal memory ([#20612](https://github.com/ethereum/go-ethereum/pull/20612)).
 * Fix an RPC connectivity issue around flaky connections ([#20414](https://github.com/ethereum/go-ethereum/pull/20414)).
 * Clean up C++ mainnet and Geth Görli bootnodes ([#20610](https://github.com/ethereum/go-ethereum/pull/20610)).
 * Fix `bytes32` and `bytes32[]` support in Clef ([#20609](https://github.com/ethereum/go-ethereum/pull/20609)).

For a full rundown of the changes please consult the Geth 1.9.11 [release milestone](https://github.com/ethereum/go-ethereum/milestone/97?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.10) (2020-01-20)

Geth v1.9.10 is one of our usual maintenance releases. To make the first release of 2020 a bit more special though, we've also [enabled the light client checkpoint oracle on mainnet](https://etherscan.io/address/0x9a9070028361f7aabeb3f2f2dc07f82c4a98a02a) (been running on all test nets for over 6 months now)! 

Among the various tiny tweaks, more notable ones are:

- Integrate DNS discovery, don't activate yet ([#20437](https://github.com/ethereum/go-ethereum/pull/20437), [#20524](https://github.com/ethereum/go-ethereum/pull/20524)).
- Bump the propagated transaction cap to 64KB ([#20352](https://github.com/ethereum/go-ethereum/pull/20352), [#20555](https://github.com/ethereum/go-ethereum/pull/20555)).
- Add propagated block integrity check before forwarding ([#20546](https://github.com/ethereum/go-ethereum/pull/20546)).
- Continued removing deprecated types from the codebase ([#20312](https://github.com/ethereum/go-ethereum/pull/20312)).
- Optimize memory allocations during trie traversal in hashing ([#20529](https://github.com/ethereum/go-ethereum/pull/20529)).
- Handle an ABI ambiguity between Solidity and Vyper outputs ([#20419](https://github.com/ethereum/go-ethereum/pull/20419)).
- Support WebSocket dialing from Go with custom HTTP configs ([#20471](https://github.com/ethereum/go-ethereum/pull/20471)).
- Extend the ABI decoder to handle Solidity 6 view/pure modifiers ([#20482](https://github.com/ethereum/go-ethereum/pull/20482)).
- Reorganize chain database writes to be less vulnerable to crashes ([#20287](https://github.com/ethereum/go-ethereum/pull/20287)).
- Support exporting a block interval via RPC too, not just through the CLI ([#20107](https://github.com/ethereum/go-ethereum/pull/20107)).

And of course, fixes:

- Fix issue where light servers disconnected each other ([#20453](https://github.com/ethereum/go-ethereum/pull/20453)).
- Fix a goroutine leak in whisper v6 implementation. ([#20520](https://github.com/ethereum/go-ethereum/pull/20520)).
- Fix discovery bootnode-fallback on blocked UDP ([#20573](https://github.com/ethereum/go-ethereum/pull/20573)).
- Fix an RPC incompatibility for pending blocks ([#20460](https://github.com/ethereum/go-ethereum/pull/20460)).

For a full rundown of the changes please consult the Geth 1.9.10 [release milestone](https://github.com/ethereum/go-ethereum/milestone/96?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.9) (2019-12-06)

**Geth v1.9.9 is yet another hard fork release!** Don't worry, it's not a hotfix for Istanbul. v1.9.9 [ships and enables](https://github.com/ethereum/go-ethereum/pull/20347) the next hard fork, [Muir Glacier](https://eips.ethereum.org/EIPS/eip-2387), **scheduled for block #9200000, expected around the 6th of January, 2020** (and block #7117117 on Ropsten, expected more or less around the same time).

Apart from Muir Glacier, the release:

- Simplifies and cleans up the internals of the `miner` package ([#20335](https://github.com/ethereum/go-ethereum/pull/20335), [#19396](https://github.com/ethereum/go-ethereum/pull/19396)).
- Fixes a regression in restoring a deleted chain from the freezer ([#20403](https://github.com/ethereum/go-ethereum/pull/20403)).
- Fixes a data race in the new iterator based discovery code ([#20421](https://github.com/ethereum/go-ethereum/pull/20421)).
- Fixes a pointer reference issue in ABI `big.Int` decoding ([#20412](https://github.com/ethereum/go-ethereum/pull/20412)).
- Fixes the help message of certain `geth` subcommands ([#20203](https://github.com/ethereum/go-ethereum/pull/20203)).

For a full rundown of the changes please consult the Geth 1.9.9 [release milestone](https://github.com/ethereum/go-ethereum/milestone/95?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.8) (2019-11-26)

Geth v1.9.8 is yet another biweekly maintenance release, but that does mean it only contains fixes! Two highlights of the release are switching the entire code base over to Go modules ([#20311](https://github.com/ethereum/go-ethereum/pull/20311)); and replacing the trie cacher, slashing memory consumption on mainnet - using default configs - by 1GB ([#19971](https://github.com/ethereum/go-ethereum/pull/19971)).

*Note: [Go modules](https://blog.golang.org/using-go-modules) were introduced in Go 1.11 and greatly polished throughout Go 1.13. Although Geth builds fine with Go 1.11 and Go 1.12 too, we recommend running at least Go 1.13 as it is more flexible in handling various circumstances. If you are using `go-ethereum` as a library, converting your project to Go modules via `go mod init` will make dependency management a lot less of a hassle than the vendor approach. Be aware, however, that Go modules require network access if the dependencies aren't yet cached locally.*

Other notable changes:

- Switch the deprecated web socket library to `github.com/gorilla/websocket` ([#20283](https://github.com/ethereum/go-ethereum/pull/20283), [#20289](https://github.com/ethereum/go-ethereum/pull/20289)).
- Deprecate the `gometalinter` in favor of `golangci-lint` ([#20295](https://github.com/ethereum/go-ethereum/pull/20295) + [dozens of linter fixes](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+staticcheck+is%3Aclosed)).
- Support aliases to manually resolve name clashes in `abigen` generated code ([#20244](https://github.com/ethereum/go-ethereum/pull/20244)).
- Add `clique_status` API to quickly glance the health of Clique networks ([#20103](https://github.com/ethereum/go-ethereum/pull/20103)).
- Initial version of a light server priority API to allow incentivized services ([#20070](https://github.com/ethereum/go-ethereum/pull/20070)).
- Make `puppeth` SSH key confirmation friendlier with bad user input ([#20350](https://github.com/ethereum/go-ethereum/pull/20350)).
- Support loading transaction input from files in the `evm` utility ([#20273](https://github.com/ethereum/go-ethereum/pull/20273)).
- Allow digits inside of labels for the internal assembler ([#20362](https://github.com/ethereum/go-ethereum/pull/20362)).

For a full rundown of the changes please consult the Geth 1.9.8 [release milestone](https://github.com/ethereum/go-ethereum/milestone/94?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.7) (2019-11-07)

Geth v1.9.7 is our almost-usual biweekly maintenance release (one release skipped due to Devcon).

**This release is a bit more special, however, as it finally [initializes](https://github.com/ethereum/go-ethereum/pull/20222) the Ethereum mainnet Istanbul fork block 9069000, targeted to arrive around the 4th of December! Please update your mainnet Geth nodes to v1.9.7 (or newer) as soon as possible to avoid surprises!**

Beside hard-coding Istanbul, we've shipped:

- Enforce proper fork orders to avoid partially initialized private networks ([#20169](https://github.com/ethereum/go-ethereum/pull/20169)).
- Deploy `eth/64`, extending the protocol handshake with a `forkid` field ([#20140](https://github.com/ethereum/go-ethereum/pull/20140)).
- Integrate Istanbul configuration support into `puppeth` for private networks ([#19926](https://github.com/ethereum/go-ethereum/pull/19926)).
- Small tweaks to the ABI to Go binding generator to correctly handle more types ([#20179](https://github.com/ethereum/go-ethereum/pull/20179)).
- Various internal light client capacity accounting optimizations for fairer servicing ([#20077](https://github.com/ethereum/go-ethereum/pull/20077)).
- Remove some unnecessary redundancy from internal state sync data structures ([#19929](https://github.com/ethereum/go-ethereum/pull/19929)).
- Avoid some unnecessary memory copying in the EVM, speeding up certain opcodes ([#20177](https://github.com/ethereum/go-ethereum/pull/20177)).
- Extend the `forkid` validation to catch two more corner cases, courtesy of @ritzdorf ([#20220](https://github.com/ethereum/go-ethereum/pull/20220)).
- Continue work towards the DNS and ENR based secondary discovery protocol ([#20168](https://github.com/ethereum/go-ethereum/pull/20168), [#20132](https://github.com/ethereum/go-ethereum/pull/20132)).
- Rework the PPA dependency management, supporting the latest Go releases from now on ([#20240](https://github.com/ethereum/go-ethereum/pull/20240)).

And fixed:

- Fix a tiny data race in the downloader ([#20204](https://github.com/ethereum/go-ethereum/pull/20204)).
- Fix some parsing issues in the EVM assembly parser ([#20210](https://github.com/ethereum/go-ethereum/pull/20210)).
- Fix IPC paths in Clef on Windows to use proper Windows pipes and not Unix pipes ([#20166](https://github.com/ethereum/go-ethereum/pull/20166)).
- Fix an issue where 2 config file fields were always overwritten by the CLI, even if unset ([#20167](https://github.com/ethereum/go-ethereum/pull/20167)).
- Fix the `evm` binary so that it gracefully ignores useless whitespace around code snippets ([#20211](https://github.com/ethereum/go-ethereum/pull/20211)).
- Fix a gas estimation issue for contracts that check `tx.gasprice` when using the Go bindings ([#20189](https://github.com/ethereum/go-ethereum/pull/20189)).

There is also an ongoing effort of porting over all our documentations from the GitHub Wiki pages, cleaning out stale information, getting relevant things up to date ([#20229](https://github.com/ethereum/go-ethereum/pull/20229)). It's a lot of work and we don't even know where to start, so contributions are more than welcome!

For a full rundown of the changes please consult the Geth 1.9.7 [release milestone](https://github.com/ethereum/go-ethereum/milestone/93?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.6) (2019-10-03)

Geth v1.9.6 is a bug fix release, which improves light client sync to not start from zero when a CHT checkpoint is available ([#20120](https://github.com/ethereum/go-ethereum/pull/20120)).

This release also adds support for EIP-1898: all state-related RPC methods now support specifying block hashes as well as block numbers. This affects `eth_getBalance`, `eth_getStorageAt`, `eth_getTransactionCount`, `eth_getCode`, `eth_call` and `eth_getProof` ([#19491](https://github.com/ethereum/go-ethereum/pull/19491)).

Minor changes:

- LES servers now advertise the les capability via ENR ([#20145](https://github.com/ethereum/go-ethereum/pull/20145))
- New metrics for p2p bandwidth per protocol message ([#20133](https://github.com/ethereum/go-ethereum/pull/20133))
- LevelDB seek compaction is disabled, reducing disk I/O ([#20130](https://github.com/ethereum/go-ethereum/pull/20130))
- Handling of invalid future blocks is improved ([#19763](https://github.com/ethereum/go-ethereum/pull/19763))

For a full rundown of the changes please consult the Geth 1.9.6 [release milestone](https://github.com/ethereum/go-ethereum/milestone/92?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.5) (2019-09-20)

This is a security-critical release that fixes a regression in v1.9.4 where blocks
with invalid state were created by the miner component.

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.4) (2019-09-19)

Geth v1.9.4 is a special maintenance release, that besides fixing issues as usual, also [locks in](https://github.com/ethereum/go-ethereum/pull/20090) the Istanbul hard fork block numbers for the Ropsten, Rinkeby and Görli test networks:

- Ropsten: `6485846`, scheduled October 2, 2019 ([ref](https://twitter.com/hudsonjameson/status/1174336671342575617)).
- Rinkeby: `5435345`, scheduled November 13, 2019 ([ref](https://twitter.com/peter_szilagyi/status/1174580897980592129)).
- Görli: `1561651`, scheduled October 30, 2019 ([ref](https://twitter.com/a4fri/status/1173939316101386240)).

**Please update your nodes on the above test networks before the deadlines, otherwise you will end up either on a wrong chain (Ropsten), or stuck altogether (Rinkeby and Görli). The `--override.istanbul` CLI flag can be used to bail out of the fork even last minute if things go sour.**

---

Improvements:

- Accumulate state writes post-Byzantium and only push into the trie at block end ([#19953](https://github.com/ethereum/go-ethereum/pull/19953)).
- Smarter locking in the transaction pool to avoid some contention ([#20080](https://github.com/ethereum/go-ethereum/pull/20080), [#20081](https://github.com/ethereum/go-ethereum/pull/20081), [#20085](https://github.com/ethereum/go-ethereum/pull/20085)).

Bugfixes:

- Fix a USB HID discovery regression on Windows ([#20092](https://github.com/ethereum/go-ethereum/pull/20092)).
- Various stability and balancing fixes for light servers ([#20079](https://github.com/ethereum/go-ethereum/pull/20079)).
- Fix some RLP decoding issues around `nil`s for discv5 ([#20064](https://github.com/ethereum/go-ethereum/pull/20064)).
- Expose the GraphQL port (8547) from our docker images ([#20033](https://github.com/ethereum/go-ethereum/pull/20033)).
- Fix GraphQL UI serving regression caused by gzip encoding ([#20046](https://github.com/ethereum/go-ethereum/pull/20046)).
- Fix GraphQL `address` type decoding to reject invalid strings ([#20046](https://github.com/ethereum/go-ethereum/pull/20046)).
- Fix P2P metric types so they work properly with Influx/Grafana ([#20047](https://github.com/ethereum/go-ethereum/pull/20047)).

For a full rundown of the changes please consult the Geth 1.9.4 [release milestone](https://github.com/ethereum/go-ethereum/milestone/90?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.3) (2019-09-03)

Geth v1.9.3 is a regular maintenance release, highlights being finalization of the Istanbul EIPs.

New Features:

- Add `eth.fillTransaction` to create-but-not-sign a transaction ([#19915](https://github.com/ethereum/go-ethereum/pull/19915)).
- Support DNS names in enode URLs during `admin.addPeer` ([#18524](https://github.com/ethereum/go-ethereum/pull/18524)).
- Enable gzip compression on the HTTP RPC API endpoint ([#19997](https://github.com/ethereum/go-ethereum/pull/19997)).
- Reduce light client default peer count from 100 to 10 ([#19933](https://github.com/ethereum/go-ethereum/pull/19933)).
- Align CLI flags and descriptions a bit better ([#19956](https://github.com/ethereum/go-ethereum/pull/19956)).

Istanbul changes ([progress tracker](https://github.com/ethereum/go-ethereum/issues/19919)):

- Implement [EIP-152](https://eips.ethereum.org/EIPS/eip-152): Blake2B F compression function precompile ([#19972](https://github.com/ethereum/go-ethereum/pull/19972)).
- Implement [EIP-2028](https://eips.ethereum.org/EIPS/eip-2028): Transaction data gas cost reduction ([#19931](https://github.com/ethereum/go-ethereum/pull/19931)).
- Implement [EIP-2200](https://github.com/ethereum/EIPs/pull/2200): Net SSTORE optimizations ([#19964](https://github.com/ethereum/go-ethereum/pull/19964)).
- Add `--override.istanbul` flag as a failsafe ([#20004](https://github.com/ethereum/go-ethereum/pull/20004)).
- Enable Istanbul EIPs in the EVM jumptable ([#19993](https://github.com/ethereum/go-ethereum/pull/19993)).

Fixed Bugs:

- Update README with the latest fork configs for private networks ([#19983](https://github.com/ethereum/go-ethereum/pull/19983), [#20002](https://github.com/ethereum/go-ethereum/pull/20002)).
- Separate light client and light server P2P handlers internally ([#19639](https://github.com/ethereum/go-ethereum/pull/19639), [#20010](https://github.com/ethereum/go-ethereum/pull/20010)).
- Fix an import crash caused by a data loss from a previous crash ([#19986](https://github.com/ethereum/go-ethereum/pull/19986)).
- Deprecate Ubuntu Cosmic and enable builders for Ubuntu Eoan ([#19955](https://github.com/ethereum/go-ethereum/pull/19955)).
- Fix an annoying log in `geth attach` about cache sanitation ([#19911](https://github.com/ethereum/go-ethereum/pull/19911)).
- Fix `admin.exportChain` to forbid overwriting existing file ([#20019](https://github.com/ethereum/go-ethereum/pull/20019)).
- Update Clef's 4byte database to the most recent version ([#19957](https://github.com/ethereum/go-ethereum/pull/19957)).
- Add more verbose logging to even small chain reorgs ([#18950](https://github.com/ethereum/go-ethereum/pull/18950)).
- Fix iOS builds broken from a `gomobile` update ([#19995](https://github.com/ethereum/go-ethereum/pull/19995)).

For a full rundown of the changes please consult the Geth 1.9.3 [release milestone](https://github.com/ethereum/go-ethereum/milestone/89?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.2) (2019-08-13)

Geth v1.9.2 is a small maintenance release. It introduces some minor features, fixes some minor bugs and starts to lay the groundwork for the Istanbul hard fork.

Features added:

- Rework client tracking for light servers to better prioritize requests, making it fairer for everyone ([#19745](https://github.com/ethereum/go-ethereum/pull/19745)). 
- Avoid retrieving full blocks for GraphQL queries on light clients if the query relies on headers ([#19886](https://github.com/ethereum/go-ethereum/pull/19886)).
- Support closing a simulated backends, avoiding a goroutine leak during log running tests ([#19902](https://github.com/ethereum/go-ethereum/pull/19902)).
- Support `eth_call` overriding chain state before execution, [see API docs](https://geth.ethereum.org/rpc/eth_call) for details ([#19917](https://github.com/ethereum/go-ethereum/pull/19917)).
- Replace all user facing `passphrase` labels with `password` to avoid confusion ([#19932](https://github.com/ethereum/go-ethereum/pull/19932)).

Bugs fixed:

- Fix an RPC annoyance where inclusion info for pending transactions was 0, not `null` ([#19901](https://github.com/ethereum/go-ethereum/pull/19901)).
- Fix an RPC regression from v1.9.1 where the block size was not hex encoded ([#19885](https://github.com/ethereum/go-ethereum/pull/19885)).
- Fix the builder to extract commit hashes from detached git branches too ([#18315](https://github.com/ethereum/go-ethereum/pull/18315)).
- Fix storage trie value encoding when running `geth dump` ([#19943](https://github.com/ethereum/go-ethereum/pull/19943)).
- Fix console output coloring for Clef on Windows ([#19889](https://github.com/ethereum/go-ethereum/pull/19889)).

Istanbul changes ([progress tracker](https://github.com/ethereum/go-ethereum/issues/19919)):

- Refactor the **internal** chain configuration to allow enabling EIPs individually in tests ([#19735](https://github.com/ethereum/go-ethereum/pull/19735)).
- Implement [EIP-1884](https://eips.ethereum.org/EIPS/eip-1884): Repricing for trie-size-dependent opcodes ([#19743](https://github.com/ethereum/go-ethereum/pull/19743)).
- Implement [EIP-1108](https://eips.ethereum.org/EIPS/eip-1108): Reduce alt_bn128 precompile gas costs ([#19904](https://github.com/ethereum/go-ethereum/pull/19904)).
- Implement [EIP-1344](https://eips.ethereum.org/EIPS/eip-1344): ChainID opcode ([#19921](https://github.com/ethereum/go-ethereum/pull/19921)).

For a full rundown of the changes please consult the Geth 1.9.2 [release milestone](https://github.com/ethereum/go-ethereum/milestone/88?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.1) (2019-07-24)

Geth v1.9.1 is a small biweekly maintenance release. For all the major new features in the 1.9 family, please see [our blog post](https://blog.ethereum.org/2019/07/10/geth-v1-9-0/) from two weeks ago.

 - Add `eth_getHeaderByNumber` and `eth_getHeaderByHash` to get headers without bodies ([#19669](https://github.com/ethereum/go-ethereum/pull/19669)).
 - Update the `go-ethereum` [list of authors](https://github.com/ethereum/go-ethereum/blob/master/AUTHORS) to July 22nd state, bringing up the count to 367 ([#19873](https://github.com/ethereum/go-ethereum/pull/19873)).
 - Switch out the websocket library, adding support for pings and continuation frames ([#19866](https://github.com/ethereum/go-ethereum/pull/19866)).
 - Implement a 1/288th grace period for the faucet to avoid request drift (5m for 24h) ([#18105](https://github.com/ethereum/go-ethereum/pull/18105)).
 - Add `debug_accountRange` to query all or part of the accounts at a given root ([#17438](https://github.com/ethereum/go-ethereum/pull/17438)).
 - Fix a crash in the transaction pool caused by the 1.9.0 internal rework ([#19835](https://github.com/ethereum/go-ethereum/pull/19835)).
 - Fix a crash in `geth dumpconfig` caused by the release oracle fields ([#19825](https://github.com/ethereum/go-ethereum/pull/19825)).
 - Fix the reported derivation paths of hardware wallet accounts in Clef ([#19827](https://github.com/ethereum/go-ethereum/pull/19827)).
 - Fix `geth --dev` thinking it's mainnet and requesting 4GB memory ([#19875](https://github.com/ethereum/go-ethereum/pull/19875)).
 - Fix `debug_chaindbProperty`, which returns an internal error ([#19856](https://github.com/ethereum/go-ethereum/pull/19856)).
 - Fix build script on OpenBSD ([#17966](https://github.com/ethereum/go-ethereum/pull/17966)).

For a full rundown of the changes please consult the Geth 1.9.1 [release milestone](https://github.com/ethereum/go-ethereum/milestone/86?closed=1).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.9.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.0) (2019-07-10)

After many months of silence, we're proud to announce the v1.9.0 release of Go Ethereum! Although this release has been in the making for a lot longer than we anticipated, we're confident there will be some juicy feature for everyone to enjoy!

Posting a list of changes here wouldn't do this release justice, so we've prepared an entire blog post to walk through all the major features: [Geth v1.9.0 - Six months distilled](https://blog.ethereum.org/2019/07/10/geth-v1-9-0/).

---

As with all our previous releases, you can find the:

- Pre-built binaries for all platforms on our [downloads page](https://geth.ethereum.org/downloads/).
- Docker images published under [`ethereum/client-go`](https://cloud.docker.com/u/ethereum/repository/docker/ethereum/client-go).
- Ubuntu packages in our [Launchpad PPA repository](https://launchpad.net/~ethereum/+archive/ubuntu/ethereum).
- OSX packages in our [Homebrew Tap repository](https://github.com/ethereum/homebrew-ethereum).

## [v1.8.27](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.27) (2019-04-17)

Geth v1.8.27 is a hotfix release to counter an active attack on mainnet, mounted against newly joining Geth nodes doing fast sync ([#19473](https://github.com/ethereum/go-ethereum/pull/19473)).

 * If you are already in sync with the network, you **don't** need to update.
 * Archive nodes, light clients and full-sync full nodes **don't** need to update.
 * If you are joining the network with a new node, then you **do need** to update.

## Background

Fast sync is susceptible to a grieving version of an eclipse attack, where a malicious remote node attempts to get a new Geth node to fast sync to some small chain, before a real heavy chain is discovered in the network. This results in Geth falling back to full sync for the main chain, taking too much time.

This attack can only be meaningfully mounted against nodes which are properly exposed on a public IP address (i.e. not firewalled, not NATed). Even then, it's a race against the node finding good peers fast enough, in which case the attack doesn't work any more.

There is no economic advantage in pulling this attack off, only causing sync annoyance. That said, there is currently a number of (at least 4 identified, maybe more) Parity nodes at `207.148.5.229`, which are doing variations of this attack. It might be deliberate, or it might also be leftover nodes from some experiment that only have a few blocks on their chain and subsequently disabled sync. A bit unprobable.

## Solution

This release repurposes the DAO challenge to do a checkpoint challenge based on the recently hard coded CHTs. It also makes the challenge stricter, in that while the node is doing a fast-sync, remote peers are not permitted to be synced below the checkpoint block. This should also help sync the chain faster, getting rid of stalling or useless peers.

---

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.26](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.26) (2019-04-10)

Geth v1.8.26 is a tiny light client DoS hotfix. We've dropped support for `les/1` on `master` (v1.9.0 unstable) and after some light servers updated, v1.8.x light clients started crashing. Turned out that `les/2` on the v1.8.x release family relied on some `les/1` throttling data ([#19437](https://github.com/ethereum/go-ethereum/pull/19437)).

*If you don't use the light client, you don't need to update. Light servers don't need to update either.*

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.25) (2019-04-09)

Geth v1.8.25 is a tiny hotfix on top of the recently released v1.8.24. During back-porting PRs into v1.8.24, one was merged incorrectly and resulted disabling 2 CLI flags (`--rpccorsdomain` and `--rpcvhosts`). You can see the fix in [#19416](https://github.com/ethereum/go-ethereum/pull/19416/files). If you are not using these flag, you're fine to remain on the v1.8.24 release.

---

To recap our original notes, Geth ~~v1.8.24~~ v1.8.25 is a small maintenance release aiming to be the final version of the 1.8.x family before rolling out our next major milestone (v1.9.0). As the previous release, ~~v1.8.24~~ v1.8.25 only back-ports stability fixes. A grab bag of the changes included are:

 * Add `--rpc.gascap` to limit the gas `eth_call` and `eth_estimateGas` may use.
 * **Baked in the Rinkeby Petersburg fork block 4321234 (~4th May, 2019).**
 * Updated light client CHTs for quicker sync times on all built-in networks.
 * Fixes the builders and macos file descriptors if using with Go 1.12+.
 * Fixes a log filtering issue that didn't report reorged EVM events.
 * Fixes a couple of networking issues in the DHT and `eth`.
 * Memory optimizations around timestamp handling.

For a full rundown of the changes please consult the **originally [Geth 1.8.24](https://github.com/ethereum/go-ethereum/pull/19370)** back-port pull request.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.24](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.24) (2019-04-08)

**We've found a tiny issue in this release which accidentally disabled two CLI flags around RPC CORS and VHosts handling. If you rely on these, please see the [Geth v1.8.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.25) hotfix release.**

---

Geth v1.8.24 is a small maintenance release aiming to be the final version of the 1.8.x family before rolling out our next major milestone (v1.9.0). As the previous release too, v1.8.24 only back-ports stability fixes. A grab bag of the changes included are:

 * Add `--rpc.gascap` to limit the gas `eth_call` and `eth_estimateGas` may use.
 * **Baked in the Rinkeby Petersburg fork block 4321234 (~4th May, 2019).**
 * Updated light client CHTs for quicker sync times on all built-in networks.
 * Fixes the builders and macos file descriptors if using with Go 1.12+.
 * Fixes a log filtering issue that didn't report reorged EVM events.
 * Fixes a couple of networking issues in the DHT and `eth`.
 * Memory optimizations around timestamp handling.

For a full rundown of the changes please consult the [Geth 1.8.24](https://github.com/ethereum/go-ethereum/pull/19370) back-port pull request.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.23) (2019-02-20)

Geth v1.8.23 is a small, scheduled maintenance release that brings a couple of improvements to various sync and block processing mechanisms. The selection of fixes have been back-ported from `master` and are not a full release of our latest code:

 * Fix transaction tracers to handle legacy Frontier/Homestead state transitions (https://github.com/ethereum/go-ethereum/pull/19029/commits/631e2f07f62372106964352a03a2f793d2fe94dc).
 * Fix block importing regressions caused by the side chain rework (https://github.com/ethereum/go-ethereum/pull/19029/commits/3a95128b22a239c8e7b878a40bb36b003ec36267, https://github.com/ethereum/go-ethereum/pull/19029/commits/3ab9dcc3bd17cdbb731ca7da829a3bd7b5e1bc1e, https://github.com/ethereum/go-ethereum/pull/19029/commits/4da209290831720b0f61754b92dabefd5cb35b6d).
 * Fix a trie decoding issue that could cause a state sync to crash (https://github.com/ethereum/go-ethereum/pull/19029/commits/a458153098d6f66ee0c6f990f31b3646ad171bbb).
 * Fix the file descriptor crash on newer versions of macos (https://github.com/ethereum/go-ethereum/pull/19029/commits/048b463b301b1ac61be187d7195665d4ad31f51f, https://github.com/ethereum/go-ethereum/pull/19029/commits/7bd6f39dc3eb174d5869613bc24f431ec96dabff).
 * Fix the PPA that broke due to Travis' firewall (https://github.com/ethereum/go-ethereum/pull/19029/commits/9f5fb15097198ea20aaae31983a7101ac0679eaa, https://github.com/ethereum/go-ethereum/pull/19029/commits/276f824707783ecbe092e521b5a7460515ee3e99, https://github.com/ethereum/go-ethereum/pull/19029/commits/b247052a64a4954de83e50970c1551efe9f8a08b, https://github.com/ethereum/go-ethereum/pull/19029/commits/84cb00a94d060b76df997f7599b312c8d30db893, https://github.com/ethereum/go-ethereum/pull/19029/commits/df355eceb41f4beec611faea0df617ea59980bef).
 * Fix the iOS CocoaPods builds failing due to upstream dependency issues (https://github.com/ethereum/go-ethereum/pull/19029/commits/d9be33766930b6978629a02643a8c83265a3006b, https://github.com/ethereum/go-ethereum/pull/19029/commits/fe5258b41ea2e363aea1494dcf52754fea9b42db).
 * Fix various data races in the cache library we used for trie reads (https://github.com/ethereum/go-ethereum/pull/19029/commits/992a7bbad531b9f44e84dfe5a779e7a6146d194d).

The release also back-ports native support for the Görli testnet via the `--goerli` flag (https://github.com/ethereum/go-ethereum/pull/19029/commits/2072c26a96badbe45d6df56a4cd68ffd1b6fb12e).

For a full rundown of the changes please consult the [Geth 1.8.23](https://github.com/ethereum/go-ethereum/pull/19029) back-port pull request.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.11 with the following improvements:

- Improved peer suggestion engine ([#18404](https://github.com/ethereum/go-ethereum/pull/18404)).
- Bootnode mode and new bootnodes ([#18498](https://github.com/ethereum/go-ethereum/pull/18498), [#18509](https://github.com/ethereum/go-ethereum/pull/18509)).
- Network snapshot generator ([#18453](https://github.com/ethereum/go-ethereum/pull/18453)).
- Saturation check for healthy networks ([#19071](https://github.com/ethereum/go-ethereum/pull/19071)).
- PSS: Updated to whisper V6 ([#19023](https://github.com/ethereum/go-ethereum/pull/19023)).
- Added a debug tool: global-store ([#19014](https://github.com/ethereum/go-ethereum/pull/19014)).
- Added debugging APIs to make it easier to explore chunks on nodes ([#18980](https://github.com/ethereum/go-ethereum/pull/18980), [#19002](https://github.com/ethereum/go-ethereum/pull/19002), [#19008](https://github.com/ethereum/go-ethereum/pull/19008)).
- Multiple data races were fixed ([#18459](https://github.com/ethereum/go-ethereum/pull/18459), [#18460](https://github.com/ethereum/go-ethereum/pull/18460), [#18462](https://github.com/ethereum/go-ethereum/pull/18462), [#18464](https://github.com/ethereum/go-ethereum/pull/18464),[#18511](https://github.com/ethereum/go-ethereum/pull/18511), [#18993](https://github.com/ethereum/go-ethereum/pull/18993), [#19017](https://github.com/ethereum/go-ethereum/pull/19017), [#19049](https://github.com/ethereum/go-ethereum/pull/19049), [#19051](https://github.com/ethereum/go-ethereum/pull/19051)).


**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.22) (2019-01-31)

Geth v1.8.22 re-enables all Constantinople changes and contains an additional fork, Petersburg,
to disable EIP-1283. This procedure is meant to ease the transition on networks like Ropsten where
the Constantinople transition had already taken place when an issue with EIP-1283 was discovered.
On the main network, Constantinople and Petersburg activate at the same time.

The block numbers are:

- 7280000 for Constantinople on Mainnet
- 7280000 for Petersburg on Mainnet
- 4939394 for Petersburg on Ropsten

Note that the Petersburg change is set to activate simultaneously with Constantinople unless 
configured otherwise. **If you are running a private network, you must schedule the Petersburg 
fork through chain configuration before updating to v1.8.22 if Constantinople has already
activated on your network.**

In addition to the consensus changes, this release contains peer-to-peer networking security
improvements in the peer discovery subsystem.

List of all changes:

- core, cmd/puppeth: implement constantinople fix, disable EIP-1283 (#18486)
- p2p/discover, p2p/enode: rework endpoint proof handling, packet logging (#18963)
- p2p/discover: improve table addition code (#18974)
- travis, appveyor: bump to Go 1.11.5 (#18947)

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.21) (2019-01-15)

Geth 1.8.21 is an **emergency hotfix release to [postpone](https://github.com/ethereum/go-ethereum/pull/18454) the mainnet Constantinople upgrade**! The reason is a last minute [reentrancy vulnerability](https://medium.com/chainsecurity/constantinople-enables-new-reentrancy-attack-ace4088297d9) dicovered by ChainSecurity.

If you don't feel comfortable with upgrading to Geth 1.8.21, you also have the option to:

* **Downgrade** to Geth 1.8.19; or
* **Continue running** Geth 1.8.20 using the `--override.constantinople=9999999` flag.

See [details here](https://gitter.im/ethereum/AllCoreDevs?at=5c3e29b4f780a1521f15ef37).

---

Beside the emergency Constantinople delay, the release also contains some tweaks:

 * Return the pending nonce from the transaction pool, not the pending block ([#15794](https://github.com/ethereum/go-ethereum/pull/15794)).
 * Accept `application/json-rpc` too as the RPC API content type ([#18310](https://github.com/ethereum/go-ethereum/pull/18310)).
 * Sanitize transaction pool config to avoid invalid user settings ([#17210](https://github.com/ethereum/go-ethereum/pull/17210)).
 * Convert ABI names to properly camel-cased names ([#16513](https://github.com/ethereum/go-ethereum/pull/16513), [#18372](https://github.com/ethereum/go-ethereum/pull/18372)).
 * Warn the user when the IPC path is too long for the kernel ([#18330](https://github.com/ethereum/go-ethereum/pull/18330)).
 * Support CALLs from the Go APIs against specific blocks ([#17942](https://github.com/ethereum/go-ethereum/pull/17942)).
 * Bubble up internal ABI parsing errors to the caller ([#17328](https://github.com/ethereum/go-ethereum/pull/17328)).
 * Support dumping the CLI configs directly to a file ([#18327](https://github.com/ethereum/go-ethereum/pull/18327)).
 * Switch Keccak implementation to upstream Go ([#18390](https://github.com/ethereum/go-ethereum/pull/18390)).
 * Add CREATE2 support to the `callTracer` ([#18318](https://github.com/ethereum/go-ethereum/pull/18318)).
 * Bump all builders to Go 1.11.4 ([#18314](https://github.com/ethereum/go-ethereum/pull/18314)).


And some bugfixes:

 * Fix a downloader panic caused by the side chain import rework ([#18335](https://github.com/ethereum/go-ethereum/pull/18335)).
 * Fix an ABI unpacking issue when parsing 2 dimensional slices ([#18321](https://github.com/ethereum/go-ethereum/pull/18321), [#18364](https://github.com/ethereum/go-ethereum/pull/18364)).
 * Fix a discovery protocol error when looking up remote peers ([#18309](https://github.com/ethereum/go-ethereum/pull/18309)).
 * Fix puppeth panic when using degenerate genesis configs ([#18344](https://github.com/ethereum/go-ethereum/pull/18344)).
 * Fix compilation regression on PPC64 processors ([#18376](https://github.com/ethereum/go-ethereum/pull/18376)).
 * Fix broken database version tracking ([#18429](https://github.com/ethereum/go-ethereum/pull/18429)).

For a full rundown of the changes please consult the [Geth 1.8.21](https://github.com/ethereum/go-ethereum/milestone/84?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

## [v1.8.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.20) (2018-12-11)

Geth v1.8.20 is a bit of a special release. On one hand it's the release that finally enables the Constantinople hard fork on mainnet at block 7080000 (and Rinkeby at block 3660663). It's also our last *planned* release of the 1.8 family (we'll still do hotfixes if need be), meaning that **we'll start merging backwards incompatible changes onto `master`** in preparation of Geth 1.9.0 (we don't have an ETA for it, but January the earliest).

Notable changes in the release are:

 * Hard code the mainnet and Rinkeby Constantinople hard-fork block numbers ([#18268](https://github.com/ethereum/go-ethereum/pull/18268))
 * Allow overriding the Constantinople hard-fork block from the command line ([#18273](https://github.com/ethereum/go-ethereum/pull/18273)).
 * Remove the artificial 2K file descriptor limit from Geth, use all allowed ([#18211](https://github.com/ethereum/go-ethereum/pull/18211)).
 * Support exporting genesis specs to Parity and Aleth from Puppeth ([#18172](https://github.com/ethereum/go-ethereum/pull/18172)).
 * Support the new (1.23) `docker_compose` APIs change in Puppeth ([18281](https://github.com/ethereum/go-ethereum/pull/18281)).
 * Enforce lowercase network names on Puppeth private networks ([#18235](https://github.com/ethereum/go-ethereum/pull/18235)).
 * Support standard-tracing transactions straight to the filesystem ([#17914](https://github.com/ethereum/go-ethereum/pull/17914)).
 * Support whitelisting blocks help chain selection during forks ([#18028](https://github.com/ethereum/go-ethereum/pull/18028)).
 * Support big number constructors on the mobile libraries ([#17828](https://github.com/ethereum/go-ethereum/pull/17828)).
 * Support arrays of dynamic types in `abigen` and `abi` ([#18051](https://github.com/ethereum/go-ethereum/pull/18051)).
 * Update `go-leveldb` to reduce CPU overhead a bit ([#18205](https://github.com/ethereum/go-ethereum/pull/18205)).
 * Warn when using deprecated config files ([#18199](https://github.com/ethereum/go-ethereum/pull/18199)).

Notable bug fixes are:

 * Fully disable the USB HID on macos on `--nousb`, avoiding rare crashes ([#18213](https://github.com/ethereum/go-ethereum/pull/18213)).
 * Fix a tracer error in the pre-state JavaScript tracer ([#18253](https://github.com/ethereum/go-ethereum/pull/18253)).
 * Fix the Puppeth faucet connectivity issue ([18281](https://github.com/ethereum/go-ethereum/pull/18281)).

For a full rundown of the changes please consult the [Geth 1.8.20](https://github.com/ethereum/go-ethereum/milestone/83?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.8, which contains:

 * Uniform PSS API ([18218](https://github.com/ethereum/go-ethereum/pull/18218)).
 * P2P simulations snapshot improvements ([18220](https://github.com/ethereum/go-ethereum/pull/18220)).

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.19) (2018-11-28)

Geth v1.8.19 is our biweekly maintenance release, this current one delivering some notable performance improvements beside the usual polishes and bugfixes:

 * Trie read cache, increasing full-sync speed by 15% and in-sync processing by 30% ([#18087](https://github.com/ethereum/go-ethereum/pull/18087)).
 * Rework downloader common ancestor lookup, reducing network bandwidth ([#18085](https://github.com/ethereum/go-ethereum/pull/18085)).
 * Avoid light servers wasting trie hashers and recreating them all the time ([#18116](https://github.com/ethereum/go-ethereum/pull/18116)).
 * Limit the number of open file descriptors Whisper may consume ([#18142](https://github.com/ethereum/go-ethereum/pull/18142)).
 * More robust side chain importing code for very long chains ([#17973](https://github.com/ethereum/go-ethereum/pull/17973)).
 * Improve the logs when the node encounters a bad block ([#18156](https://github.com/ethereum/go-ethereum/pull/18156)).
 * Enable Constatinople in developers chains ([#18162](https://github.com/ethereum/go-ethereum/pull/18162), [#18179](https://github.com/ethereum/go-ethereum/pull/18179)).

Notable bugfixes:

 * Fix a garbage collector flaw that lead to data corruption with high memory allowance ([#18165](https://github.com/ethereum/go-ethereum/pull/18165)).
 * Fix a light client fetcher issue that sometimes resulted in sync getting stuck ([#18072](https://github.com/ethereum/go-ethereum/pull/18072)).
 * Fix light client ancestor lookup issue surfaced by the downloader rework ([#18196](https://github.com/ethereum/go-ethereum/pull/18196)).
 * Fix a light client issue that stored invalid servers in its node database ([#18093](https://github.com/ethereum/go-ethereum/pull/18093)).

For a full rundown of the changes please consult the [Geth 1.8.19](https://github.com/ethereum/go-ethereum/milestone/82?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.7, which contains many bugfixes:

 * Add new database abstractions (shed package) ([18183](https://github.com/ethereum/go-ethereum/pull/18183)).
 * Remove multihash from Swarm Feeds ([18175](https://github.com/ethereum/go-ethereum/pull/18175)).
 * Refactor PSS message handler ([18169](https://github.com/ethereum/go-ethereum/pull/18169)).
 * Fix New function for-loop scope in simulation package ([18161](https://github.com/ethereum/go-ethereum/pull/18161)).
 * Add accounting metrics persistence ([18136](https://github.com/ethereum/go-ethereum/pull/18136)).
 * Fix batch writes in database migration ([18113](https://github.com/ethereum/go-ethereum/pull/18113)).
 * Use simulations.Event in simulation package ([18098](https://github.com/ethereum/go-ethereum/pull/18098)).
 * Fix Kademlia neighborhood depth in network package ([18066](https://github.com/ethereum/go-ethereum/pull/18066)).

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.18) (2018-11-14)

Geth v1.8.18 is our biweekly maintenance release (postponed by 2 weeks due to devcon iv). It is a tiny release focusing mostly on polishes, test speedups and a handful of new features.

 * Emit warning logs on unsuccessful remote account accesses ([#17887](https://github.com/ethereum/go-ethereum/pull/17887)).
 * Extend `eth_getWork` miner packages with the block number ([#18038](https://github.com/ethereum/go-ethereum/pull/18038)).
 * Start generating digitally signed Ethereum node records ([#17753](https://github.com/ethereum/go-ethereum/pull/17753)).
 * Support using `go-ethereum` as a WebAssembly library ([#17709](https://github.com/ethereum/go-ethereum/pull/17709)).
 * Minor EVM `EXP` shortcuts to make common cases fast ([#16851](https://github.com/ethereum/go-ethereum/pull/16851)).
 * Implement Merkle proof retriever RPC APIs ([#17737](https://github.com/ethereum/go-ethereum/pull/17737), [#17965](https://github.com/ethereum/go-ethereum/pull/17965)).
 * Add the refund amount to the standard json traces ([#17910](https://github.com/ethereum/go-ethereum/pull/17910)).
 * Capture source maps via solc compiler wrapper ([#18020](https://github.com/ethereum/go-ethereum/pull/18020)).
 * Support setting `GOMAXPROCS` from an env var ([#17148](https://github.com/ethereum/go-ethereum/pull/17148)).
 * Support encrypting the Clef master seed ([#17704](https://github.com/ethereum/go-ethereum/pull/17704)).
 * Bump most Go builders to 1.11.2 ([#18031](https://github.com/ethereum/go-ethereum/pull/18031)).

Notable bugfixes:

 * Fix a downloader issue to better measure remote capacity ([#17983](https://github.com/ethereum/go-ethereum/pull/17983)).
 * Fix an RPC race on concurrent subscribe/notification ([#17874](https://github.com/ethereum/go-ethereum/pull/17874)).
 * Fix an RPC data race around unsubscribe/close ([#17894](https://github.com/ethereum/go-ethereum/pull/17894)).
 * Fix bootnode enode output on `-writeaddress` ([#17932](https://github.com/ethereum/go-ethereum/pull/17932)).
 * Fix a deadlock in the p2p simulator framework ([#17891](https://github.com/ethereum/go-ethereum/pull/17891)).
 * Fix `ethclient` log filtering via block hashes ([#17996](https://github.com/ethereum/go-ethereum/pull/17996)).

For a full rundown of the changes please consult the [Geth 1.8.18](https://github.com/ethereum/go-ethereum/milestone/79?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.6, which among many bugfixes features:

 * Swarm light node mode: disable sync, retrieve, subscription messages ([17899](https://github.com/ethereum/go-ethereum/pull/17899)).
 * Bugfix: Swarm Feed update with empty signature returns HTTP 200 ([18008](https://github.com/ethereum/go-ethereum/pull/18008)).
 * Initial P2P accounting for message exchange ([17951](https://github.com/ethereum/go-ethereum/pull/17951)).

*Note that a bug was fixed within the local Swarm database implementation, which requires a cleanup procedure to be run upon Swarm startup. This means that your Swarm node might take a while to start once you upgrade to this version.*

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.17) (2018-10-09)

Geth v1.8.17 is our regular maintenance release, which among numerous bugfixes also features the full Constantinople feature set and enables it on the Ropsten testnet at block #4230000.

 * Update the chain indexer to react only to head events opposed to all blocks ([#17826](https://github.com/ethereum/go-ethereum/pull/17826)).
 * Deprecate building with Go 1.9 and update builders to Go 1.11.1 ([#17807](https://github.com/ethereum/go-ethereum/pull/17807), [#17820](https://github.com/ethereum/go-ethereum/pull/17820)).
 * Expose the `enode` URL of a peer through the `admin.peers` endpoint ([#17838](https://github.com/ethereum/go-ethereum/pull/17838)).
 * Optimize CREATE and CREATE2 to do a bit less Keccak256 hashing ([#17806](https://github.com/ethereum/go-ethereum/pull/17806)).
 * Optimize the `SHA3` opcode to do fewer allocs, reaching 2x speed ([#17863](https://github.com/ethereum/go-ethereum/pull/17863)).
 * Implement a lower bound on block propagation targets to 4 peers ([#17725](https://github.com/ethereum/go-ethereum/pull/17725)).
 * Drop the `goimports` dependency for running `abigen` ([#17768](https://github.com/ethereum/go-ethereum/pull/17768).
 * Extend `abigen` to support piping `solc` output into it ([#17648](https://github.com/ethereum/go-ethereum/pull/17648)).
 * Use hex addresses in EVM assembly dumps ([#17870](https://github.com/ethereum/go-ethereum/pull/17870)).

Notable bugfixes:

 * Fix an annoying 2-3 minute hang when completing initial fast sync ([#17825](https://github.com/ethereum/go-ethereum/pull/17825)).
 * Fix the infamous `invalid hash chain` error during initial sync ([#17839](https://github.com/ethereum/go-ethereum/pull/17839)).
 * Fix flag parsing to allow archive nodes serving light clients ([#17803](https://github.com/ethereum/go-ethereum/pull/17803)).
 * Fix a `puppeth` regression causing invalid enode errors ([#17802](https://github.com/ethereum/go-ethereum/pull/17802)).

For a full rundown of the changes please consult the [Geth 1.8.17](https://github.com/ethereum/go-ethereum/milestone/78?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.5, which among many bugfixes features:

 * Swarm Feeds (previously knowns as MRU): Adaptive frequency / Predictable lookups ([17559](https://github.com/ethereum/go-ethereum/pull/17559), [17796](https://github.com/ethereum/go-ethereum/pull/17796)).
 * Introduced named database schemas and migrations ([17813](https://github.com/ethereum/go-ethereum/pull/17813)).
 * Add stream peer servers limit ([17747](https://github.com/ethereum/go-ethereum/pull/17747)).
 * Fixed DoS attack with invalid OfferedHashes message length ([17734](https://github.com/ethereum/go-ethereum/pull/17734)).
 * Fixed panic on ARMl6 arch (64bit struct alignment) ([17766](https://github.com/ethereum/go-ethereum/pull/17766)).

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.16) (2018-09-24)

Geth v1.8.16 is out regular maintenance release focusing on bugfixes:

 * Fix RPC timeouts on WebSockets when using the `ethclient` package ([#17549](https://github.com/ethereum/go-ethereum/pull/17549)).
 * Fix the ABI decoder for negative integers which previously overflown ([#17583](https://github.com/ethereum/go-ethereum/pull/17583)).
 * Fix the Clique consensus regression that caused the Rinkeby split ([#17620](https://github.com/ethereum/go-ethereum/pull/17620)).
 * Fix incorrect file permissions for the proof-of-concept Clef signer ([#17652](https://github.com/ethereum/go-ethereum/pull/17652)).
 * Fix a light client deadlock causing the Rinkeby faucet hangs ([#17705](https://github.com/ethereum/go-ethereum/pull/17705), [#17727](https://github.com/ethereum/go-ethereum/pull/17727)).
 * Fix the evm state tester to support more prestate fields ([#17685](https://github.com/ethereum/go-ethereum/pull/17685)).
 * Fix the `ethash` mining hashrate reporting regression ([#17667](https://github.com/ethereum/go-ethereum/pull/17667)).
 * Fix nil-pointer panic when processing malicious ABIs ([#17653](https://github.com/ethereum/go-ethereum/pull/17653)).
 * Fix a rare panic in the light client on shutdown ([#17639](https://github.com/ethereum/go-ethereum/pull/17639)).

Also on adding a few minor features:

 * Miners will prefer their own blocks if all else is equal during fork choice ([17656](https://github.com/ethereum/go-ethereum/pull/17656), [17715](https://github.com/ethereum/go-ethereum/pull/17715)).
 * Reduce `les` overhead via internal caching on the Rinkeby faucet ([#17732](https://github.com/ethereum/go-ethereum/pull/17732)).
 * Double check account creation password after writing to disk ([#17348](https://github.com/ethereum/go-ethereum/pull/17348)).
 * Support basic authentication for WebSockets through URLs ([#17699](https://github.com/ethereum/go-ethereum/pull/17699)).
 * Polish confusing info logs for insta-mining dev chains ([#17614](https://github.com/ethereum/go-ethereum/pull/17614)).
 * Add support for Clique based test chain generation ([#17622](https://github.com/ethereum/go-ethereum/pull/17622)).
 * Add Constantinople ice-age delay implementation ([#17675](https://github.com/ethereum/go-ethereum/pull/17675)).
 * Add Constantinople net gas store implementation ([#17383](https://github.com/ethereum/go-ethereum/pull/17383)).
 * Add initial switches for EVM/EWASM extensions ([#17687](https://github.com/ethereum/go-ethereum/pull/17687)).
 * Add block `age` fields to chain import logs ([#17718](https://github.com/ethereum/go-ethereum/pull/17718)).
 * Bump the build servers to Go 1.11 ([#17701](https://github.com/ethereum/go-ethereum/pull/17701)).

For a full rundown of the changes please consult the [Geth 1.8.16](https://github.com/ethereum/go-ethereum/milestone/77?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.4, which among many bugfixes features:

 * Fixed ENS resolver bug ([17483](https://github.com/ethereum/go-ethereum/pull/17483)).
 * Fixed `bzz-immutable` URLS ([17602](https://github.com/ethereum/go-ethereum/pull/17602)).
 * Refactored and simplified Kademlia code ([17641](https://github.com/ethereum/go-ethereum/pull/17641)).
 * Refactored chunk storage and synchronization ([17659](https://github.com/ethereum/go-ethereum/pull/17659)).
 * Add passwords as access controlled entities ([17598](https://github.com/ethereum/go-ethereum/pull/17598)).

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.15) (2018-08-29)

Geth v1.8.15 is a small patch release on top of [v1.8.14 (Khazad-dûm)](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.14) to fix a few issues discovered post-release and to implement a few missing features to make mining "complete":

 * Fix a data race that could lead to corruption if multiple PoW solutions are found concurrently within the same mining cycle, but for different work tasks ([#17489](https://github.com/ethereum/go-ethereum/pull/17489)).
 * Fix a data corruption if a PoW solution is found to an older block within a single mining cycle ([#17490](https://github.com/ethereum/go-ethereum/pull/17490)).
 * Fix the timestamp and difficulty for recommitted transaction sets at the same block height ([17547](https://github.com/ethereum/go-ethereum/pull/17547)).
 * Fix CPU mining for embedded in-process go-ethereum instances ([#17492](https://github.com/ethereum/go-ethereum/pull/17492)).
 * Extend the logs to differentiate between reorged-but-included and reorged-and-excluded blocks ([#17494](https://github.com/ethereum/go-ethereum/pull/17494)).
 * Extend `ethash` to accept stale results for past mining tasks and track them as potential uncles ([#17506](https://github.com/ethereum/go-ethereum/pull/17506)).
 * Make uncle tracking stateless to permit correct inclusion even in the face or large-ish reorgs ([#17540](https://github.com/ethereum/go-ethereum/pull/17540)).
 * Support skipping PoW verification for remote miner submissions via `--miner.noverify` ([#17506](https://github.com/ethereum/go-ethereum/pull/17506)).
 * Support hard capping the gas limit of mined blocks via `--miner.gaslimit` ([#17546](https://github.com/ethereum/go-ethereum/pull/17546)).

**Note, `--miner.gastarget` and `--miner.gaslimit` defaults to 8M starting from this release. If you require different values, make sure you set them explicitly. Puppeth was also extended to support both configs.**

For a full rundown of the changes please consult the [Geth 1.8.15](https://github.com/ethereum/go-ethereum/milestone/76?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

A quick recap of the miner features introduced in v1.8.14:

* Track multiple miner work packages concurrently, permitting miners to start working on an empty block and replace it in the background with a full one, reducing mining delay by 200+ ms on mainnet ([#15853](https://github.com/ethereum/go-ethereum/pull/15853), [#17323](https://github.com/ethereum/go-ethereum/pull/17323)).
* Insert uncles instantly into active work packages instead of waiting till the next block ([#17320](https://github.com/ethereum/go-ethereum/pull/17320), [#17469](https://github.com/ethereum/go-ethereum/pull/17469)).
* Recommit active mining work every 3 seconds, maximizing block fullness and miner fees ([#17413](https://github.com/ethereum/go-ethereum/pull/17413)).
* Configurable recommit interval with dynamic adjustment in case of system overload ([#17444](https://github.com/ethereum/go-ethereum/pull/17444)).
* Support miner push notifications for new work packages via HTTP POST requests ([#17347](https://github.com/ethereum/go-ethereum/pull/17347)).
* Support priority mining for local transactions, specifiable via `--txpool.locals` ([#17472](https://github.com/ethereum/go-ethereum/pull/17472)).
* Log block fullness and expected fees for current miner work package ([#17416](https://github.com/ethereum/go-ethereum/pull/17416), [#17426](https://github.com/ethereum/go-ethereum/pull/17426)).
* Remote mining will use memory mapped ethash DAGs instead of caches ([#17405](https://github.com/ethereum/go-ethereum/pull/17405)).
* Clean up miner CLI flags and deprecate old ones ([#17402](https://github.com/ethereum/go-ethereum/pull/17402)).

**Note, there are two minor breaking changes wrt mining: 1) remote mining requires the `--mine` flag from now on and will not automatically start on a `eth_getWork` request; 2) `--mine` will not start a CPU miner, you need to explicitly specify `--miner.threads=N` in addition.**

## [v1.8.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.14) (2018-08-22)

Geth v1.8.14 is our regular maintenance release, the current one focusing on heavy miner improvements and general polishes throughout the code base. Big shout-out to @rjl493456442 for championing this release and @ppratscher for helping test it.

First up, the newly introduced miner features:

* Track multiple miner work packages concurrently, permitting miners to start working on an empty block and replace it in the background with a full one, reducing mining delay by 200+ ms on mainnet ([#15853](https://github.com/ethereum/go-ethereum/pull/15853), [#17323](https://github.com/ethereum/go-ethereum/pull/17323)).
* Insert uncles instantly into active work packages instead of waiting till the next block ([#17320](https://github.com/ethereum/go-ethereum/pull/17320), [#17469](https://github.com/ethereum/go-ethereum/pull/17469)).
* Recommit active mining work every 3 seconds, maximizing block fullness and miner fees ([#17413](https://github.com/ethereum/go-ethereum/pull/17413)).
* Configurable recommit interval with dynamic adjustment in case of system overload ([#17444](https://github.com/ethereum/go-ethereum/pull/17444)).
* Support miner push notifications for new work packages via HTTP POST requests ([#17347](https://github.com/ethereum/go-ethereum/pull/17347)).
* Support priority mining for local transactions, specifiable via `--txpool.locals` ([#17472](https://github.com/ethereum/go-ethereum/pull/17472)).
* Log block fullness and expected fees for current miner work package ([#17416](https://github.com/ethereum/go-ethereum/pull/17416), [#17426](https://github.com/ethereum/go-ethereum/pull/17426)).
* Remote mining will use memory mapped ethash DAGs instead of caches ([#17405](https://github.com/ethereum/go-ethereum/pull/17405)).
* Clean up miner CLI flags and deprecate old ones ([#17402](https://github.com/ethereum/go-ethereum/pull/17402)).

**Note, there are two minor breaking changes wrt mining: 1) remote mining requires the `--mine` flag from now on and will not automatically start on a `eth_getWork` request; 2) `--mine` will not start a CPU miner, you need to explicitly specify `--miner.threads=N` in addition.**

Other smaller features:

* Light client checkpoint support for Rinkeby and Clique in general to avoid long sync times ([#17466](https://github.com/ethereum/go-ethereum/pull/17466)).
* Light client CHT and log bloom indexers are supported in light client mode too ([16534](https://github.com/ethereum/go-ethereum/pull/16534), [#17419](https://github.com/ethereum/go-ethereum/pull/17419), [17465](https://github.com/ethereum/go-ethereum/pull/17465)).
* Support trusted peer management (addition/removal) via RPC API function calls ([#16333](https://github.com/ethereum/go-ethereum/pull/16333)).
* Support specifying the block gas limit for simulated test chains (minor API break) ([#17358](https://github.com/ethereum/go-ethereum/pull/17358)).
* Support specifying pubkey identity files into puppeth connection strings ([#17407](https://github.com/ethereum/go-ethereum/pull/17407)).
* Gracefully shut down puppeth instances when deploying new versions ([#17311](https://github.com/ethereum/go-ethereum/pull/17311)).
* Add missing `bn256` license, release package under BSD-3 ([#17451](https://github.com/ethereum/go-ethereum/pull/17451)).

Noteworthy bugfixes:

* Fix a trie GC error in the transaction tracer that occasionally resulted in panics ([#17357](https://github.com/ethereum/go-ethereum/pull/17357)).
* Fix a transaction chain tracing panic if an invalid parametrization was specified ([#17460](https://github.com/ethereum/go-ethereum/pull/17460)).
* Fix a crash in the mobile framework when setting a transaction recipient to `nil` ([#17310](https://github.com/ethereum/go-ethereum/pull/17310)).
* Fix the `alltools` cross compiled bundles to properly build every binary ([#17288](https://github.com/ethereum/go-ethereum/pull/17288)).
* Fix a puppeth `nil` panic caused by non responsive remote servers ([#17412](https://github.com/ethereum/go-ethereum/pull/17412)).
* Update Constantinople `CREATE2` contract formula to the new spec ([#17393](https://github.com/ethereum/go-ethereum/pull/17393)).
* Break accidental PPA dependency of Geth -> Swarm ([#17425](https://github.com/ethereum/go-ethereum/pull/17425)).
* Fix compilation issues on Go 1.11 ([#17368](https://github.com/ethereum/go-ethereum/pull/17368), [#17467](https://github.com/ethereum/go-ethereum/pull/17467)).

For a full rundown of the changes please consult the [Geth 1.8.14](https://github.com/ethereum/go-ethereum/milestone/75?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.2, which among many bugfixes features:

 * Access control to content in Swarm using password or Elliptic Curve keys ([#17404](https://github.com/ethereum/go-ethereum/pull/17404)).
 * Updated Swarm bootnodes ([#17414](https://github.com/ethereum/go-ethereum/pull/17414)).
 * MRU panic fix ([#17313](https://github.com/ethereum/go-ethereum/pull/17313)).

You can download stable versions of Swarm using the `ethereum-swarm` debian package through the well known Ethereum PPA repository.

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.13) (2018-07-31)

Geth v1.8.13 (Swarming) is our ~biweekly maintenance release, currently focusing on minor code polishes, build reorganization and general bug fixes. A highlight of the release is a version and bundle separation for Swarm. Furthermore, we'll also do combo release notes going forward, explicitly detailing both Geth and Swarm!

 * Support block filtering via hashes, not just range queries ([#16734](https://github.com/ethereum/go-ethereum/pull/16734), [#17284](https://github.com/ethereum/go-ethereum/pull/17284)).
 * Upgrade leveldb to resume writes during heavy compaction too ([#17144](https://github.com/ethereum/go-ethereum/pull/17144)).
 * Support swappable interpreters in preparation for ewasm ([#17093](https://github.com/ethereum/go-ethereum/pull/17093)).
 * Implement progress reports for long running chain exports ([#17066](https://github.com/ethereum/go-ethereum/pull/17066)).
 * Switch the Azure blobstore client API to fix deprecated builds ([#17245](https://github.com/ethereum/go-ethereum/pull/17245)).
 * Configurable timeout values for the HTTP RPC API web server ([#17240](https://github.com/ethereum/go-ethereum/pull/17240)).
 * Polish puppeth's status listing to display long ethstats ban lists ([#17279](https://github.com/ethereum/go-ethereum/pull/17279)).
 * Fix a puppeth node id retrieval issue on <2GB RAM machines ([#17281](https://github.com/ethereum/go-ethereum/pull/17281)).
 * Fix a miner race condition for low block-period (1s) chains ([#17173](https://github.com/ethereum/go-ethereum/pull/17173)).
 * Disable Ubuntu Artful, enable Ubuntu Cosmic PPA builds ([#17251](https://github.com/ethereum/go-ethereum/pull/17251)).
 * Detect and reject invalid flag/argument usage ([#17248](https://github.com/ethereum/go-ethereum/pull/17248)).
 * Write `evm` utility errors to `stderr` ([#17118](https://github.com/ethereum/go-ethereum/pull/17118)).

Preliminary EIPs for the upcoming Constantinople fork:

 * Skinny create to allow counterfactual contract creation ([#17196](https://github.com/ethereum/go-ethereum/pull/17196)).
 * Extcodehash to allow smart contracts to retrieve code hashes ([#17202](https://github.com/ethereum/go-ethereum/pull/17202)).

Other notable changes:

 * Relicense the `secp256k1` Go wrapper from LGPL under BSD-3 ([#17225](https://github.com/ethereum/go-ethereum/pull/17225), [#17239](https://github.com/ethereum/go-ethereum/pull/17239)).

For a full rundown of the changes please consult the [Geth 1.8.13](https://github.com/ethereum/go-ethereum/milestone/74?closed=1) release milestone.

**Geth binaries and mobile libraries are available on the [Geth download page](https://geth.ethereum.org/downloads).**

---

This release contains Swarm v0.3.1, which features:

 * Own Launchpad PPA package `ethereum-swarm` and custom version tracking ([#16988](https://github.com/ethereum/go-ethereum/pull/16988)).
 * Client-side MRU signatures: create and update mutable resources through any node, including Web3 support (Mist, Metamask, etc) ([#17231](https://github.com/ethereum/go-ethereum/pull/17231)).
 * PSS generic notifications ([#17150](https://github.com/ethereum/go-ethereum/pull/17150)).
 * Refactored network simulation framework (https://github.com/ethereum/go-ethereum/pull/17241).
 * Better abstraction for HTTP request hangling ([#17164](https://github.com/ethereum/go-ethereum/pull/17164)).
 * Cross process and machine tracing support ([#17169](https://github.com/ethereum/go-ethereum/pull/17169), [#17170](https://github.com/ethereum/go-ethereum/pull/17170)).

From now on you can now download stable versions of Swarm using the `ethereum-swarm` debian package through the well known Ethereum PPA repository.

**Swarm binaries are available on the [Swarm download page](https://swarm-gateways.net/bzz:/theswarm.eth/downloads/).**

## [v1.8.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.12) (2018-07-05)

This release contains the PoC 3 version of Swarm as well as some minor improvements and
bug fixes. The Swarm update merges one year of development effort back into the main
go-ethereum repository. [Read the announcement blog post](https://blog.ethereum.org/2018/06/21/announcing-swarm-proof-of-concept-release-3/) for more information.

Performance changes:

- The intermediate trie GC buffer now uses less memory ([#16876](https://github.com/ethereum/go-ethereum/pull/16876))
- The EVM big integer pool is reused across transactions ([#17070](https://github.com/ethereum/go-ethereum/pull/17070))
- MSTORE and SLOAD EVM instructions are a bit faster ([#16939](https://github.com/ethereum/go-ethereum/pull/16939))

Networking changes:

- A crash in the LES header fetcher is resolved ([#17034](https://github.com/ethereum/go-ethereum/pull/17034))
- Discovery ping/pong logic uses less goroutines ([#17048](https://github.com/ethereum/go-ethereum/pull/17048))

Miscellaneous changes:

- Log message time stamps are properly aligned again ([#17054](https://github.com/ethereum/go-ethereum/pull/17054))
- Network ID is set correctly when -dev is set ([#16833](https://github.com/ethereum/go-ethereum/pull/16833))
- Opcode analysis tracers have been added ([#16954](https://github.com/ethereum/go-ethereum/pull/16954))
- Geth now supports exporting metrics to InfluxDB ([#16979](https://github.com/ethereum/go-ethereum/pull/16979))

For a full rundown of the changes please consult the [Geth 1.8.12](https://github.com/ethereum/go-ethereum/milestone/73?closed=1) release milestone.

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.11) (2018-06-12)

Geth v1.8.11 (Streamline) is our biweekly maintenance release, focusing mainly on performance. Overall this release is ~28% lighter on disk IO and ~23% faster on block processing:

 * Reduce full-node database rate of growth by 28% ([#16810](https://github.com/ethereum/go-ethereum/pull/16810)).
 * Speed up `ethash` verification cache generation by 24% ([#16857](https://github.com/ethereum/go-ethereum/pull/16857)).
 * Increase the Merkle trie hashing performance by 12-50% ([#16896](https://github.com/ethereum/go-ethereum/pull/16896)). 
 * Increase block processing and propagation speed by 15-25% ([#16882](https://github.com/ethereum/go-ethereum/pull/16882)).
 * Reduce runtime complexity of retrieving pending transactions ([#16958](https://github.com/ethereum/go-ethereum/pull/16958)).
 * Cap cache allowance to Go's GC limits and stabilize memory ([#16800](https://github.com/ethereum/go-ethereum/pull/16800)).
 * Reduce database lookups for reverse header retrieval requests ([#16946](https://github.com/ethereum/go-ethereum/pull/16946)).

Not performance improvement, but nice nonetheless:

 * Extend `abigen` to support binding contracts from `STDIN` ([#16683](https://github.com/ethereum/go-ethereum/pull/16683)).
 * Extend bad block tracking and reporting to full blocks ([#16902](https://github.com/ethereum/go-ethereum/pull/16902)).

Beside the improvements, there are also a few bugs squashed:

 * Fix invalid errors for balance retrievals of non-existing blocks ([#16942](https://github.com/ethereum/go-ethereum/pull/16942)).
 * Enforce RPC timeouts to avoid stalling HTTP connections ([#16880](https://github.com/ethereum/go-ethereum/pull/16880)).
 * Fix a goroutine leak in light clients for cancelled requests ([#16861](https://github.com/ethereum/go-ethereum/pull/16861)).
 * Fix a goroutine leak in light clients for timed out requests ([#16776](https://github.com/ethereum/go-ethereum/pull/16776)).
 * Fix an invalid header request handling in light servers ([#16891](https://github.com/ethereum/go-ethereum/pull/16891)).
 * Fix light server tracking regression in light clients ([#16947](https://github.com/ethereum/go-ethereum/pull/16947)).
 * Fix miner to handle side blocks more gracefully ([#16751](https://github.com/ethereum/go-ethereum/pull/16751)).
 * Fix 4byte tracer to count external CALLs too ([#16879](https://github.com/ethereum/go-ethereum/pull/16879)).
 * Fix shutdown hang on invalid genesis json ([#16794](https://github.com/ethereum/go-ethereum/pull/16794)).

For a full rundown of the changes please consult the [Geth 1.8.11](https://github.com/ethereum/go-ethereum/milestone/72?closed=1) release milestone.

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.10) (2018-05-30)

Geth Sacrosancter (v1.8.10) is a tiny hotfix release to the v1.8.9 release:

 * Fixes a deadlock in the transaction pool during high churn ([#16843](https://github.com/ethereum/go-ethereum/pull/16843)).

---

The original v1.8.9 release contained:

 * Handle transaction churn gracefully, reducing idle CPU load on mainnet by an entire core ([#16720](https://github.com/ethereum/go-ethereum/pull/16720)).
 * Fix an issue where slow peers could cause goroutine leaks and out-of-memory crashes ([#16769](https://github.com/ethereum/go-ethereum/pull/16769)).
 * Warn user when database performance is degraded/paused due to leveldb compaction ([#16766](https://github.com/ethereum/go-ethereum/pull/16766)).
 * Reduce transaction retrieval API median/worstcase call times by an order of magnitude ([#16670](https://github.com/ethereum/go-ethereum/pull/16670)).

Feature wise, we've added a few Go API niceties, mostly building blocks for later releases:

 * Support Go tags for ABI structures during marshalling and unmarshalling ([#16648](https://github.com/ethereum/go-ethereum/pull/16648)).
 * Ensure Ethereum Node Records are compatible with discovery v4 ([#16679](https://github.com/ethereum/go-ethereum/pull/16679)).
 * Support Merkle proof generation from trie iterators too ([#16722](https://github.com/ethereum/go-ethereum/pull/16722)).

For a full rundown of the changes please consult the [Geth 1.8.9](https://github.com/ethereum/go-ethereum/milestone/70?closed=1) and [Geth 1.8.10](https://github.com/ethereum/go-ethereum/milestone/71?closed=1) release milestones.

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.9) (2018-05-28)

Geth Sacrosanct (v1.8.9) is our biweekly maintenance release, focusing on stability improvements:

 * Handle transaction churn gracefully, reducing idle CPU load on mainnet by an entire core ([#16720](https://github.com/ethereum/go-ethereum/pull/16720)).
 * Fix an issue where slow peers could cause goroutine leaks and out-of-memory crashes ([#16769](https://github.com/ethereum/go-ethereum/pull/16769)).
 * Warn user when database performance is degraded/paused due to leveldb compaction ([#16766](https://github.com/ethereum/go-ethereum/pull/16766)).
 * Reduce transaction retrieval API median/worstcase call times by an order of magnitude ([#16670](https://github.com/ethereum/go-ethereum/pull/16670)).

Feature wise, we've added a few Go API niceties, mostly building blocks for later releases:

 * Support Go tags for ABI structures during marshalling and unmarshalling ([#16648](https://github.com/ethereum/go-ethereum/pull/16648)).
 * Ensure Ethereum Node Records are compatible with discovery v4 ([#16679](https://github.com/ethereum/go-ethereum/pull/16679)).
 * Support Merkle proof generation from trie iterators too ([#16722](https://github.com/ethereum/go-ethereum/pull/16722)).

For a full rundown of the changes please consult the [Geth 1.8.9 release milestone](https://github.com/ethereum/go-ethereum/milestone/70?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.8) (2018-05-14)

Geth Coffice (v1.8.8) is a tiny biweekly release, containing countless code cleanup PRs from @kielbarry and:

 * Upgrade Android NDK to 16b for the mobile archives ([#16562](https://github.com/ethereum/go-ethereum/pull/16562)).
 * Fix crash when using an invalid private network genesis ([#16682](https://github.com/ethereum/go-ethereum/pull/16682)).
 * Clean up memory database initialization for cleaner code ([#16716](https://github.com/ethereum/go-ethereum/pull/16716)).
 * Handle Android signing keys flexibly, hopefully fixing Maven ([#16696](https://github.com/ethereum/go-ethereum/pull/16696)).
 * Clean up high level raw database access APIs used internally ([#16666](https://github.com/ethereum/go-ethereum/pull/16666)).
 * Support retrieving receipt status codes from the mobile libraries ([#16598](https://github.com/ethereum/go-ethereum/pull/16598)).

For a full rundown of the changes please consult the [Geth 1.8.8 release milestone](https://github.com/ethereum/go-ethereum/milestone/69?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.7) (2018-05-02)

Geth Titanium (v1.8.7) is a hotfix release to address a leveldb crash affecting archive nodes.

 * Fixes some faulty transaction traces due to a dirty handling bug in the tracer ([#16588](https://github.com/ethereum/go-ethereum/pull/16588)).
 * Fixes `goleveldb` to handle databases exceeding 1 tebibyte (Ti) storage ([#16630](https://github.com/ethereum/go-ethereum/pull/16630)).
 * Fixes a transaction discard bug for underpriced but local transactions ([#16576](https://github.com/ethereum/go-ethereum/pull/16576)).

For a full rundown of the changes please consult the [Geth 1.8.7 release milestone](https://github.com/ethereum/go-ethereum/milestone/68?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.6) (2018-04-23)

Geth Third Derivative (v1.8.6) is a [tiny hotfix](https://github.com/ethereum/go-ethereum/milestone/67?closed=1) release for the original Dirty Derivative (v.1.8.4) release:

 * Fix a downloader deadlock which froze sync on master peer drop ([#16546](https://github.com/ethereum/go-ethereum/pull/16546)).
 * Revert the docker images to not use custom users ([#16552](https://github.com/ethereum/go-ethereum/pull/16552), [#16551](https://github.com/ethereum/go-ethereum/pull/16551), [#16550](https://github.com/ethereum/go-ethereum/pull/16550)).

Please see below the original v1.8.4 changelog:

---

 * Optimizes dirty object handling during block processing, leading to 40% faster blocks ([#15225](https://github.com/ethereum/go-ethereum/pull/15225)).
 * Fixes the PPA builds by working around a Travis -> Launchpad connectivity issue ([#16404](https://github.com/ethereum/go-ethereum/pull/16404)).
 * Makes docker images use non-privileged users ([#16052](https://github.com/ethereum/go-ethereum/pull/16052), [#16478](https://github.com/ethereum/go-ethereum/pull/16478), [#16477](https://github.com/ethereum/go-ethereum/pull/16477)).
 * Stabilizes transaction spam eviction from the transaction pool ([#16494](https://github.com/ethereum/go-ethereum/pull/16494))
 * Speeds up external API calls operating on the pending state ([#16497](https://github.com/ethereum/go-ethereum/pull/16497)).
 * Handles downloader interruptions more gracefully ([#16280](https://github.com/ethereum/go-ethereum/pull/16280), [#16509](https://github.com/ethereum/go-ethereum/pull/16509)).
 * Adds leveldb write delay measurements and warnings ([#16499](https://github.com/ethereum/go-ethereum/pull/16499)).
 * Fixes a rare light client deadlock ([#16360](https://github.com/ethereum/go-ethereum/pull/16360)).

The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**

---

For a full rundown of the changes please consult the [Geth 1.8.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/65?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.5) (2018-04-23)

Geth Second Derivative (v1.8.5) is a [tiny hotfix](https://github.com/ethereum/go-ethereum/milestone/66?closed=1) release for the original Dirty Derivative (v.1.8.4) release. Shoutout to @reductionista for investigating and fixing the issue for us!

 * Fix a downloader deadlock which froze sync on master peer drop ([#16546](https://github.com/ethereum/go-ethereum/pull/16546)).

Please see below the original v1.8.4 changelog:

---

 * Optimizes dirty object handling during block processing, leading to 40% faster blocks ([#15225](https://github.com/ethereum/go-ethereum/pull/15225)).
 * Fixes the PPA builds by working around a Travis -> Launchpad connectivity issue ([#16404](https://github.com/ethereum/go-ethereum/pull/16404)).
 * Makes docker images use non-privileged users ([#16052](https://github.com/ethereum/go-ethereum/pull/16052), [#16478](https://github.com/ethereum/go-ethereum/pull/16478), [#16477](https://github.com/ethereum/go-ethereum/pull/16477)).
 * Stabilizes transaction spam eviction from the transaction pool ([#16494](https://github.com/ethereum/go-ethereum/pull/16494))
 * Speeds up external API calls operating on the pending state ([#16497](https://github.com/ethereum/go-ethereum/pull/16497)).
 * Handles downloader interruptions more gracefully ([#16280](https://github.com/ethereum/go-ethereum/pull/16280), [#16509](https://github.com/ethereum/go-ethereum/pull/16509)).
 * Adds leveldb write delay measurements and warnings ([#16499](https://github.com/ethereum/go-ethereum/pull/16499)).
 * Fixes a rare light client deadlock ([#16360](https://github.com/ethereum/go-ethereum/pull/16360)).

The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**

---

For a full rundown of the changes please consult the [Geth 1.8.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/65?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.4) (2018-04-17)

**Nuked due to a downloader deadlock. Please use [v1.8.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.5) instead.**

---

Geth Dirty Derivative (v1.8.4) is our biweekly maintenance release.

 * Optimizes dirty object handling during block processing, leading to 40% faster blocks ([#15225](https://github.com/ethereum/go-ethereum/pull/15225)).
 * Fixes the PPA builds by working around a Travis -> Launchpad connectivity issue ([#16404](https://github.com/ethereum/go-ethereum/pull/16404)).
 * Makes docker images use non-privileged users ([#16052](https://github.com/ethereum/go-ethereum/pull/16052), [#16478](https://github.com/ethereum/go-ethereum/pull/16478), [#16477](https://github.com/ethereum/go-ethereum/pull/16477)).
 * Stabilizes transaction spam eviction from the transaction pool ([#16494](https://github.com/ethereum/go-ethereum/pull/16494))
 * Speeds up external API calls operating on the pending state ([#16497](https://github.com/ethereum/go-ethereum/pull/16497)).
 * Handles downloader interruptions more gracefully ([#16280](https://github.com/ethereum/go-ethereum/pull/16280), [#16509](https://github.com/ethereum/go-ethereum/pull/16509)).
 * Adds leveldb write delay measurements and warnings ([#16499](https://github.com/ethereum/go-ethereum/pull/16499)).
 * Fixes a rare light client deadlock ([#16360](https://github.com/ethereum/go-ethereum/pull/16360)).

The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**

---

For a full rundown of the changes please consult the [Geth 1.8.4 release milestone](https://github.com/ethereum/go-ethereum/milestone/65?closed=1).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.3) (2018-03-27)

This is a security and maintenance release containing bug fixes and protection against various types of DoS attacks that could lead to slow block processing.

### Improvements:

- `geth --shh` and the mobile frameworks use Whisper v6 ([#16371](https://github.com/ethereum/go-ethereum/pull/16371))
- The `geth export-preimages` and `geth import-preimages` commands allow export and import of known keccak256 preimages ([#16319](https://github.com/ethereum/go-ethereum/pull/16319))
- bn256 EC pairing is much faster on arm64 ([#16301](https://github.com/ethereum/go-ethereum/pull/16301))

### Squashed bugs:

- The block number in sync stats (`eth_syncing`) is updated more frequently ([#16283](https://github.com/ethereum/go-ethereum/pull/16283))
- The 128kB size limit for RPC requests is now also applied to WebSocket connections and HTTP requests using chunked transfer encoding
  ([#16310](https://github.com/ethereum/go-ethereum/pull/16310), [#16343](https://github.com/ethereum/go-ethereum/pull/16343))
- abigen wrappers for contract events with a single parameter are fixed ([#16256](https://github.com/ethereum/go-ethereum/pull/16256))

### Credits

Thanks to Dominic Brütsch and Vasily Vasiliev for reporting bugs through the bounty program.

---

For a full rundown of the changes please consult the [Geth 1.8.3 release milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.8.3).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**


## [v1.8.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.2) (2018-03-05)

Geth Frost (v1.8.2) is our biweekly maintenance release.

### Features:

 * Support gracefully terminating on SIGTERM too, not just SIGINT (https://github.com/ethereum/go-ethereum/pull/16142).
 * Reject contract creation RPC requests if no transaction data is provided (https://github.com/ethereum/go-ethereum/pull/16108).
 * Support unpacking ABI binaries into untyped interfaces for future signer work (https://github.com/ethereum/go-ethereum/pull/15770).
* Support multiple ENS resolvers for different TLDs in Swarm (https://github.com/ethereum/go-ethereum/pull/15748).
 * Flush in-memory tries more flexibly on graceful termination (https://github.com/ethereum/go-ethereum/pull/16143).
 * Start work on Swarm metrics collection and aggregation (https://github.com/ethereum/go-ethereum/pull/15969, https://github.com/ethereum/go-ethereum/pull/16177).
 * Introduce Swarm landing page for browser requests (https://github.com/ethereum/go-ethereum/pull/15926).
 * Move Puppeth faucet state updates to background processing (https://github.com/ethereum/go-ethereum/pull/16228).
 * Support creating Go mutex profiles via the debug RPC endpoint (https://github.com/ethereum/go-ethereum/pull/16230).
 * Track number of downloaded state trie nodes across restarts (https://github.com/ethereum/go-ethereum/pull/16224).
 * Use Cloudflare's bn256 library on amd64 for 80% speed improvement (https://github.com/ethereum/go-ethereum/pull/16203).

### Fixes:

 * Disallow hyphens in Puppeth network names (https://github.com/ethereum/go-ethereum/pull/16157).
 * Handle Twitter redirects in Puppeth faucets / Rinkeby (https://github.com/ethereum/go-ethereum/pull/16165).
 * Return better errors on missing RPC subscription endpoints (https://github.com/ethereum/go-ethereum/pull/16163).
 * Fix missing receipt fields in light client mode (https://github.com/ethereum/go-ethereum/pull/16164).
 * Allow exceeding max peers for trusted peers (https://github.com/ethereum/go-ethereum/pull/16189).
 * Update chain specification for Puppeth Parity deploys (https://github.com/ethereum/go-ethereum/pull/16188).
 * Fix RPC response for missing/pending receipts (https://github.com/ethereum/go-ethereum/pull/16217).
 * Fix RPC vhosts to allow setting from config files (https://github.com/ethereum/go-ethereum/pull/16141).
 * Fix `eth_call` invocations doing nested subcalls (https://github.com/ethereum/go-ethereum/pull/16229).

### Deprecations:

 * Drop support for Whisper v2 (https://github.com/ethereum/go-ethereum/pull/16153).
 * Drop support for building with Go 1.7 (https://github.com/ethereum/go-ethereum/pull/16207).

For a full rundown of the changes please consult the [Geth 1.8.2 release milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.8.2).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.1) (2018-02-19)

Geth Iceberg² (v1.8.1) is a [tiny hotfix](https://github.com/ethereum/go-ethereum/milestone/62?closed=1) release for the original Iceberg¹ (v.1.8.0) release:

 * Fix two light client data races resulting in crashes ([#16103](https://github.com/ethereum/go-ethereum/pull/16103), [16095](https://github.com/ethereum/go-ethereum/pull/16095)).
 * Fix a discovery issue that incorrectly measured peer bond time ([16109](https://github.com/ethereum/go-ethereum/pull/16109)).
 * Fix compilation on Go 1.10 ([16115](https://github.com/ethereum/go-ethereum/pull/16115)).

Please see below the original v1.8.0 changelog:

---

We are delighted to announce the [first release in the 1.8 series](https://blog.ethereum.org/2018/02/14/geth-1-8-iceberg¹), shipping 5 months after v1.7.0 and 10 months after v1.6.0.

### Commands

  - This release includes the devcon3 version of puppeth ([#15390](https://github.com/ethereum/go-ethereum/pull/15390))
  - `geth attach` accepts `--datadir`, `--rinkeby`, `--testnet` as an alternative to supplying the IPC path ([#15597](https://github.com/ethereum/go-ethereum/pull/15597), [#15686](https://github.com/ethereum/go-ethereum/pull/15686), [#15517](https://github.com/ethereum/go-ethereum/pull/15517))
  - The geth console has a new `admin.clearHistory` command ([#15614](https://github.com/ethereum/go-ethereum/pull/15614))
  - `ethkey`, a tool for handling keystore files has been added ([#15438](https://github.com/ethereum/go-ethereum/pull/15438), [#15807](https://github.com/ethereum/go-ethereum/pull/15807))

### ETH Protocol

Note: State pruning is enabled on all `--syncmode` variations (including `--syncmode=full`). If you are running an archive node where you would like to retain all historical data, disable pruning via `--gcmode=archive`.

  - Geth now writes less state to disk, slowing down datadir growth ([#15857](https://github.com/ethereum/go-ethereum/pull/15857))
  - Crashes around ethash cache and dataset handling are fixed ([#15864](https://github.com/ethereum/go-ethereum/pull/15864))
  - Pending state creation is faster if there are many transactions in the pool ([#15883](https://github.com/ethereum/go-ethereum/pull/15883))

### RPC API

The HTTP/WS RPC endpoint was extended with DNS rebind protection. If you are running an RPC endpoint addressed by name rather than IP, run with `--rpcvhosts=your.domain` to continue accepting remote requests.

Tracing and pruning: By default, state for the last 128 blocks kept in memory. Most states are garbage collected. If you are running a block explorer or other service relying on transaction tracing without an archive node (`--gcmode=archive`), you need to trace within this window! Alternatively, specify the `"reexec"` tracer option to allow regenerating historical state; and ideally switch to chain tracing which amortizes overhead across all traced blocks.

  - VM tracing is much improved and supports tracing entire block ranges. We have also added a suite of predefined tracer scripts for common tasks such as extracting all internal transactions. ([#15516](https://github.com/ethereum/go-ethereum/pull/15516))
  - In transaction sending RPC calls, the "input" field can be used instead of "data" ([#15640](https://github.com/ethereum/go-ethereum/pull/15640))
  - The gas price oracle is improved to handle tx price surges better ([#15828](https://github.com/ethereum/go-ethereum/pull/15828))
  - The RPC server now protects against DNS rebinding attacks ([#15962](https://github.com/ethereum/go-ethereum/pull/15962))
  - The new `personal_signTransaction` method signs a transaction without sending it to the network ([#15971](https://github.com/ethereum/go-ethereum/pull/15971))
  - HTTP OPTIONS requests no longer require the Content-Type header ([#15759](https://github.com/ethereum/go-ethereum/pull/15759))
  - `admin_nodeInfo` returns the chain configuration in light client mode ([#15732](https://github.com/ethereum/go-ethereum/pull/15732))
  - `debug_storageRangeAt` returns RLP-decoded values ([#15476](https://github.com/ethereum/go-ethereum/pull/15476))

### Light Client

Note: the experimental peer discovery v5 protocol has changed, breaking backwards compatibility with previous releases. Geth 1.8 will not discover LES servers running geth 1.7.

The peer discovery protocols now share the same UDP port (30303 by default). If you are doing manual peer management and using the light client, you may need to ensure your v1.8 clients are pointed to port 30303 and not 30304 as previously.

  - Several bugs around CHT handling are resolved ([#15692](https://github.com/ethereum/go-ethereum/pull/15692), [#16039](https://github.com/ethereum/go-ethereum/pull/16039))
  - Light client peer count is now properly limited ([#16010](https://github.com/ethereum/go-ethereum/pull/16010))

### Swarm

Most swarm development happens over at [the ethersphere fork](https://github.com/ethersphere/go-ethereum) and only a few changes were merged in this cycle. We're in the process of moving swarm development back to the main repository.

  - The swarm command supports TOML config files like geth ([#15548](https://github.com/ethereum/go-ethereum/pull/15548))
  - The API now supports the `bzz-list`, `bzz-raw` and `bzz-immutable` URL schemes ([#15667](https://github.com/ethereum/go-ethereum/pull/15667))

### Whisper

  - Whisper protocol v6 as described in [EIP-627](https://github.com/ethereum/EIPs/pull/627) is implemented ([#15666](https://github.com/ethereum/go-ethereum/pull/15666), [#15701](https://github.com/ethereum/go-ethereum/pull/15701), [#15802](https://github.com/ethereum/go-ethereum/pull/15802), [#15578](https://github.com/ethereum/go-ethereum/pull/15578), [#15631](https://github.com/ethereum/go-ethereum/pull/15631), [#16046](https://github.com/ethereum/go-ethereum/pull/16046))
  - The `wnode` command speaks Whisper v6 ([#16051](https://github.com/ethereum/go-ethereum/pull/16051))

### Networking

  - The peer discovery protocol removes dead nodes more aggressively ([#16069](https://github.com/ethereum/go-ethereum/pull/16069))
  - Discovery v4 and the experimental discovery v5 protocol now run on the same UDP port ([#15200](https://github.com/ethereum/go-ethereum/pull/15200))
  - Several issues in the experimental discovery v5 protocol are resolved ([#15995](https://github.com/ethereum/go-ethereum/pull/15995), [#16036](https://github.com/ethereum/go-ethereum/pull/16036))
  - We have added more bootstrap nodes for the ropsten testnet, including a few parity nodes ([#16008](https://github.com/ethereum/go-ethereum/pull/16008), [#16029](https://github.com/ethereum/go-ethereum/pull/16029))

### Build

  - The `gosimple` and `goconst` linters are enabled for PR builds ([#15593](https://github.com/ethereum/go-ethereum/pull/15593), [#15566](https://github.com/ethereum/go-ethereum/pull/15566))
  - The Ubuntu Zesty PPA is removed, there is a new PPA for Ubuntu Bionic ([#16063](https://github.com/ethereum/go-ethereum/pull/16063))

### Changes for Go and Mobile App Developers

New features in the contract binding generator rely on modifications to internal go-ethereum types within generated code. If you are using wrappers generated prior to v1.8.0, you will need to regenerate them to be compatible with the new code base.

  - The `GasLimit` and `GasUsed` fields of blocks and transactions have type `uint64` instead of `*big.Int`. **This is a breaking API change.** ([#15466](https://github.com/ethereum/go-ethereum/pull/15466))
  - The abigen tool now supports generating contract event filtering methods ([#15832](https://github.com/ethereum/go-ethereum/pull/15832))
  - Several issues in the ABI decoder are resolved ([#15285](https://github.com/ethereum/go-ethereum/pull/15285), [#15731](https://github.com/ethereum/go-ethereum/pull/15731))
  - In the Android/iOS frameworks, the `BigInt` type has a new `GetSign` method ([#15558](https://github.com/ethereum/go-ethereum/pull/15558))
  - The crypto package now supports public key compression and signature verification ([#15615](https://github.com/ethereum/go-ethereum/pull/15615), [#15626](https://github.com/ethereum/go-ethereum/pull/15626))
  - The new common/fdlimit package can be used to raise file descriptor limits ([#15850](https://github.com/ethereum/go-ethereum/pull/15850))
  - The new p2p/enr package implements [EIP-778](https://github.com/ethereum/EIPs/pull/778) ([#15585](https://github.com/ethereum/go-ethereum/pull/15585))

We have a new [code review policy](https://github.com/ethereum/go-ethereum/wiki/Code-Review-Guidelines) which should speed up the contribution process.

For a full rundown of the changes please consult the [Geth 1.8.0 release milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.8.0).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.8.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.0) (2018-02-14)

We are delighted to announce the [first release in the 1.8 series](https://blog.ethereum.org/2018/02/14/geth-1-8-iceberg¹), shipping 5 months after v1.7.0 and 10 months after v1.6.0.

### Commands

  - This release includes the devcon3 version of puppeth ([#15390](https://github.com/ethereum/go-ethereum/pull/15390))
  - `geth attach` accepts `--datadir`, `--rinkeby`, `--testnet` as an alternative to supplying the IPC path ([#15597](https://github.com/ethereum/go-ethereum/pull/15597), [#15686](https://github.com/ethereum/go-ethereum/pull/15686), [#15517](https://github.com/ethereum/go-ethereum/pull/15517))
  - The geth console has a new `admin.clearHistory` command ([#15614](https://github.com/ethereum/go-ethereum/pull/15614))
  - `ethkey`, a tool for handling keystore files has been added ([#15438](https://github.com/ethereum/go-ethereum/pull/15438), [#15807](https://github.com/ethereum/go-ethereum/pull/15807))

### ETH Protocol

Note: State pruning is enabled on all `--syncmode` variations (including `--syncmode=full`). If you are running an archive node where you would like to retain all historical data, disable pruning via `--gcmode=archive`.

  - Geth now writes less state to disk, slowing down datadir growth ([#15857](https://github.com/ethereum/go-ethereum/pull/15857))
  - Crashes around ethash cache and dataset handling are fixed ([#15864](https://github.com/ethereum/go-ethereum/pull/15864))
  - Pending state creation is faster if there are many transactions in the pool ([#15883](https://github.com/ethereum/go-ethereum/pull/15883))

### RPC API

The HTTP/WS RPC endpoint was extended with DNS rebind protection. If you are running an RPC endpoint addressed by name rather than IP, run with `--rpcvhosts=your.domain` to continue accepting remote requests.

Tracing and pruning: By default, state for the last 128 blocks kept in memory. Most states are garbage collected. If you are running a block explorer or other service relying on transaction tracing without an archive node (`--gcmode=archive`), you need to trace within this window! Alternatively, specify the `"reexec"` tracer option to allow regenerating historical state; and ideally switch to chain tracing which amortizes overhead across all traced blocks.

  - VM tracing is much improved and supports tracing entire block ranges. We have also added a suite of predefined tracer scripts for common tasks such as extracting all internal transactions. ([#15516](https://github.com/ethereum/go-ethereum/pull/15516))
  - In transaction sending RPC calls, the "input" field can be used instead of "data" ([#15640](https://github.com/ethereum/go-ethereum/pull/15640))
  - The gas price oracle is improved to handle tx price surges better ([#15828](https://github.com/ethereum/go-ethereum/pull/15828))
  - The RPC server now protects against DNS rebinding attacks ([#15962](https://github.com/ethereum/go-ethereum/pull/15962))
  - The new `personal_signTransaction` method signs a transaction without sending it to the network ([#15971](https://github.com/ethereum/go-ethereum/pull/15971))
  - HTTP OPTIONS requests no longer require the Content-Type header ([#15759](https://github.com/ethereum/go-ethereum/pull/15759))
  - `admin_nodeInfo` returns the chain configuration in light client mode ([#15732](https://github.com/ethereum/go-ethereum/pull/15732))
  - `debug_storageRangeAt` returns RLP-decoded values ([#15476](https://github.com/ethereum/go-ethereum/pull/15476))

### Light Client

Note: the experimental peer discovery v5 protocol has changed, breaking backwards compatibility with previous releases. Geth 1.8 will not discover LES servers running geth 1.7.

The peer discovery protocols now share the same UDP port (30303 by default). If you are doing manual peer management and using the light client, you may need to ensure your v1.8 clients are pointed to port 30303 and not 30304 as previously.

  - Several bugs around CHT handling are resolved ([#15692](https://github.com/ethereum/go-ethereum/pull/15692), [#16039](https://github.com/ethereum/go-ethereum/pull/16039))
  - Light client peer count is now properly limited ([#16010](https://github.com/ethereum/go-ethereum/pull/16010))

### Swarm

Most swarm development happens over at [the ethersphere fork](https://github.com/ethersphere/go-ethereum) and only a few changes were merged in this cycle. We're in the process of moving swarm development back to the main repository.

  - The swarm command supports TOML config files like geth ([#15548](https://github.com/ethereum/go-ethereum/pull/15548))
  - The API now supports the `bzz-list`, `bzz-raw` and `bzz-immutable` URL schemes ([#15667](https://github.com/ethereum/go-ethereum/pull/15667))

### Whisper

  - Whisper protocol v6 as described in [EIP-627](https://github.com/ethereum/EIPs/pull/627) is implemented ([#15666](https://github.com/ethereum/go-ethereum/pull/15666), [#15701](https://github.com/ethereum/go-ethereum/pull/15701), [#15802](https://github.com/ethereum/go-ethereum/pull/15802), [#15578](https://github.com/ethereum/go-ethereum/pull/15578), [#15631](https://github.com/ethereum/go-ethereum/pull/15631), [#16046](https://github.com/ethereum/go-ethereum/pull/16046))
  - The `wnode` command speaks Whisper v6 ([#16051](https://github.com/ethereum/go-ethereum/pull/16051))

### Networking

  - The peer discovery protocol removes dead nodes more aggressively ([#16069](https://github.com/ethereum/go-ethereum/pull/16069))
  - Discovery v4 and the experimental discovery v5 protocol now run on the same UDP port ([#15200](https://github.com/ethereum/go-ethereum/pull/15200))
  - Several issues in the experimental discovery v5 protocol are resolved ([#15995](https://github.com/ethereum/go-ethereum/pull/15995), [#16036](https://github.com/ethereum/go-ethereum/pull/16036))
  - We have added more bootstrap nodes for the ropsten testnet, including a few parity nodes ([#16008](https://github.com/ethereum/go-ethereum/pull/16008), [#16029](https://github.com/ethereum/go-ethereum/pull/16029))

### Build

  - The `gosimple` and `goconst` linters are enabled for PR builds ([#15593](https://github.com/ethereum/go-ethereum/pull/15593), [#15566](https://github.com/ethereum/go-ethereum/pull/15566))
  - The Ubuntu Zesty PPA is removed, there is a new PPA for Ubuntu Bionic ([#16063](https://github.com/ethereum/go-ethereum/pull/16063))

### Changes for Go and Mobile App Developers

New features in the contract binding generator rely on modifications to internal go-ethereum types within generated code. If you are using wrappers generated prior to v1.8.0, you will need to regenerate them to be compatible with the new code base.

  - The `GasLimit` and `GasUsed` fields of blocks and transactions have type `uint64` instead of `*big.Int`. **This is a breaking API change.** ([#15466](https://github.com/ethereum/go-ethereum/pull/15466))
  - The abigen tool now supports generating contract event filtering methods ([#15832](https://github.com/ethereum/go-ethereum/pull/15832))
  - Several issues in the ABI decoder are resolved ([#15285](https://github.com/ethereum/go-ethereum/pull/15285), [#15731](https://github.com/ethereum/go-ethereum/pull/15731))
  - In the Android/iOS frameworks, the `BigInt` type has a new `GetSign` method ([#15558](https://github.com/ethereum/go-ethereum/pull/15558))
  - The crypto package now supports public key compression and signature verification ([#15615](https://github.com/ethereum/go-ethereum/pull/15615), [#15626](https://github.com/ethereum/go-ethereum/pull/15626))
  - The new common/fdlimit package can be used to raise file descriptor limits ([#15850](https://github.com/ethereum/go-ethereum/pull/15850))
  - The new p2p/enr package implements [EIP-778](https://github.com/ethereum/EIPs/pull/778) ([#15585](https://github.com/ethereum/go-ethereum/pull/15585))

We have a new [code review policy](https://github.com/ethereum/go-ethereum/wiki/Code-Review-Guidelines) which should speed up the contribution process.

For a full rundown of the changes please consult the [Geth 1.8.0 release milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.8.0).

**Binaries and mobile libraries are available [on our download page](https://geth.ethereum.org/downloads).**

## [v1.7.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.3) (2017-11-21)

Geth v1.7.3 ([Weir](https://en.wikipedia.org/wiki/Weir)) is a maintenance release to address various issues in our previous releases, most importantly along log filtering and replacement transaction propagation. The release also contains a few developer niceties. **Please see compatibility section below!**

**New features:**

 * Roll out v2 of the `les` light client protocol (https://github.com/ethereum/go-ethereum/pull/14970, https://github.com/ethereum/go-ethereum/pull/15367, https://github.com/ethereum/go-ethereum/pull/15391).
 * Insta-mining, zero CPU, clique based PoA developer mode `--dev` (https://github.com/ethereum/go-ethereum/pull/15323).
 * Add API endpoint to list modified accounts between two blocks (https://github.com/ethereum/go-ethereum/pull/15512).
 * Gas estimation returns error instead of maxgas if transaction cannot be executed (https://github.com/ethereum/go-ethereum/pull/15477).
 * Improve EVM jump destination analysis for worst case scenarios (https://github.com/ethereum/go-ethereum/pull/14582).
 * Enforce `application/json` for HTTP RPC requests (https://github.com/ethereum/go-ethereum/pull/15220, https://github.com/ethereum/go-ethereum/pull/15496).
 * Private network faucet support Facebook, Twitter and Google+ authentication (https://github.com/ethereum/go-ethereum/pull/15313).
 * Support encrypted SSH keys in the Puppeth network manager (https://github.com/ethereum/go-ethereum/pull/15443).
 * Introduce docker images containing all Ethereum tools (https://github.com/ethereum/go-ethereum/pull/15467).
 * Start shipping Ubuntu Artful Aardvark launchpad packages (https://github.com/ethereum/go-ethereum/pull/15344).

**Squashed bugs:**

 * Fix log filtering when specifying non-8-multiple starting block number (https://github.com/ethereum/go-ethereum/pull/15489).
 * Fix replacement transaction propagation (https://github.com/ethereum/go-ethereum/pull/15343).
 * Reduce disk overhead on keystore startup (https://github.com/ethereum/go-ethereum/pull/15526, https://github.com/ethereum/go-ethereum/pull/15527, https://github.com/ethereum/go-ethereum/pull/15529).
 * Fix occasional Rinkeby chain split by additional fork selection logic (https://github.com/ethereum/go-ethereum/pull/15470).
 * Fix JavaScript tracing to permit working with `Address` types (https://github.com/ethereum/go-ethereum/pull/15297).
 * Fix missing commit hash in docker image versions (https://github.com/ethereum/go-ethereum/pull/15458, https://github.com/ethereum/go-ethereum/pull/15464).

**Compatibility caveats:**

 * All HTTP RPC requests from now on need to have the `Content-Type: application/json` header set on them. Geth v1.7.3 will **refuse** to service requests with no content type headers set (or headers with different content types). This is a security measure to counter a browser CORS circumvention technique.
 * Geth v1.7.3 ships with `les/2` light client support, which might have harder time finding light servers initially until the server providers upgrade to v1.7.3 too.

For a full rundown of changes, please see the [v1.7.3](https://github.com/ethereum/go-ethereum/milestone/59?closed=1) milestone.

**As always, binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/)**.

## [v1.7.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.2) (2017-10-14)

**Geth 1.7.2 is a hotfix release to address a DOS vulnerability in the EVM starting at Byzantium. Please upgrade before block 4_370_000!**

Pre-built binaries should be available shortly at https://geth.ethereum.org/downloads/.

## [v1.7.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.1) (2017-10-04)

This is a maintenance release that fixes regressions in the 1.7.0 release.

**This release enables the Byzantium hard fork transition at block number 4370000 (~17th October) on the mainnet and block number 1035301 (~9th October) on the Rinkeby test network. Please update well before these dates to ensure a smooth transition.**

Noteworthy changes:

* Log queries handle the null topic correctly. ([#15195](https://github.com/ethereum/go-ethereum/issues/15195))
* Ledger Nano S and Trezor hardware wallets should once again be detected on macOS. ([#15232](https://github.com/ethereum/go-ethereum/issues/15232))
* p2p: messages are compressed using Snappy. See [EIP706](https://github.com/ethereum/EIPs/pull/706) for more information. ([#15106](https://github.com/ethereum/go-ethereum/issues/15106))
* ethclient: The new `TransactionSender` method can be used to derive the sender address
  of a transaction at the time of inclusion. ([#15127](https://github.com/ethereum/go-ethereum/issues/15127)).

For a full rundown of changes, please see the [v1.7.1 milestone](https://github.com/ethereum/go-ethereum/milestone/56?closed=1).

**As always, binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.7.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.0) (2017-09-14)

The Go Ethereum team is proud to announce the next release family of Geth, the first incarnation of which focuses on laying the groundwork for the upcoming Metropolis hard forks (Byzantium and Constantinople) and consists of [125+ code contributions](https://github.com/ethereum/go-ethereum/milestone/47?closed=1) to various parts of the project.

## Byzantium fork

The current incarnation of Geth contains all the Byzantium EIPs implemented and also features the fork block number **1,700,000** for the Ropsten testnet transition. The block numbers for Rinkeby and the main Ethereum network will be finalized when Ropsten is deemed stable.

You can find details about individual protocol updates at the following locations:

* **[EIP 98](https://github.com/ethereum/EIPs/issues/98)**: Removal of intermediate state roots from receipts ([#14750](https://github.com/ethereum/go-ethereum/pull/14750)).
* Expanded by **[EIP 658](https://github.com/ethereum/EIPs/pull/658)**: Embedding transaction return data in receipts ([#15014](https://github.com/ethereum/go-ethereum/pull/15014)).
* **[EIP 100](https://github.com/ethereum/EIPs/issues/100)**: Change difficulty adjustment to target mean block time including uncles ([#14733](https://github.com/ethereum/go-ethereum/pull/14733)).
* **[EIP 198](https://github.com/ethereum/EIPs/issues/198)**, **[EIP 212 (197)](https://github.com/ethereum/EIPs/issues/212)** and **[EIP 213 (196)](https://github.com/ethereum/EIPs/issues/213)**: Precompiled contracts for modular exponentiation; elliptic curve addition, scalar multiplication and pairing ([#14959](https://github.com/ethereum/go-ethereum/pull/14959), [#14993](https://github.com/ethereum/go-ethereum/pull/14993)).
* **[EIP 214 (116)](https://github.com/ethereum/EIPs/pull/214)**: Expanding the EVM with static contract calls ([#14978](https://github.com/ethereum/go-ethereum/pull/14978)).
* **[EIP 211](https://github.com/ethereum/EIPs/pull/211)**: Expanding the EVM with dynamically sized return data ([#14981](https://github.com/ethereum/go-ethereum/pull/14981)).
* **[EIP 206 (140)](https://github.com/ethereum/EIPs/pull/206)**: Expanding the EVM with cheap state revertals ([#14983](https://github.com/ethereum/go-ethereum/pull/14983)).
* **[EIP 649](https://github.com/ethereum/EIPs/pull/669)**: Delaying the difficulty bomb and reducing the block reward ([#15028](https://github.com/ethereum/go-ethereum/pull/15028)).
* **[EIP 684](https://github.com/ethereum/EIPs/issues/684)**: Preventing overwriting contracts (Byzantium prep) ([#15039](https://github.com/ethereum/go-ethereum/pull/15039)).

## Performance optimizations

The 1.7 release series of Geth is aimed to focus primarily on performance improvements (beside the Byzantium hard fork). The first release of the family already packs a heavy punch with two database schema modifications resulting in significant optimizations:

* Transaction and receipt storage was completely reworked, cutting the data storage requirements of a fast synced node in half, from 26.3GB to 14.9GB at the time of the implementation ([#14801](https://github.com/ethereum/go-ethereum/pull/14801)).
* EVM log storage and indexing was completely reworked, cutting the filtering time of the entire chain for contract events by 2-3 orders of magnitude, from minutes to under a second ([#14522](https://github.com/ethereum/go-ethereum/pull/14522), [#14631](https://github.com/ethereum/go-ethereum/pull/14631)).

Some work-in-progress updates that will land in the next releases are:

* Upgrading the base peer-to-peer protocol used by all Ethereum sub-protocols, cutting the bandwidth needed for a fast sync from 33.6GB to 13.5GB ([#15106](https://github.com/ethereum/go-ethereum/pull/15106)). This upgrade will improve the general bandwidth requirement of the network as well as light clients too.
* Introducing a more sophisticated memory caching for state tries, reducing disk IO by a couple orders of magnitude. Exact number are pending a final implementation ([#14952](https://github.com/ethereum/go-ethereum/pull/14952)).

## Trezor wallets

About this time last year we've introduced support for the Ledger hardware wallet. Due to popular demand, we've now expanded on our hardware wallet offering by also rolling out support for the Trezor ([#14885](https://github.com/ethereum/go-ethereum/pull/14885)).

Opposed to the Ledger however, the Trezor is a bit more complicated as it requires a PIN-unlock sent from the communicating machine instead of directly input by the user. As such, when a user plugs in a Trezor, Geth will print:

`New wallet appeared, failed to open url=trezor://0003:0007:00 err="trezor: pin needed"`

The Geth console can be used to unlock the Trezor by invoking `personal.openWallet(url)`, which will request the user to enter the shuffled PIN code and send that over to the Trezor for verification:

```
> personal.openWallet("trezor://0003:0007:00")

Look at the device for number positions

7 | 8 | 9
--+---+--
4 | 5 | 6
--+---+--
1 | 2 | 3

Please enter current PIN:

INFO [08-10|11:58:06] New wallet appeared url=trezor://0003:0007:00 status="Trezor v1.5.0 'Hi' online"
```
For details on how to interact with the Trezor from the JSON-RPC APIs, please consult the [PR description](https://github.com/ethereum/go-ethereum/pull/14885).

## Transaction journal

In the 1.6.x release family of Geth we've introduced a new transaction pool to avoid propagation issues due to minimum gas-price requirements. This new pool accepted all transactions irrelevant of pricing, and always kept the best paying 4K of them, discarding the cheaper ones.

The new pool features a special exemption mechanism for local accounts so that a user's own transactions are always prioritized over remote ones, even if they are under-priced compared to everyone else's. This ensures that cheap transactions don't get flushed out of the network during heavy usage (e.g. ICO) as long as the originating node remains online.

Geth 1.7.0 takes this protective measure a step forward by journaling all locally created transactions to disk, and loading them back up on a node restart. This ensures that even if the originating node goes offline, cheap transactions still have a chance to be included when the node comes back ([#14784](https://github.com/ethereum/go-ethereum/pull/14784)).

The transaction journal can be an enormous help for node operators during software upgrades by not having to worry about local transactions going missing. Furthermore, the journal also acts as a resiliency mechanism against node crashes, ensuring that no transaction data is lost.

## Rinkeby updates

There have been a lot of subtle tunings made to Puppeth and Rinkeby over the course of this release, such as better ethstats logging to detect malicious reporters and IP address blacklisting to deny access for them.

The Rinkeby testnet also proved vital in finding and and fixing a transaction pool event race that caused a lot of headache around lost transactions and/or duplicate nonce assignments. All such known errors have now been fixed ([#15085](https://github.com/ethereum/go-ethereum/pull/15085)).

Lastly we're extremely happy to announce that [Infura became an active player](https://blog.infura.io/infuras-signer-and-bootnode-on-rinkeby-440de6f70961) in the Rinkeby test network by aiding the community both with their own bootnode as well as running an authorized signer node. This should make the Rinkeby network even more robust and resilient.

## Closing remarks

Geth 1.7.0 contains a lot of bug fixes and we consider it our best release until now, however we urge everyone to take care with the upgrade and monitor it closely afterwards as it does contain non-trivial database upgrades.

Furthermore, we'd like to emphasize that **the upgraded database cannot be used by previous versions of Geth**. Our recommendation for production users it to sync from scratch with Geth 1.7.0, and leave the old database backed up until you confirm that the new release works correctly for all your use cases.

For a full rundown of the changes please consult the [Geth 1.7.0 release milestone](https://github.com/ethereum/go-ethereum/pulls?page=1&amp;q=is%3Apr+milestone%3A1.7.0+is%3Aclosed).

**As always, binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.7) (2017-07-12)

Geth v1.6.7 (All Your Transaction Are Belong To Us) is another fine maintenance release:

* The transaction pool now tracks 'local' transactions based on sender address (https://github.com/ethereum/go-ethereum/pull/14737).
   * Transactions sent by local addresses are exempt from gas pricing- and overall queue size limits.
* Docker images are smaller because consensus tests are no longer included (https://github.com/ethereum/go-ethereum/pull/14734, https://github.com/ethereum/go-ethereum/pull/14792).
 * Extend RPC formatting error messages with field details to better describe the failure (https://github.com/ethereum/go-ethereum/pull/14686).
 * Add Whisper v5 CLI flags to Geth and add a Whisper Go client library (https://github.com/ethereum/go-ethereum/pull/14540).

**Important notice to projects running public nodes (MyEtherWallet, Etherscan, Infura, etc): The sender address of any transaction submitted via RPC will be considered a local address, and all transactions from such addresses will be exempt of the gas price and pool size enforcement. To avoid DOS attacks due to the exemption algorithm, please run with `--txpool.nolocals` to disable special exemptions for local accounts.**

For other minor changes in this release, see the [1.6.7 milestone](https://github.com/ethereum/go-ethereum/milestone/54?closed=1).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.6) (2017-06-23)

This is another maintenance release:

* Fast sync should be more reliable with magnetic disks (https://github.com/ethereum/go-ethereum/pull/14460)
* A transaction pool inconsistency regarding tracking of pending transactions is resolved (https://github.com/ethereum/go-ethereum/pull/14673)
* ethclient: TransactionByHash now actually returns pending = false for mined transactions (https://github.com/ethereum/go-ethereum/pull/14663)


For other minor changes in this release, see the [1.6.6 milestone](https://github.com/ethereum/go-ethereum/milestone/52?closed=1).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.5) (2017-06-01)

Geth v1.6.5 is a security release to counter on ongoing attack on mainnet!

 - Use a bitmap instead of a hashmap to store the results of jumpdest analysis, and optimise analysis process ([#14570](https://github.com/ethereum/go-ethereum/pull/14570)).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.4) (2017-06-01)

Geth v1.6.4 is a hotfix release to patch a transaction signing regression introduced by 1.6.2:

 * Properly initialize account nonce mutex in the API (https://github.com/ethereum/go-ethereum/pull/14568).

Previous (v1.6.3) version of Geth included the following hotfixes:

 * Fix a quadratic overhead when adding new transactions to the pool (https://github.com/ethereum/go-ethereum/pull/14561).
 * Reduce egress network traffic sent to ethstats to avoid overloads (https://github.com/ethereum/go-ethereum/pull/14563).
 * Fix our `ethereum/client-go:alpine` image to point to the 1.6 branch (https://github.com/ethereum/go-ethereum/pull/14564).
 * Fix an account handling regression with badly encoded legacy keyfiles (https://github.com/ethereum/go-ethereum/pull/14565).

Original (v1.6.2) version of Geth being hotfixed included:

- Server-side tx nonce assignment works for concurrent RPC calls to sendTransaction (#14516).
- The transaction pool accepts transactions with any gas price (#14442).
- **(potentially breaking change)** Key imports enforce private key length of 32 bytes (#14502). This affects the `personal_importRawKey` RPC method.

Previously hidden transaction pool defaults can now be set:

```
--txpool.pricelimit value    Minimum gas price limit to enforce for acceptance into the pool (default: 1)
--txpool.pricebump value     Price bump percentage to replace an already existing transaction (default: 10)
--txpool.accountslots value  Minimum number of executable transaction slots guaranteed per account (default: 16)
--txpool.globalslots value   Maximum number of executable transaction slots for all accounts (default: 4096)
--txpool.accountqueue value  Maximum number of non-executable transaction slots permitted per account (default: 64)
--txpool.globalqueue value   Maximum number of non-executable transaction slots for all accounts (default: 1024)
--txpool.lifetime value      Maximum amount of time non-executable transaction are queued (default: 3h0m0s)
```


**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.3) (2017-06-01)

**Nuked: Please see https://github.com/ethereum/go-ethereum/releases/tag/v1.6.4.**

Geth v1.6.3 is a hotfix release to address some issues discovered during yesterday's crazy transaction volume:

 * Fix a quadratic overhead when adding new transactions to the pool (https://github.com/ethereum/go-ethereum/pull/14561).
 * Reduce egress network traffic sent to ethstats to avoid overloads (https://github.com/ethereum/go-ethereum/pull/14563).
 * Fix our `ethereum/client-go:alpine` image to point to the 1.6 branch (https://github.com/ethereum/go-ethereum/pull/14564).
 * Fix an account handling regression with badly encoded legacy keyfiles (https://github.com/ethereum/go-ethereum/pull/14565).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.2) (2017-05-31)

**Nuked: Please see https://github.com/ethereum/go-ethereum/releases/tag/v1.6.4.**

This is mostly a maintenance release with new features added around transaction handling:

- Server-side tx nonce assignment works for concurrent RPC calls to sendTransaction (#14516).
- The transaction pool accepts transactions with any gas price (#14442).
- **(potentially breaking change)** Key imports enforce private key length of 32 bytes (#14502). This affects the `personal_importRawKey` RPC method.

Previously hidden transaction pool defaults can now be set:

```
--txpool.pricelimit value    Minimum gas price limit to enforce for acceptance into the pool (default: 1)
--txpool.pricebump value     Price bump percentage to replace an already existing transaction (default: 10)
--txpool.accountslots value  Minimum number of executable transaction slots guaranteed per account (default: 16)
--txpool.globalslots value   Maximum number of executable transaction slots for all accounts (default: 4096)
--txpool.accountqueue value  Maximum number of non-executable transaction slots permitted per account (default: 64)
--txpool.globalqueue value   Maximum number of non-executable transaction slots for all accounts (default: 1024)
--txpool.lifetime value      Maximum amount of time non-executable transaction are queued (default: 3h0m0s)
```

For other minor changes in this release, see the [1.6.2 milestone](https://github.com/ethereum/go-ethereum/milestone/49?closed=1).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.1) (2017-05-04)

Geth 1.6.1 is mostly a bugfix release adding various polishes throughout the codebase.

Highlights of the release are the capability to iterate over a contract's storage entries via the RPC; transitioning [Whisper to protocol v5](https://github.com/ethereum/go-ethereum/wiki/Whisper) along with bundled diagnostics tools; and shorthand connectivity to the Rinkeby test network.

For a detailed rundown, please consult the [1.6.1 milestone](https://github.com/ethereum/go-ethereum/milestone/48?closed=1).

### New features:

 
 * `debug_storageRangeAt` RPC supports iterating over contract storage entries (https://github.com/ethereum/go-ethereum/pull/14350).
 * Switch Whisper to protocol v5 (https://github.com/ethereum/go-ethereum/pull/14387), ship diagnostic tool (https://github.com/ethereum/go-ethereum/pull/14414).
 * Add `--rinkeby` flag to quickly connect to the Rinkeby testnet (https://github.com/ethereum/go-ethereum/pull/14418).
 * Support disabling USB hardware wallet lookups (https://github.com/ethereum/go-ethereum/pull/14357).
 * GitHub faucet bot protection (https://github.com/ethereum/go-ethereum/pull/14339) and funding tiers (https://github.com/ethereum/go-ethereum/pull/14402).
 * Add explicit SSH server key management for `puppeth` (https://github.com/ethereum/go-ethereum/pull/14398).
 * `geth init` and `geth removedb` operate on both full and light chains (https://github.com/ethereum/go-ethereum/pull/14412).


### Bugfixes and annoyances:

 * Forbid leading zeroes in RPC block numbers (https://github.com/ethereum/go-ethereum/pull/13886).
 * Friendly error if the user tries to reinit its chain with an incompatible genesis (https://github.com/ethereum/go-ethereum/pull/14358).
 * Return `[]` instead of `null` when listing accounts but none can be found (https://github.com/ethereum/go-ethereum/pull/14374).
 * When generating bootnode keys, print and exit, don't start up node (https://github.com/ethereum/go-ethereum/pull/14372).
 * Make network IDs uniform uint64 everywhere (https://github.com/ethereum/go-ethereum/pull/14377).
 * Fix data race in during native dapp node restarts (https://github.com/ethereum/go-ethereum/pull/14379).
 * Support setting flags after sub-commands too (https://github.com/ethereum/go-ethereum/pull/14388, https://github.com/ethereum/go-ethereum/pull/14413).
 * Fix `clique` miner ugly error log when starting a new network (https://github.com/ethereum/go-ethereum/pull/14411).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/).**

## [v1.6.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.0) (2017-04-14)

We are delighted to announce the first release in the 1.6 series! Below is a condensed version of everything we've been working on and you can also find a friendly write up [on our blog](https://blog.ethereum.org/2017/04/14/geth-1-6-puppeth-master/)!

## Breaking Changes

 * The `--fast` and `--light` flags are deprecated (**still usable**) in favor of `--syncmode=fast` and `--syncmode=light`.
 * The default sync mode is `--syncmode=fast`, for full sync please use `--syncmode=full`.
 * If `--datadir` is specified in conjunction with `--testnet`, it will no longer create a `testnet` folder within the given data directory, rather will use the one requested directly.

## New Features

* The 'clique' Proof of Authority consensus engine can be used for private networks (#3753).
* The new `puppeth` command can be used to deploy private networks (#13854).
* `geth --config` can be used to configure geth using a TOML file (#13875).
* Swarm manifests can be mounted as a directory through FUSE (#3690, #13872).
* Swarm manifests can be listed using the `swarm ls` command (#3742).
* The `swarm` command accepts file uploads through stdin (#3744).
* The `evm` command can disassemble contracts. This replaces the disasm command (#3729).
* The `evm` command supports creation of contracts written in EVM assembly language (#3686)
* For `geth --dev` chains, Ethereum protocol upgrades (Homestead, etc.) are enabled at block 0 (#3697).
* The new `debug_getBadBlocks` RPC method returns recently seen 'bad blocks'.
* The `geth bug` command can be used to open bug reports (#3684).
* JS tracing can use `contract.caller`, `contract.address`, `contract.value`, `contract.calldata` to inspect the current call context (#14311).
* `rlpdump` can stop decoding after the first value with the `--single` flag (#14320).

## Removed Features

* The `eth_compileSolidity` RPC method is removed (#3740). See ethereum/EIPs#209 for more information.
* cmd/gethrpctest, cmd/ethtest are gone (#3705).
* `geth upgradedb` is gone. It's no longer feasible to reimport all blocks given the length of the mainnet chain. (#3705)

## Internal Improvements

* Building go-ethereum now requires Go 1.7 or later (#3808). This enables use of the standard-library package "context", which is now used instead of "golang.org/x/net/context" (#3809).
* go-ethereum has preliminary support for pluggable consensus engines. (#3817)
* core/vm uses 64bit integers to represent the gas counter and memory size (#3674)
* Fast sync auto-resumes after deep chain reorganisations (#3743)
* Genesis block JSON handling is stricter and safer. Notably, most JSON fields now require the "0x" prefix. (#3794)
* Log messages are enhanced with contextual, machine-readable information (#3696)
* The `newHeads` subscription returns blocks hashes in addition to other header fields (#13868).
* Package trie contains new union and difference iterators (#3637, #14312)
* Package rlp supports the `rlp:"-"` struct tag to skip fields.
* The new swarm/api/client package wraps the Swarm HTTP API (#3742).
* `make devtools` installs commands required for `go generate`
* Package core can be built without cgo (#3680, #3750).
* Ethash verification caches are memory-mapped, improving startup time (#3750).
* Contract calls made through Go bindings can use a custom sender address (#3782).
* The gas price oracle is now less complicated (#13853).

## Squashed Bugs

* Hardware wallet support is improved. Ledger integration should now work on Windows and macOS. (#3681)
* The `logsBloom` field is now set for pending blocks returned through RPC (#3717)
* HTTP CORS check allows arbitrary other headers (#3783)
* The light client answers RPC requests during sync (#3660).
* Light client servers will no longer refuse to talk to other light client servers (#13889).
* `geth --ethstats` plays nicer with some quirks of the ethstats server (#3812, #13856).
* The current head is announced to peers when sync completes (#13883).
* Transactions are accepted as soon as CPU mining starts. This fixes transaction inclusion issues for small private networks (#13882).
* Discovery bootstrap nodes are used as a last-resort dial target (#13874).
* cmd/swarm doesn't crash if `--ethapi` is empty or invalid (#3754).
* The RPC server delivers all pending responses before closing the connection (#3814).

**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/)**.

## [v1.5.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.9) (2017-02-13)

_"Roses are red, Violets are blue, Geth now supports Hardware wallets too!"_
Geth v1.5.9 is a feature release working towards hardware and HD (hierarchical deterministic) wallets. The first step consists of support for the _Ledger Nano S_ and _Ledger Blue (USB only)_ HD hardware wallets (https://github.com/ethereum/go-ethereum/pull/3592), also laying the groundwork for supporting subsequent hardware and software HD wallets.
Other notable changes:
- Genesis JSON additions and polishes for black box testing https://github.com/ethereum/go-ethereum/pull/3635, https://github.com/ethereum/go-ethereum/pull/3656
- Split off Android builds into Linux build jobs https://github.com/ethereum/go-ethereum/pull/3662
- Swarm manifest manipulation commands https://github.com/ethereum/go-ethereum/pull/3645
- Slim down versioned Alpine Docker images https://github.com/ethereum/go-ethereum/pull/3670
Bugfixes:
- Fix data race in transaction pool nonce retrieval https://github.com/ethereum/go-ethereum/pull/3625
- Support swarm in-process restarts https://github.com/ethereum/go-ethereum/pull/3651
- Fix `abigen` contract parsing for `solc` v1.4.8+ https://github.com/ethereum/go-ethereum/pull/3648
- Fix shutdown issue for swarm running in a docker container https://github.com/ethereum/go-ethereum/pull/3649
For a full rundown, please see the [v1.5.9 milestone](https://github.com/ethereum/go-ethereum/milestone/46?closed=1).
**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/)**.
### Ledger ProTips
- Signing EIP155 transactions requires at least v1.0.3 of the Ethereum app on the Ledger.
  - You may sign non-EIP155 transactions using the `go-ethereum` library directly.
- Contract interactions or deploys need to enable `Contract data` from the Ethereum app `Settings`.
- `Browser support` needs to be turned off from the Ethereum app `Settings` (different protocol).
- Windows support requires a USB driver (official Red Hat): https://github.com/daynix/UsbDk/releases
  - May be bundled into our installer later, we're curious how well it holds up in the wild first.
- Linux support might require [granting the user write access](http://support.ledgerwallet.com/knowledge_base/topics/ledger-wallet-is-not-recognized-on-linux) to the Ledger 
- Accounts are automatically derived, searching for non zero balance or non zero nonce.
  - First empty account is always listed (sending it funds will derive the next empty account).
- Mist doesn't know about Ledger, if asked for a password, enter empty and confirm tx on device.
- If in doubt, unplug and replug :P 
### Go API changes
_Just to emphasize, the section below is about the Go API (i.e. `go-ethereum` library) changes. The RPC did not incur any backwards incompatible changes. If you do find something, it's a bug, please report it._
Until now the `go-ethereum` codebase assumed that accounts are equivalent to individual private keys stored on disk inside a keystore. To support hardware and HD wallets however, this invariant breaks, requiring a new API with different constructs and mechanisms. We've tried our best to introduce this new API in such a way that it's trivial to transition to it for anyone already using `go-ethereum` as a library from Go or Java/ObjC/Swift.
The first breaking change is that while previously the `accounts.Manager` (or `AccountManager` on mobile) was a glorified keystore, now it changed its scope to handle `Wallet` objects, which may contain multiple accounts, as well as may be backed by different backends (keystore or hardware). The previous keystore is kept intact, just renamed to `Keystore` within the `keystore` package (arguably a better name and location). If you used the account manager in your own code, a simple swap to the keystore should more or less just work™.
The other breaking change you need to be aware of is that while previously transaction signing in the keystore only required a hash of the transaction, the new code requires the entire transaction object. This is needed to support hardware wallets which require the transaction details for user confirmation UI elements.

## [v1.5.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.8) (2017-02-01)

Geth v1.5.8 is a patch release to address bugs and annoyances. For a full rundown of the changes please see the [1.5.8 milestone](https://github.com/ethereum/go-ethereum/milestone/45?closed=1).
Notable changes:
- Gas estimation now does binary search to find the right amount of gas even in complex refund conditions where the required gas is higher that the final consumption. (#3587)
- ECDSA signing uses a deterministic nonce. This change affects transaction signing. (#3561)
- A new developer tool, `cmd/wnode`, allows testing the Whisper v5 protocol. (#3580)
**Binaries and mobile libraries are available on our [download page](https://geth.ethereum.org/downloads/)**.

## [v1.5.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.7) (2017-01-16)

This is a bug fix release that resolves several regressions related to hex handling in the RPC API.
See the [1.5.7 milestone](https://github.com/ethereum/go-ethereum/milestone/44?closed=1) for more details.
There are a few other changes in this release:
- The geth console now uses web3.js v0.18.1 (#3545)
- `--olympic` is gone (#3553)
- A hang in the light client is resolved (#3568)
As always, you can install/update via your favorite package manager, or download pre-built binaries from our downloads page at https://geth.ethereum.org/downloads/

## [v1.5.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.6) (2017-01-09)

Geth 1.5.6 - Peach (:peach:) is a refactor and clean up (plumping) release.
This release also improves the EVMs call stack (#3378) and improves the overal EVM performance by 40% and is a pre-patch for the next 64bit gas counting PR. It will also allow for unmetered EVM calls for non-consensus critical execution (e.g. `eth_call`), however at this point this has not yet been integrated.
### Feature(s)
- Support for swarm CORS headers #3388
### Bug fix(es)
- Ropsten chain dump file import fix #3515
- Allow zero priced txs #3454
- RPC APIs no longer accept hex values without 0x prefix. #3475
- Fixed hex handling for signing functions #3453
- Enforce chain ancestry and fixed future block imports #3433 
- `eth_compileSolidity` once again produces identical results for each compilation #3522 
- Error reporting of the Swarm HTTP API is improved #3468 #3469 #3470
### Go API Changes
- `vm.Log` has moved to core/types to reduce dependencies of package ethclient. #3518
- `types.SignECDSA` is now called `types.SignTx`. The `SignECDSA` method of `Transaction` has been removed. #3516
- Go functions dealing with signatures now expect a V value of 0 or 1. `crypto.SignEthereum` and related APIs have been removed. You can convert the signature to ethereum format with a V value of 27 or 28 using  `sig[64] += 27`. #3455 
- accounts/abi handles more Solidity types. #3403 #3464 #3533 
**Binaries are available on our [download page](https://geth.ethereum.org/downloads/)**

## [v1.5.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.5) (2016-12-14)

Geth 1.5.5 is a patch release, mostly fixing bugs and annoyances.
The brave soul/sole feature of the release is support for exporting the blockchain to- and importing it from gzipped data streams (#3427) too. This can be useful for private network development purposes to back up and restore snapshots of the chain and for debugging/testing purposes. You can do compressed export/import operation simply via specifying a chain output file name ending in `.gz`.
The built in netstats client was fixed to report a few infos that were not sent to the netstats server in the previous release due to an oversight (#3370, #3373, #3390). This should help sort out the issues seen on the netstats page that certain charts had missing data in them. Further it adds support for historical data queries that are relevant mostly for private netstats servers with only Geth nodes reporting (#3425).
A few data race issues were fixed in the transaction pool (#3412, #3429) that should help with some duplicate nonce allocations during heavy/parallel transaction publishes; and in the miner (#3431, #3430) that were harmless, just found and fixed. The tracking of mined but not yet conformed blocks was reworked to make it nicer and stabler (if by any chance you relied on these logs having a certain format, be advised that they have changed slightly).
The release fixes a bug in the Windows installer that occasionally corrupted the `PATH` environment variable (#3419); and contains a batch of tweaks and fixes for the light client (#3413) and swarm (3421).
As of 13th December, Canonical deprecated Ubuntu Wily and dropped support for building and distributing launchpad PPA packages for that version of their OS. As a result, we had to remove those PPA builds from out build service too (#3439). However, you can still download bundles binaries from our [downloads page](https://geth.ethereum.org/downloads/) which will work on any Linux distribution (based on libc).
Beside the above, a handful of minor patches were also included in theis release. For a full rundown, please see the [1.5.5 milestone](https://github.com/ethereum/go-ethereum/milestone/42?closed=1)  page.
Binaries are available on our [downloads page](https://geth.ethereum.org/downloads/).
_PS: Honorable mention goes to Razvan, Viorel and Vlad of the Ethereum Cluj meetup for helping find 5 of the bugs fixed above and providing the motivation for 3 awesome features planned for the next release ;)_

## [v1.5.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.4) (2016-11-28)

This release fixes minor bugs and adds exciting features.
### New Features
Geth now includes a built-in netstats reporter. Use `--ethstats "<nodename>:<secret>@ethstats.net"` to get listed on ethstats.net. (#3336) **Assignment**: find secret Skype room with ethstats password :wink: 
Network communication can now be restricted to a list of IP subnetworks. This feature is intended for private chains (and meetups!). For example, `geth --netrestrict 192.168.0.0/16` only allows connections in the commonly used LAN range. (#3325) 
RPC filters can now filter logs in the pending block. (#3219)
### Fixed Bugs
- Two bugs in ethclient were squashed. (#3330, #3328)
- go-ethereum should once again build with git 1.7 and later. (#3352)
- For the full list of changes, [see the 1.5.4 milestone](https://github.com/ethereum/go-ethereum/milestone/41?closed=1).
**Binaries are available on our [download page](https://geth.ethereum.org/downloads/)**
### Note About Go API Freeze
We originally planned to freeze the Go API of certain packages by the 1.5.4 release. We will delay the
freeze by a couple of versions because we're still discovering edge cases as we continue to build
libraries and apps on top of it.

## [v1.5.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.3) (2016-11-24)

This release fixes the consensus failure that occurred at block 2686351.
Geth was failing to revert empty account deletions when the transaction causing the
deletions of empty accounts ended with an an out-of-gas exception. An additional issue
was found in Parity, where the Parity client incorrectly failed to revert empty account
deletions in a more limited set of contexts involving out-of-gas calls to precompiled
contracts; the new Geth behaviour matches Parity's, and empty accounts will cease to be a
source of concern in general in about one week once the state clearing process finishes.
**Binaries are available on our [download page](https://geth.ethereum.org/downloads/)**
## Ropsten Testnet
With 1.5.3 `--testnet` now selects the Ropsten network. If you have a blockchain database
for the Morden network, run `geth --testnet removedb` to remove it.

## [v1.5.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.2) (2016-11-18)

This release fixes a regression that caused geth to fail to full sync past the previous (EIP150) hard fork at block number 2463000.
- Fix a regression that caused the uncle on block 2463002 to be mistakenly determined to be invalid.
### 1.5.1 Release notes
This release fixes typo that set the EIP155 hard fork block to zero in --testnet mode.
For other changes in this release, please see [the 1.5.1 milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.5.1+is%3Aclosed).
### 1.5.0 Release notes
Geth 1.5 contains about 8 months of work and includes many new features and fixes. The
most prominent features include:
- Ethereum hard fork No. 4 containing [EIP155](https://github.com/ethereum/EIPs/issues/155) (replay
  protection), [EIP161](https://github.com/ethereum/EIPs/issues/161) (state clearing), and [EIP170](https://github.com/ethereum/EIPs/issues/170) (code size limit).
- Improvements to the RPC API (see below)
- Initial release of the stable Go API, iOS and Android support. APIs are released as a
  preview and will receive more changes in the upcoming weeks. We expect to freeze certain
  Go APIs in the 1.5.4 release.
For a full rundown and a more detailed post about the changes please see the [Whoa ... Geth 1.5](https://blog.ethereum.org/2016/11/17/whoa-geth-1-5/).
This release overhauls the build infrastructure. Release packages are now built on Travis,
AppVeyor and CircleCI. Archives are available from [geth.ethereum.org](https://geth.ethereum.org)
### Database Upgrade
The 1.5.0 release changes the structure of the blockchain database. Geth will upgrade the database
during normal operation, **but you cannot revert to the previous 1.4.x releases**. If you
do want to revert, you'll need to keep a backup of the chaindata directory or resync.
### Changes to the RPC API
- **Breaking Change**: `eth_sign` prepends a known string to the input and hashes the
  message on the server side. See [PR #2940](https://github.com/ethereum/go-ethereum/pull/2940) for more information.
- We have also added `personal_sign` and `personal_recover`.
- Block responses now include the `mixDigest`.
- Transaction responses include `v`, `r` and `s` values.
- In receipt responses, the `root` field is now prefixed with `0x`.
- `personal_importRawKey` makes it possible to import an unencrypted private key via RPC.
- `eth_getRawTransaction` returns the RLP encoding of a transaction.
- `debug_traceTransaction` can filter the EVM through an arbitrary JavaScript map/reduce
  function on the server side. [See documentation](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#javascript-based-tracing) for more details.
- You can subscribe to real time events when using the WebSocket and IPC
  transports. [See Pub/Sub documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) for more details.
### Changes for Go Developers (and people building from git)
- Go dependencies are now vendored using the `vendor/` directory. If you use Go 1.5 or Go 1.6, you
  need to set `GO15VENDOREXPERIMENT=1` in your environment.
- The `develop` branch is deprecated. All development will happen on the `master` branch.
  This makes it easier for you to get the latest changes. We will continue to keep the
  develop branch in sync with master for one more month to ease the transition.
- If you want to stick to stable releases only, please use the `release/1.5` branch.
- Releases will happen more frequently (promise).
### Experimental Features
Note that these features are highly experimental. Expect bugs and breakage while we
stabilise them over the next couple of releases.
- Geth can now run in light client mode with the `--light` flag. Light client mode syncs
  recent block headers and fetches state values on demand. Note that very few light client
  servers are available yet. You too can be a server using the `--lightserv` flag.
- The Swarm daemon (`bzzd`) and associated helper tools are included and somewhat
  functional.
- Whisper v5 PoC code is included in the repository, but not enabled yet.
- You can now use go-ethereum as a library in Android (Java) and iOS (ObjC/Swift)
  projects. `abigen` has gained preliminary support for creating Java bindings to Ethereum
  contracts.
Please report any issues you encounter.
You can find GPG-signed binaries for all supported platforms on https://geth.ethereum.org/downloads.

## [v1.5.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.1) (2016-11-17)

This release fixes typo that set the EIP155 hard fork block to zero in --testnet mode.
For other changes in this release, please see [the 1.5.1 milestone](https://github.com/ethereum/go-ethereum/pulls?q=is%3Apr+milestone%3A1.5.1+is%3Aclosed).
### 1.5.0 Release notes
Geth 1.5 contains about 8 months of work and includes many new features and fixes. The
most prominent features include:
- Ethereum hard fork No. 4 containing [EIP155](https://github.com/ethereum/EIPs/issues/155) (replay
  protection), [EIP161](https://github.com/ethereum/EIPs/issues/161) (state clearing), and [EIP170](https://github.com/ethereum/EIPs/issues/170) (code size limit).
- Improvements to the RPC API (see below)
- Initial release of the stable Go API, iOS and Android support. APIs are released as a
  preview and will receive more changes in the upcoming weeks. We expect to freeze certain
  Go APIs in the 1.5.4 release.
For a full rundown and a more detailed post about the changes please see the [Whoa ... Geth 1.5](https://blog.ethereum.org/2016/11/17/whoa-geth-1-5/).
This release overhauls the build infrastructure. Release packages are now built on Travis,
AppVeyor and CircleCI. Archives are available from [geth.ethereum.org](https://geth.ethereum.org)
### Database Upgrade
The 1.5.0 release changes the structure of the blockchain database. Geth will upgrade the database
during normal operation, **but you cannot revert to the previous 1.4.x releases**. If you
do want to revert, you'll need to keep a backup of the chaindata directory or resync.
### Changes to the RPC API
- **Breaking Change**: `eth_sign` prepends a known string to the input and hashes the
  message on the server side. See [PR #2940](https://github.com/ethereum/go-ethereum/pull/2940) for more information.
- We have also added `personal_sign` and `personal_recover`.
- Block responses now include the `mixDigest`.
- Transaction responses include `v`, `r` and `s` values.
- In receipt responses, the `root` field is now prefixed with `0x`.
- `personal_importRawKey` makes it possible to import an unencrypted private key via RPC.
- `eth_getRawTransaction` returns the RLP encoding of a transaction.
- `debug_traceTransaction` can filter the EVM through an arbitrary JavaScript map/reduce
  function on the server side. [See documentation](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#javascript-based-tracing) for more details.
- You can subscribe to real time events when using the WebSocket and IPC
  transports. [See Pub/Sub documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) for more details.
### Changes for Go Developers (and people building from git)
- Go dependencies are now vendored using the `vendor/` directory. If you use Go 1.5 or Go 1.6, you
  need to set `GO15VENDOREXPERIMENT=1` in your environment.
- The `develop` branch is deprecated. All development will happen on the `master` branch.
  This makes it easier for you to get the latest changes. We will continue to keep the
  develop branch in sync with master for one more month to ease the transition.
- If you want to stick to stable releases only, please use the `release/1.5` branch.
- Releases will happen more frequently (promise).
### Experimental Features
Note that these features are highly experimental. Expect bugs and breakage while we
stabilise them over the next couple of releases.
- Geth can now run in light client mode with the `--light` flag. Light client mode syncs
  recent block headers and fetches state values on demand. Note that very few light client
  servers are available yet. You too can be a server using the `--lightserv` flag.
- The Swarm daemon (`bzzd`) and associated helper tools are included and somewhat
  functional.
- Whisper v5 PoC code is included in the repository, but not enabled yet.
- You can now use go-ethereum as a library in Android (Java) and iOS (ObjC/Swift)
  projects. `abigen` has gained preliminary support for creating Java bindings to Ethereum
  contracts.
Please report any issues you encounter.
You can find GPG-signed binaries for all supported platforms on https://geth.ethereum.org/downloads.

## [v1.5.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.0) (2016-11-15)

Geth 1.5 contains about 8 months of work and includes many new features and fixes. The
most prominent features include:
- Ethereum hard fork No. 4 containing [EIP155](https://github.com/ethereum/EIPs/issues/155) (replay
  protection), [EIP161](https://github.com/ethereum/EIPs/issues/161) (state clearing), and [EIP170](https://github.com/ethereum/EIPs/issues/170) (code size limit).
- Improvements to the RPC API (see below)
- Initial release of the stable Go API, iOS and Android support. APIs are released as a
  preview and will receive more changes in the upcoming weeks. We expect to freeze certain
  Go APIs in the 1.5.4 release.
For a full rundown and a more detailed post about the changes please see the [Whoa ... Geth 1.5](https://blog.ethereum.org/2016/11/17/whoa-geth-1-5/).
This release overhauls the build infrastructure. Release packages are now built on Travis,
AppVeyor and CircleCI. Archives are available from [geth.ethereum.org](https://geth.ethereum.org)
### Database Upgrade
The 1.5.0 release changes the structure of the blockchain database. Geth will upgrade the database
during normal operation, **but you cannot revert to the previous 1.4.x releases**. If you
do want to revert, you'll need to keep a backup of the chaindata directory or resync.
### Changes to the RPC API
- **Breaking Change**: `eth_sign` prepends a known string to the input and hashes the
  message on the server side. See [PR #2940](https://github.com/ethereum/go-ethereum/pull/2940) for more information.
- We have also added `personal_sign` and `personal_recover`.
- Block responses now include the `mixDigest`.
- Transaction responses include `v`, `r` and `s` values.
- In receipt responses, the `root` field is now prefixed with `0x`.
- `personal_importRawKey` makes it possible to import an unencrypted private key via RPC.
- `eth_getRawTransaction` returns the RLP encoding of a transaction.
- `debug_traceTransaction` can filter the EVM through an arbitrary JavaScript map/reduce
  function on the server side. [See documentation](https://github.com/ethereum/go-ethereum/wiki/Management-APIs#javascript-based-tracing) for more details.
- You can subscribe to real time events when using the WebSocket and IPC
  transports. [See Pub/Sub documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) for more details.
### Changes for Go Developers (and people building from git)
- Go dependencies are now vendored using the `vendor/` directory. If you use Go 1.5 or Go 1.6, you
  need to set `GO15VENDOREXPERIMENT=1` in your environment.
- The `develop` branch is deprecated. All development will happen on the `master` branch.
  This makes it easier for you to get the latest changes. We will continue to keep the
  develop branch in sync with master for one more month to ease the transition.
- If you want to stick to stable releases only, please use the `release/1.5` branch.
- Releases will happen more frequently (promise).
### Experimental Features
Note that these features are highly experimental. Expect bugs and breakage while we
stabilise them over the next couple of releases.
- Geth can now run in light client mode with the `--light` flag. Light client mode syncs
  recent block headers and fetches state values on demand. Note that very few light client
  servers are available yet. You too can be a server using the `--lightserv` flag.
- The Swarm daemon (`bzzd`) and associated helper tools are included and somewhat
  functional.
- Whisper v5 PoC code is included in the repository, but not enabled yet.
- You can now use go-ethereum as a library in Android (Java) and iOS (ObjC/Swift)
  projects. `abigen` has gained preliminary support for creating Java bindings to Ethereum
  contracts.
Please report any issues you encounter.
You can find GPG-signed binaries for all supported platforms on https://geth.ethereum.org/downloads.

## [v1.4.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.19) (2016-11-15)

Geth 1.4.19 contains only the Ethereum hard fork No. 4 which includes [EIP#155](https://github.com/ethereum/EIPs/issues/155) (replay
  protection), [EIP#161](https://github.com/ethereum/EIPs/issues/161) (state clearing), and [EIP#170](https://github.com/ethereum/EIPs/issues/170) (code size limit).

## [v1.4.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.18) (2016-10-15)

This is release Note 7 :boom: and includes the [EIP150 1b/1c Hardfork](https://github.com/ethereum/EIPs/issues/150) and a few fixes:
- Import reporter improvement https://github.com/ethereum/go-ethereum/pull/3120
- Trie memory fix https://github.com/ethereum/go-ethereum/pull/3135
- Expvar debug flag https://github.com/ethereum/go-ethereum/pull/3136
- Transaction pool soft limitations https://github.com/ethereum/go-ethereum/pull/3138
**Note for users running geth on the Ethereum Classic network (--oppose-dao-fork):**
The EIP150 transition is programmed to occur at block 2463000 whether or not --oppose-dao-fork is used.
If you use this option please migrate to the geth client maintained by Ethereum Classic developers.
---
``` text
geth-darwin-amd64-1.4.18-ef9265d0.tar.gz
sha256: 6325050cb382f4543863d88e2abd6609e9c2ebfe34a2c2915fb9ef5a5367d04b
```
``` text
geth-linux-amd64-1.4.18-ef9265d0.tar.gz
sha256: efbace0ef748974becd563803b518965f1567de55b51a444d54a619ed3dae612
```
``` text
geth-windows-amd64-1.4.18-ef9265d0.zip
sha256: d26dd020c7c2a3bbae9e9cdfb278a24d2e840be3910c88b87e7a04fb46fa7bf1
```

## [v1.4.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.17) (2016-10-10)

![pool-aid](https://cloud.githubusercontent.com/assets/6915/19241407/d483dcae-8f0e-11e6-9980-607e2a33c5f9.gif)
This release limits the number of transactions per-user and globally so as to limit maximum memory consumption and egress network traffic.
``` text
geth-darwin-amd64-1.4.17-5a6008e0.tar.gz
sha256: c2c815c2d5175ede115f3740f6e37b55e8c81b93496c3d73bdfb922b6a5c5d07
```
``` text
geth-linux-amd64-1.4.17-5a6008e0.tar.gz
sha256: 10a3f1ff357d3d6c566b4557df242a28169d1431f19b6505ac1e06c540bfadfd
```
``` text
geth-windows-amd64-1.4.17-5a6008e0.zip
sha256: da230058e6d11436179576ed30a9512eda82d9715c0c1ff13a2e0ab13fee158c
```

## [v1.4.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.16) (2016-10-06)

This release improves block processing performance to work around recent DoS attacks. It also disables the --jitvm A/B test which previously enabled the experimental JIT VM for 10% of geth users.
The DoS issue that began affecting the network two days ago has been remediated by adding state journalling to Geth. Journalling means that we no longer need to copy a transaction's state when a call is made; this is the root cause of many of the recently identified issues. Journalling, along with other recent improvements in response to other attacks, should also make Geth significantly faster at importing transactions.

## [v1.4.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.15) (2016-10-03)

Geth 1.4.15 is a hotfix release to address two DoS attack vectors abused on the mainnet:
- Wasted CPU resources by abusing the EVMs JUMPDEST cache's key generation.
- Huge memory and CPU consumption by abusing quadratic contract state dirty tracking.
Please update ASAP. Further optimizations will most probably follow as we comb the code for bottlenecks.

## [v1.4.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.14) (2016-09-28)

This is a Geth pre-release to counter the network DoS attacks from yesterday and those detected today. It is a prerelease (to help people get back on track fast) with further updates coming soon. As always, please keep an eye on your nodes while using as it features a new bleeding edge caching mechanism.
Note, initial attack blocks may process slower as cold data is loaded from the disk, but the longer Geth runs the less impact such transactions should have.
_Being a pre-release, we won't land this code just yet in the `master` branch, also inherently meaning we won't be providing stable binaries from the usual channels (brew, ppa, chocolatey), however cross builds are available for testing below. Mist builds containing the pre-release will also be available._
---
:nut_and_bolt: [[xgo](https://github.com/karalabe/xgo) build bot] Published [geth 1.4.14-prerelease (f88bca7)](https://bintray.com/karalabe/ethereum/geth/1.4.14-prerelease-f88bca7/view) - **release/1.4** branch
| Package | Size | SHA1 Checksum |
| --- | :-: | --- |
| [geth-1.4.14-prerelease-f88bca7-android-21.aar.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-android-21.aar.tar.bz2) | 22M | `a033fae9dd7c301d1e413de1fbef10337cb4f73e` |
| [geth-1.4.14-prerelease-f88bca7-darwin-10.6-amd64.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-darwin-10.6-amd64.tar.bz2) | 5.3M | `5cf7d9afe89c74f5d635c2f9d7ff6b41e2ab1b17` |
| [geth-1.4.14-prerelease-f88bca7-ios-7.0-framework.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-ios-7.0-framework.tar.bz2) | 20M | `bf714d97fb533a3dfb751cb2b433a07a6f805b4c` |
| [geth-1.4.14-prerelease-f88bca7-linux-386.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-386.tar.bz2) | 6.3M | `81a97b37c7beb938015f52d00c966de1c1397239` |
| [geth-1.4.14-prerelease-f88bca7-linux-amd64.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-amd64.tar.bz2) | 6.5M | `26022ac82c7974359c90f6108cc3ba1f1803a22d` |
| [geth-1.4.14-prerelease-f88bca7-linux-arm-5.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-arm-5.tar.bz2) | 6.0M | `fb5e18aef2bc309fe2d8102d4c13ec62dc3975a6` |
| [geth-1.4.14-prerelease-f88bca7-linux-arm-6.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-arm-6.tar.bz2) | 6.0M | `08b33e0608debfd865959b17c7e01b43fab05a8f` |
| [geth-1.4.14-prerelease-f88bca7-linux-arm-7.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-arm-7.tar.bz2) | 6.0M | `8a7c801c5eefd66ee8602226cb0e54193dd37ccc` |
| [geth-1.4.14-prerelease-f88bca7-linux-arm64.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-arm64.tar.bz2) | 6.0M | `b6bd9ae9965d5dd5b007f660145f2812b0213bde` |
| [geth-1.4.14-prerelease-f88bca7-linux-mips64.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-mips64.tar.bz2) | 5.7M | `f44d08e7f50d11c25983d699be490161f26cf743` |
| [geth-1.4.14-prerelease-f88bca7-linux-mips64le.tar.bz2](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-linux-mips64le.tar.bz2) | 5.8M | `f62bbfc22c126e99a55b28153d17e746d39f035c` |
| [geth-1.4.14-prerelease-f88bca7-windows-4.0-386.exe.zip](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-windows-4.0-386.exe.zip) | 5.6M | `817435ad820663371cad23473f1cf7c9c5a6d1d7` |
| [geth-1.4.14-prerelease-f88bca7-windows-4.0-amd64.exe.zip](https://bintray.com/artifact/download/karalabe/ethereum/geth-1.4.14-prerelease-f88bca7-windows-4.0-amd64.exe.zip) | 5.7M | `99a7354471ae8974a66184ab5ad34a96330598d2` |
_Disclaimer: All of these binaries have been cross-compiled from Linux. Their primary goal is to provide access to unsupported or experimental platforms. We cannot guarantee that a cross compiler will produce the same performing code as a native build will. For any issues found, please contact [@karalabe](https://github.com/karalabe)._

## [v1.4.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.13) (2016-09-26)

This patch is a hotfix release to mitigate last weeks DoS attacks, and those ongoing currently against Geth nodes in the network.
This patch should still be considered bleeding-edge. We advise exchanges and other users running business-critical applications that intend to run this release to simultaneously run a different client node and trigger a circuit breaker in case there is another DoS vulnerability or consensus failure. Users should continue to be on the alert for new patches and attacks against the network. Once enough nodes run the patch and the situation sufficiently stabilizes we will advise miners to slowly raise the gas limit.
There will be an additional patch created over the next several weeks to implement some more permanent fixes including cache journaling and cache-miss mitigation.

## [v1.4.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.12) (2016-09-19)

Hotfix release for a network DOS attack! Please update ASAP.

## [v1.4.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.11) (2016-08-18)

This release fixes the following issues:
- Preparing for a new build system #2726 #2896
- Fixes concurrent map access in filtering #2711
- Cancel DAO challenge on peer drop #2833 
- Removed eth/61 protocol #2842
- Downloader improvements #2855 #2861 #2867 #2868 #2866 
- Removed ECRECOVER log message #2892
- Added support for MIPS64 #2682
- Fixed a temporal anomaly in the blockchain when inserting blocks #2873
- Minor text fixes..[.](http://knowyourmeme.com/memes/pokemon-go-updates-controversy) 

## [v1.4.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.10) (2016-07-16)

**Looking for the latest Ethereum Wallet/Mist? See [here](https://github.com/ethereum/mist/releases/latest)**
Geth 1.4.10 "Return of the ETH" is the `go-ethereum` team's DAO hard-fork implementation. It enables anyone to choose whether they would like to support the DAO hard-fork or oppose it, and take them to their blockchain of choice.
- To **support** the DAO hard-fork, start Geth with `--support-dao-fork`
- To **oppose** the DAO hard-fork, start Geth with `--oppose-dao-fork`
On startup (on the main network) Geth will print the currently configured choice, which you can freely change with the appropriate flag at any time. If neither of the above flags is specified, the previous configuration is used. **If no fork choice was ever provided, Geth will default to supporting the fork [per the majority vote](http://carbonvote.com/).**
_Please note, fast sync and light clients do not verify state transitions, only perform header validations and PoW checks. As such, a fork unaware client (i.e. non-updated one) will always fast-sync to the longest chain, as it doesn't have the necessary information on what to look for in the headers. Even if you oppose the fork, please update so your client knows what chain to avoid._
References:
- Hard fork spec: https://blog.slock.it/hard-fork-specification-24b889e70703#.cumx78uqu
- Implementation details: https://github.com/ethereum/go-ethereum/pull/2814

## [v1.4.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.9) (2016-06-29)

Geth 1.4.9 is a reversal release to undo the code changes that went into the 1.4.8 "DAO Wars" soft-fork release, as the soft-fork was deemed too vulnerable to DOS attacks, opening up the entire Ethereum network to resource abuse by malicious users.
Most importantly, this release reverts a data race introduced by the rushed soft-fork that can lead to miners crashing if they are running transactions simultaneously with importing blocks from the network. Thank you @9600- for finding this!
Additionally, the previously release's soft-fork code required a non-insignificant performance impact even when the soft-fork is not enacted, making this current release at the same time a minor performance boost compared to 1.4.8.

## [v1.4.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.8) (2016-06-29)

**This release should be considered nuked. The soft-fork turned out to violate a system invariant leading to potential DOS attack vectors, and the rushed code contains a data race that can sometime lead to miner crashes. Please either use 1.4.7 or 1.4.9 instead (they are equivalent).**
---
Version 1.4.8 is a small patch release to give the community a voice to decide whether to **temporarily** freeze TheDAOs v1.0 from releasing funds or not. If the community decides to freeze the funds, only a few whitelisted accounts can retrieve the blocked funds and return them to previous owners.
_Note: If the soft-fork passes, it will block all DAOs from releasing funds, not just the ones the community considers attacked. This is understandably undesired for all legitimately split DAOs. As such – if the community votes to enact the soft-fork – we propose a follow up patch to the soft-fork that will whitelist all DAOs split according to the intent upheld by the enacted soft-fork._
### How to use this release?
Miners supporting the DAO soft-fork can do so by starting Geth with `--dao-soft-fork`. This will cause the block gas limits to be lowered towards Pi million until the deciding block `1800000` (approx. 6 days from now) is reached. If the gas limit of this block is below or equal 4M, the soft-fork goes into effect and (all updating) miners will start blocking DAO transactions that release funds.
Miners not supporting the DAO soft-fork can run Geth normally without any extra arguments needed. They will try to keep the block gas limits at the current 4.7 million. If the gas limit of the decisive block will be above 4M, the soft-fork is denied and (all updating) miners will accept DAO transactions that release funds.
_Note: All updating clients will agree upon the outcome of the vote and will adhere to that decision. If the soft-fork vote passes, miners voting against it will start blocking transactions too; whereas if the soft-fork is denied, miners voting for it will also accept all transactions._
### What if I don't update?
Miners who do not update by definition vote against the soft-fork as they will continue the current logic of keeping the gas limit above the vote threshold. If the soft-fork is accepted by the majority, non-updating miners will still accept blocked transactions. In that case, non-updating miners will either fork off their own Ethereum network, diverging from the majority, or will forfeit any blocks they mined (since it's not accepted by the majority, overruling the minority blocks).
### Should non-miners (nodes, wallets, mist, etc) update?
From the perspective of non-miners, this update has little relevance. Either outcome of the vote is equally valid from a plain node's perspective, so plain nodes will accept the heavier chain miners decide on without having to know anything about the soft-fork mechanism or results.
### Epilogue
This release implements a **soft-fork**. A soft-fork is perfectly compatible with all protocol rules and requires only the consensus of the majority of miners to enact. It is temporary and can be removed/amended at any point in time upon miner consensus. It does not break protocol rules; it does not roll back any executed transactions/blocks; and it does not change any blockchain state outside of the original protocol capabilities.
_Note: This release does not represent a consent to hard-fork the network. It is a means to give people more time to come up with the best solution._

## [v1.4.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.7) (2016-06-15)

Version 1.4.7 is a tiny patch release mainly to address a Windows console colour issue and to fix a few miscellaneous annoyances.
- Fix Windows console colour output https://github.com/ethereum/go-ethereum/pull/2669
- Fix console to not choke on parentheses in strings https://github.com/ethereum/go-ethereum/pull/2670
- Work around some CI and build server issues https://github.com/ethereum/go-ethereum/pull/2671 and https://github.com/ethereum/go-ethereum/pull/2687
- Fix typo in the `geth account` subcommand help https://github.com/ethereum/go-ethereum/pull/2673
- Update the CLI package dependencies https://github.com/ethereum/go-ethereum/pull/2677 and https://github.com/ethereum/go-ethereum/pull/2681
- Fix ABI number encoding issues for native Dapps https://github.com/ethereum/go-ethereum/pull/2653 and https://github.com/ethereum/go-ethereum/pull/2680
- Fix EVM debug traces for same-block contract suicides https://github.com/ethereum/go-ethereum/pull/2686
- Extend `evm` utility tool with contract creation support https://github.com/ethereum/go-ethereum/pull/2693
- Fix whitespace typos during chain import/export https://github.com/ethereum/go-ethereum/pull/2697

## [v1.4.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.6) (2016-06-06)

As Ethereum got more popular, the peer-to-peer networking infrastructure became extremely versatile (we have 3G peers in New Zealand and Satellite peers in the US). Certain assumptions that Geth made about the network when we've launched became untrue in the last months, especially with TheDAO bringing in an unexpected number of new users and environments. In this newly expanded network, the original assumptions caused Geth to be too aggressive with which peers are useful and which are not.
Geth 1.4.6 "EDGE" aims to solve the synchronization issues resulting from this newly found heterogeneity: it introduces a lot more download concurrency to avoid bottlenecks caused by badly connected remote machines, and also introduces adaptive quality-of-service tuning to make peer selection less aggressive for users with more restricted connectivity.
The result is a newly polished sync mechanism that can churn out a 7-8MB/s download speed in the current Ethereum main network (if your connectivity allows it), but also provide a solid and stable stream for all users, verified for as low as [**EDGE**](https://en.wikipedia.org/wiki/Enhanced_Data_Rates_for_GSM_Evolution) connections (440ms latency, 200 kbps upstream, 220kbps downstream).
We'd like to shout out a big thanks to @ellis2323 for the tireless benchmarking and re-benchmarking of our polishes on high end machines and connectivity (reaching 3 hours full sync times and 20 minutes fast sync times) as well as to @JasonCoombs for verifying connectivity and sync stability on extremely high latency satellite links.
A breakdown of the sync changes:
- Concurrent header retrievals to prevent sync bottlenecks #2315 
- Disable transaction processing during initial sync cycle #2574, #2649
- Download state trie concurrently with blockchain during fast sync #2627
- Adaptive quality of service tuning for restricted connectivity #2630 
- Fast-sync critical-section download failure resiliency #2647
- Enterprise grade hand-tuned multi-level heuristic smart caching :trollface: #2585
Polishes and bugfixes:
- Refactor JavaScript console for external library reuse #2535, #2656
- Re-enable bad block reporting to track any strange activity #2614
- Fix a transaction pool data-race for extreme tests #2655
- Fix eth_getTransactionCount on the testnet for non existent accounts #2626
- Fix compiler path and listing for Solidity compilation #2613, #2612
- Fix an extremely rare downloader hang after a successful sync  #2637
- Fix a CI server `go vet` annoyance with mutex copying #2580

## [v1.4.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.5) (2016-05-24)

This is a bug fix release and includes the following changes:
- Fixed `eth_estimateGas` for non-contract accounts #2589
- Fixed `eth_signTransaction` to return the signed transaction #2578 
- Fixed `nil` pointer crash for topics #2585
- Excluded all `personal` namespace from history #2567
- Added new `personal_signAndSendTransaction` method to the RPC #2564

## [v1.4.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.4) (2016-05-17)

Credit for the release name goes to @sjalq. ([See gitter discussion](https://gitter.im/ethereum/go-ethereum?at=573604f964dbdadc7debd4e4))
**Security fixes**
eth protocol vulnerabilities have been discovered by bounty hunters.
We'll give out more details at a later time along with the full report that we have received.
- Fixed: DoS attack vector in the block downloader
- Fixed: theoretical (but impractical) method of increasing the local minimum block difficulty
This release also fixes a couple of bugs:
- Fixed: crash when assigning `--rpcaddr` #2555 (#2563)
- Fixed: panic in the `abigen` tool #2519 (#2559 #2551)
- Fixed: WebSocket HTTP origin (#2539)
- Skip transaction processing during fast sync (#2574)

## [v1.4.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.3) (2016-05-10)

**Note: 1.4.3 fixes a regression introduced at the very last minute that affected miners**
Release 1.4 is a major release in the history of Go Ethereum and brings you about roughly 6 months worth of work and less [crashes](http://www.vox.com/2016/5/8/11606554/nerc-rrs-david-attenborough-boaty-mcboatface-explained). This is the final release of the 1.4 series and marks the end of the release candidates.
This document will describe some of the major features included in this release and list most of the minor features. For a full rundown of features and fixes please refer to the [1.4.0 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.4.0+is%3Aclosed), [1.4.1 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.4.1+is%3Aclosed) and [1.4.2 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.4.2+is%3Aclosed).
#### On chain version update notifications
We've added a [Geth release oracle](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code) contract to the network which holds the current version of `geth` with the release hash. All updated clients will check the contract and fetch the version number and check whether the version number is greater than the version you're running. It will emit a notice telling you there's a new client available. More info on #2497 .
New releases need to be signed off by 2 out of 3 current signers (@obscuren, @fjl and @karalabe) to be accepted by the oracle and users be notified of it. Note, that we do not do any automatic upgrades, only display a small CLI notification to the user every once in a while.
The verified contract is live at [0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code). The full contract ABI is [available here](https://gist.github.com/karalabe/090246a39e01904361c98bf432e1fdac), but you can also use a [reduced ABI](https://gist.github.com/karalabe/4d45a2ba244e9c60c13e9c77d858b6ef) that just contains the list of signers and the current release version.
#### Event Subscriptions
Over the past months we've significantly overhauled our RPC implementation and added several new capabilities and transport layers. At present `geth` supports the following transport layers:
- WebSockets (available with the `--ws` flag)
- HTTP (available with the `--rpc` flag)
- IPC (available with the `--ipc` flag and enabled by default)
Previously when creating new filters you'd need to poll for them and query to see if there were any pending filter updates. With the new asynchronous implementation we've added an new additional subscription model where, instead of polling, you get notified by the client via pushed updates.
Please refer to the [documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) about our new subscription model.
#### Native Go DApp Development
With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.
#### Private networks
With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.
With the coming of Geth 1.4 we've deprecated the `--genesis <json_file>` flag and replaced with a [`geth init <json file>`](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options#init) sub command. This means that you'll no longer be able to mix the **destructive** `--genesis` flag with other flags.
In addition to the above mentioned `init` subcommand we've added a new field to the genesis file: `config` which allows you to specify configuration options for the chain. At present the only available option is `"homesteadBlock":"0x block_number"` and may be extended with more options as we progress. The configuration option is not yet standardised and may not be implemented in other implementations.
#### Basic build support for iOS and Android platform
Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.
#### Debugging options
We've added several new debugging features that will allow you to get transaction traces and full block traces:
- `traceTransaction( hash )`
- `traceBlockByHash( hash )`
- `traceBlockByNumber( number )`
- `traceBlockByFile( file_with_rlp_encoded_block )`
- `traceBlock( rlp_hex_encoded_string )`
The tracer functions can be found in the `debug` namespace. Please note that these functions are not available within web3 and may only be accessed through either HTTP, IPC, WebSocket RPC and the `geth console`.
### Feature mentions
- New EVM enabled for a few lucky ones
- Node refactoring to allow using Geth as a library
- Fully rewritten RPC layer
- Pending transaction inspection
- Many, many fixes [other](https://github.com/ethereum/go-ethereum/milestones/1.4) fixes
- NTP time check
- General optimisations

## [v1.4.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.2) (2016-05-10)

## Gethy has moved on to [1.4.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.3).

## [v1.4.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.1) (2016-05-04)

**Note: all binaries have been cross compiled to the various platforms. If any of the binaries do not work for your platform please [raise an issue](https://github.com/ethereum/go-ethereum/new).**
---
Release 1.4 is a major release in the history of Go Ethereum and brings you about roughly 6 months worth of work. For this release we've planned several RCs (Release Candidates) so we can test the release together with our community and gather feedback for the final release.
If you want to build this release from source, please use the `release/1.4` branch.
``` text
$ git checkout release/1.4
```
This document will describe some of the major features included in this release and list most of the minor features. For a full rundown of features and fixes please refer to the [1.4.0 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.4.0+is%3Aclosed) and [1.4.1 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.4.1+is%3Aclosed).
#### On chain version update notifications
We've added a [Geth release oracle](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code) contract to the network which holds the current version of `geth` with the release hash. All updated clients will check the contract and fetch the version number and check whether the version number is greater than the version you're running. It will emit a notice telling you there's a new client available. More info on #2497 .
New releases need to be signed off by 2 out of 3 current signers (@obscuren, @fjl and @karalabe) to be accepted by the oracle and users be notified of it. Note, that we do not do any automatic upgrades, only display a small CLI notification to the user every once in a while.
The verified contract is live at [0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code). The full contract ABI is [available here](https://gist.github.com/karalabe/090246a39e01904361c98bf432e1fdac), but you can also use a [reduced ABI](https://gist.github.com/karalabe/4d45a2ba244e9c60c13e9c77d858b6ef) that just contains the list of signers and the current release version.
#### Event Subscriptions
Over the past months we've significantly overhauled our RPC implementation and added several new capabilities and transport layers. At present `geth` supports the following transport layers:
- WebSockets (available with the `--ws` flag)
- HTTP (available with the `--rpc` flag)
- IPC (available with the `--ipc` flag and enabled by default)
Previously when creating new filters you'd need to poll for them and query to see if there were any pending filter updates. With the new asynchronous implementation we've added an new additional subscription model where, instead of polling, you get notified by the client via pushed updates.
Please refer to the [documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) about our new subscription model.
#### Native Go DApp Development
With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.
#### Private networks
With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.
With the coming of Geth 1.4 we've deprecated the `--genesis <json_file>` flag and replaced with a [`geth init <json file>`](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options#init) sub command. This means that you'll no longer be able to mix the **destructive** `--genesis` flag with other flags.
In addition to the above mentioned `init` subcommand we've added a new field to the genesis file: `config` which allows you to specify configuration options for the chain. At present the only available option is `"homesteadBlock":"0x block_number"` and may be extended with more options as we progress. The configuration option is not yet standardised and may not be implemented in other implementations.
#### Basic build support for iOS and Android platform
Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.
#### Debugging options
We've added several new debugging features that will allow you to get transaction traces and full block traces:
- `traceTransaction( hash )`
- `traceBlockByHash( hash )`
- `traceBlockByNumber( number )`
- `traceBlockByFile( file_with_rlp_encoded_block )`
- `traceBlock( rlp_hex_encoded_string )`
The tracer functions can be found in the `debug` namespace. Please note that these functions are not available within web3 and may only be accessed through either HTTP, IPC, WebSocket RPC and the `geth console`.
### Feature mentions
- New EVM enabled for a few lucky ones
- Node refactoring to allow using Geth as a library
- Fully rewritten RPC layer
- Pending transaction inspection
- Many, many fixes [other](https://github.com/ethereum/go-ethereum/milestones/1.4) fixes
- NTP time check
- General optimisations

## [v1.4.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.0) (2016-04-20)

**Note: all binaries have been cross compiled to the various platforms. If any of the binaries do not work for your platform please [raise an issue](https://github.com/ethereum/go-ethereum/new).**
---
Release 1.4 is a major release in the history of Go Ethereum and brings you about roughly 6 months worth of work. For this release we've planned several RCs (Release Candidates) so we can test the release together with our community and gather feedback for the final release.
If you want to build this release from source, please use the `release/1.4` branch.
``` text
$ git checkout release/1.4
```
This document will describe some of the major features included in this release and list most of the minor features. For a full rundown of features and fixes please refer to the [1.4 milestone](https://github.com/ethereum/go-ethereum/milestones/1.4.0).
#### Event Subscriptions
Over the past months we've significantly overhauled our RPC implementation and added several new capabilities and transport layers. At present `geth` supports the following transport layers:
- WebSockets (available with the `--ws` flag)
- HTTP (available with the `--rpc` flag)
- IPC (available with the `--ipc` flag and enabled by default)
Previously when creating new filters you'd need to poll for them and query to see if there were any pending filter updates. With the new asynchronous implementation we've added an new additional subscription model where, instead of polling, you get notified by the client via pushed updates.
Please refer to the [documentation](https://github.com/ethereum/go-ethereum/wiki/RPC-PUB-SUB) about our new subscription model.
#### Native Go DApp Development
With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.
#### Private networks
With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.
With the coming of Geth 1.4 we've deprecated the `--genesis <json_file>` flag and replaced with a [`geth init <json file>`](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options#init) sub command. This means that you'll no longer be able to mix the **destructive** `--genesis` flag with other flags.
In addition to the above mentioned `init` subcommand we've added a new field to the genesis file: `config` which allows you to specify configuration options for the chain. At present the only available option is `"homesteadBlock":"0x block_number"` and may be extended with more options as we progress. The configuration option is not yet standardised and may not be implemented in other implementations.
#### Basic build support for iOS and Android platform
Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.
#### Debugging options
We've added several new debugging features that will allow you to get transaction traces and full block traces:
- `traceTransaction( hash )`
- `traceBlockByHash( hash )`
- `traceBlockByNumber( number )`
- `traceBlockByFile( file_with_rlp_encoded_block )`
- `traceBlock( rlp_hex_encoded_string )`
The tracer functions can be found in the `debug` namespace. Please note that these functions are not available within web3 and may only be accessed through either HTTP, IPC, WebSocket RPC and the `geth console`.
### Feature mentions
- New EVM enabled for a few lucky ones
- Node refactoring to allow using Geth as a library
- Fully rewritten RPC layer
- Pending transaction inspection
- Many, many fixes [other](https://github.com/ethereum/go-ethereum/milestones/1.4) fixes
- NTP time check
- General optimisations

## [v1.3.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.6) (2016-04-01)

This release fixes an issue that fails to strip eol in some cases where a password is required. 

## [v1.3.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5) (2016-03-03)

This release contains a crucial network hotfix #2285 
This release contains all changes from the [homestead release (1.3.4)](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.4).

## [v1.3.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.4) (2016-02-29)

**UPDATE: Please use the [`1.3.5` release](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5) which contains a network hotfix**
This is the first minor protocol upgrade that will set in at `1.150.000`'s block (Testnet = `494.000`). This release includes the following changes to the protocol and fixes:
### Protocol changes
- [EIP-7](https://github.com/ethereum/EIPs/issues/23): New EVM opcode `DELEGATECALL`
- [EIP-2](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2.mediawiki): Consensus rules changes:
  1. The gas cost for creating contracts via a transaction is increased from 21000 to 53000;
  2. All transaction signatures whose s-value is greater than `secp256k1n/2` are now considered invalid;
  3. OOG `CREATE` calls when there's insufficient gas for storing the code (as opposed to silently ignoring the code storage);
  4. Difficulty adjust algorithm has changed to `block_diff = parent_diff + parent_diff // 2048 * max(1 - (block_timestamp - parent_timestamp) // 10, -99) + int(2**((block.number // 100000) - 2))`
### Fixes / Other
- Increased artificial gas floor #2266
- Decreased minimum accepted gas price #2273 
- Fixed windows p2p discover bug #2146 
- Fixed transaction sorting #2143
- Implement [EIP-8](https://github.com/ethereum/EIPs/pull/49) p2p protocol upgrade #2091 
- Fixed downloader OOM #2265 
#### Miners
The minimum accepted gas price for both relaying and block inclusion has changed with #2273 to 20 Shannon (from 50 Shannon), this because of the recent changes in the price of Ether. You can change this setting with the `--gasprice` flag when starting up the client and restore the old gas price if desired. 

## [v1.3.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.3) (2016-01-05)

This release contains only bug fixes:
- Fast sync downloader fix #2097
- Transaction pool fixed #2101

## [v1.3.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.2) (2015-11-25)

This release contains bug fixes and improvements.
### Features and improvements
Removed legalese #1993 
ABI fixes #1943 
Polish protocol gathering #1934 
Ongoing core refactor #1917 
Added unofficial signTransaction method #1666 
Peer download selection algorithm #1953 
### Bug fixes
Verify recovery ID #1984 
Downloader delivery #1980 
Downloader ignore list #1960 

## [v1.3.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.1) (2015-11-03)

This release is a preliminary release to our 2.0 release is already quite packed with new features, optimisations and fixes. For a full list of changes and fixes see the [1.3.0 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.3.0+is%3Aclosed).
This release also marks a new line of release names. We've chosen the list of rodents starting with the [Geomyidae (gophers)](https://en.wikipedia.org/wiki/Gopher).
### Features and improvements
- Light KDF #1940
- Optimised log filtering #1899
- GPU mining #1869
- Ethereum fast sync, eth/63 #1889
- Implemented testnet #1888
- Preliminary ICAP functions #1887
### Bug fixes
- Fixed GPO goroutine leak #1932
- Fixed OOM IPC #1946
- Fixed several console issues #1840 
More information about the next release can be found on the [milestone 1.3.2](https://github.com/ethereum/go-ethereum/milestones/1.3.2) page.

## [v1.2.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.2.3) (2015-10-21)

This release fixes a critical, yet unlikely, bug in the EVM which may cause you to go out of sync due to a state root failure.
More information about the issue can be found in [this gist](https://gist.github.com/obscuren/87ca82e1193548330a39).

## [v1.2.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.2.2) (2015-10-02)

This release fixes a potential issue with clients getting stuck during chain re-organisation. This is a potential random DOS, triggered on possibly some clients (no full-scale DOS) #1865 
This release also fixes an issue with clients syncing on eth/62 occasionally dropping older eth/61 peers, though healthy peers will never drop anyone #1866 

## [1.2.1](https://github.com/ethereum/go-ethereum/releases/tag/1.2.1) (2015-10-01)

This release includes mostly bug fixes, optimisations and improvements. For a full list of changes and fixes see the [1.2.0 milestone](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A1.2.0+is%3Aclosed). Here are the most notable changes:
### Improvements & features
- Improved chain re-organisation and adding of missing transactions #1669
- Ethereum Wire Protocol version 62 #1701 ¹
- Added `receiptRoot` to RPC `getBlock` response #1742
### Bug fixes
- Fixed `submitHashrate` RPC call #1671
- Solidity compiler API update fixes compatibility issue with latest solc ##1770 and expose solidity errors #1598
- Expose missing `shh_getMessage` #1718
- Fixed crash on unknown blocks (`develop` only) #1842
- Fixed duplication of `nonce`s during fast transaction generation #1662
### Notes
¹ While we've been running eth/62 for the last month or so untouched and haven't notices issues, a small local environment is very different from the live network. If you experience any sync problems, you can force geth to sync on the previous eth/61 protocol via `--eth=61`.

## [v1.1.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.1.3) (2015-09-10)

This release fixes an important issue related to state handling in block processing.

## [v1.1.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.1.2) (2015-09-02)

This release is mainly for miners; fixing an important mining strategy with block times.

## [v1.1.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.1.1) (2015-09-01)

This update fixes an important issue related to block gas tracking.

## [v1.0.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.0.3) (2015-09-01)

This update fixes an important issue related to block gas tracking.

## [v1.1.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.1.0) (2015-08-25)

![Imgur](http://i.imgur.com/Etb7jVK.jpg)
This release includes several bug fixes, optimisations and improvements:
### Improvements & features
- New VM (JIT enabled) #1490, #1638
- Merged databases to 1 #1604
- REPL `inspect` method added #1667
- REPL ctrl-c, ctrl-d behaviour changed #1658
- `ethtest` tracing option #1640
- REPL NATSPEC/password prompts (user agents) #1635
- Trie improvements #1603, #1594
- Hashrate reporter #1596
### Bug fixes
- `SendTransaction` recipient validation #1610, #1615
- `--olympic` fix #1611
- Tx rate limiter (thru locking) #1663
- `--exec` output fix #1659
- `EstimateGas` reports correct gas consumption #1654
- `attach` reconnectability #1652
- Windows REPL improvements and fixes #1642, #1600
- Default extra data changed #1595
- Web3 update #1641
- Removed `fdtrack` message #1694 
- TCP/UDP timeout fixes #1689

## [v1.0.1.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.0.1.1) (2015-08-05)

![Thawing!](http://i.imgur.com/4Ohhwwo.jpg)
This release includes several bug fixes and improvements and includes the thawing process.
- Start of thawing process #1578
- Difficulty adjust scheme #1588
- Miner transaction sorting #1583
- Default, community chosen genesis block #1569
- Crash in chain manager #1568
- Log number fixes #1545
- Go 1.5 ready (crypto fix) #1536
- Fix for eth_call and eth_estimateGas #1534
- console `resend` method fix #1461

## [v1.0.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.0.0) (2015-07-29)

![Frontier](https://files.gitter.im/ethereum/go-ethereum/private/Yxkm/68747470733a2f2f66696c65732e6769747465722e696d2f657468657265756d2f676f2d657468657265756d2f707269766174652f336d43722f6d617872657364656661756c742e6a70673.jpg)
This is the first major and first live-net version of the go-ethereum client **geth** (1.0.0) and marks a major milestone in the history of Ethereum and the Go Ethereum codebase.
- Protocol changes:
  - 5 ETH reward per block
  - Target block time upped to 15s
  - Temp hard limit of 5000g
- Downloader fixes and improvements
- Mining improvements and state syncing issues resolved
- Canary fixes
- License update
- Genesis loading

## [v0.9.38](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.38) (2015-07-09)

This is the Frontier RC2 release. This release fixes the following issues:
- Windows fixes for account creation #1453
- Syncing DoS protection #1451, #1450
- web3 update to 0.8.1
- Possible (though hardly!) chain DoS fixed #1443
- Miner log events #1439
For a list of changes included in this release please see [Frontier RC2](https://github.com/ethereum/go-ethereum/milestones/Frontier%20RC2).

## [v0.9.36](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.36) (2015-07-07)

![Frozen](https://files.gitter.im/ethereum/go-ethereum/private/3mCr/maxresdefault.jpg)
This is version 0.9.36 and is a major release in the history of Ethereum and the Go Ethereum team. This release marks a feature stop and is a pre-release for Frontier :shipit: and will go down in history as the "Frozen" release.
This release improves and fixes the following:
- Fixes to receipt storage and added RPC getters for the receipts #1399, #1397
- Bad block reporting #1400
- Lazy validation for miners #1389
- Recover tools #1373
- Massively reduces state size, disk throughput and transaction processing #1396
- Account management improvements #1283
- Improved block downloading & implemented Protocol version 61 #1333
- Web3.js 0.8.0 #1430 
For a list of changes included in this release please see [Milestone 0.9.36](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.36) and [Milestone Frontier](https://github.com/ethereum/go-ethereum/issues?q=milestone%3AFrontier).

## [v0.9.34-1](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.34-1) (2015-06-30)

This release fixes a potential crash. For changes see https://github.com/ethereum/go-ethereum/releases/tag/v0.9.34

## [v0.9.34](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.34) (2015-06-30)

This is release 0.9.34 and addresses the following issues and improvements: 
- Core optimisations & immutability of consensus types (#1301)
- Monitoring and metrics system. Please see the [Metrics and Monitoring wiki](https://github.com/ethereum/go-ethereum/wiki/Metrics-and-Monitoring) for more info  (#1321 #1346)
- New keystore encryption scheme (+ backward compatibility) (#1085)
For a list of changes included in this release please see [Milestone 0.9.34](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.34). For the next release please see [Milestone 0.9.36](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.36)

## [v0.9.32](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.32) (2015-06-23)

This is release 0.9.32 and addresses the following issues and improvements: 
- Downloader potential DOS attack #1314
- Removed old version of mist #1302
- Core optimisations #1282
- Integration of the finalised console
  - Attach to a running geth node with `geth attach`
  - Start in console mode with `geth console`
- RPC raw transaction `eth_sendRawTransaction` #1267
For a list of changes included in this release please see [Milestone 0.9.32](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.32). For the next release please see [Milestone 0.9.34](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.34)

## [v0.9.30](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.30) (2015-06-15)

This is release "Consolation" and addresses the following issues and improvements: 
- Optimisations on the VM [1228](https://github.com/ethereum/go-ethereum/pull/1228)
  - Structured logging
  - Program Counter change
- A new RPC layer and added IPC [1200](https://github.com/ethereum/go-ethereum/pull/1200)
- Downloader improvements
- GPO (Gas Price Oracle). The GPO advices on gas price. Please refer to the [GPO article](https://github.com/ethereum/go-ethereum/wiki/Gas-Price-Oracle) for more information.
- P2P improvements [1261](https://github.com/ethereum/go-ethereum/pull/1261)
- Transaction pool improvement [1260](https://github.com/ethereum/go-ethereum/pull/1260). This issue fixes network load.
- Implemented a new console (`make console`) which communicates over IPC.
For a list of changes included in this release please see [Milestone 0.9.30](https://github.com/ethereum/go-ethereum/issues?q=milestone%3A0.9.30)

## [v0.9.28](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.28) (2015-06-09)

This is a stability and security fix and is required for all users.
### Highlights
- Security issue EC Recover and right padding up to 128
- Downloader security improvements
- Chucking of max size blocks and transactions
- Improved parallelisation of nonce checks during import
- Improved transaction handling
- Automated ARM builds
For a full list of changes check [v0.9.26 / v0.9.28](https://github.com/ethereum/go-ethereum/compare/v0.9.26...v0.9.28)

## [v0.9.26](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.26) (2015-05-28)

This is a mandatory update and helps to improve the network's stability through several upgrade mechanism and download policies.
This release will go down in history as the **fork slayer** :-)

## [v0.9.25](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.25) (2015-05-27)

This update fixes two things:
1. [Hardcode](https://github.com/ethereum/go-ethereum/blob/develop/core/blocks.go#L7) a block's header hash so that when it encounters it in your blockchain revert back the **valid** parent and from there on get back on the right track. All clients which catch up from anything previous to the **invalid** block won't have any issue and might not even notice the bad chain. 
2. Fixes an issue within the Ethereum VM's memory when doing a sub-call (op codes `CALL` & `CALLCODE`). When doing a sub-call you reserve a bit of memory for the sub-calls return data and write to that memory region when the sub-call returns. The subtle change is that previously Geth would zero out the memory and then write, now it doesn't zero and write directly. Here's an example of the before and after
Before:
```
Mem before call   = [1,1,1,1,1,1,1,1,1,1]
Sub calls returns = [6,6]
Mem after call    = [6,6,0,0,0,0,0,0,0,0]
```
After:
```
Mem before call   = [1,1,1,1,1,1,1,1,1,1]
Sub calls returns = [6,6]
Mem after call    = [6,6,1,1,1,1,1,1,1,1]
```
This update also includes a re-work of the `p2p` dialing mechanism.

## [v0.9.24](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.24) (2015-05-26)

This update fixes two things:
1. [Hardcode](https://github.com/ethereum/go-ethereum/blob/develop/core/blocks.go#L7) a block's header hash so that when it encounters it in your blockchain revert back the **valid** parent and from there on get back on the right track. All clients which catch up from anything previous to the **invalid** block won't have any issue and might not even notice the bad chain. 
2. Fixes an issue within the Ethereum VM's memory when doing a sub-call (op codes `CALL` & `CALLCODE`). When doing a sub-call you reserve a bit of memory for the sub-calls return data and write to that memory region when the sub-call returns. The subtle change is that previously Geth would zero out the memory and then write, now it doesn't zero and write directly. Here's an example of the before and after
Before:
```
Mem before call   = [1,1,1,1,1,1,1,1,1,1]
Sub calls returns = [6,6]
Mem after call    = [6,6,0,0,0,0,0,0,0,0]
```
After:
```
Mem before call   = [1,1,1,1,1,1,1,1,1,1]
Sub calls returns = [6,6]
Mem after call    = [6,6,1,1,1,1,1,1,1,1]
```
This update also includes a re-work of the `p2p` dialing mechanism.

## [0.9.23](https://github.com/ethereum/go-ethereum/releases/tag/0.9.23) (2015-05-21)

This Olympic release comes with fixes in the following areas:
- BUG: fixes for creation transactions.
- FEATURE: AOT DAG generation
- BUG: uncle fix during GPU mining
- BUG: coinbase fix
- FEATURE: parallel nonce checking during block processing
- BUG: downloader fixes and vuln patches
- FEATURE: multi CORS for RPC
- FEATURE: `removedb` implemented for killing your local chain
- FEATURE: multiple account unlocks from CLI
- FEATURE: solidity compiler accepts multi-contract source (API change)
#### Installation instructions
Build instructions for OSX, Windows and Linux can be found [here](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum). 
#### Upgrade from <0.9.20
If you have accounts in your wallet that you'd like to keep you need export them first first with the `gethkey` tool found [here](https://github.com/ethereum/gethkey). Download and install and run the following for each of the accounts you'd like to keep:
```
mkdir -p ~/export_keys
gethkey <account_address> ~/export_keys/account_address
geth account import ~/export_keys/account_address
```
When you've verified and imported all of your keys `rm -rf ~/export_keys`.
#### Resources
- [Geth console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)
- [Geth CLI](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)
- [Geth account management](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
- [WIP Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/)

## [v0.9.21.1](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.21.1) (2015-05-16)

Olympic release comes with many improvements to the CLI tools and includes a fully programmable JavaScript interface and JavaScript REPL (**R**ead **E**valuate **P**rint **L**oop). We've also added support for solidity using the solidity stand-alone compiler: `solc`.
The CLI has seen a massive overhaul in usability and CLI utilities. For a complete list of utils and tools please run `geth help` or `get help topic` for a detailed description.
We're also in the process of writing a  [Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/). Please note that this book is a work in progress and will serve as the main guide for frontier once released.
#### Installation instructions
Build instructions for OSX, Windows and Linux can be found [here](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum). 
#### Upgrade from <0.9.20
If you have accounts in your wallet that you'd like to keep you need export them first first with the `gethkey` tool found [here](https://github.com/ethereum/gethkey). Download and install and run the following for each of the accounts you'd like to keep:
```
mkdir -p ~/export_keys
gethkey <account_address> ~/export_keys/account_address
geth account import ~/export_keys/account_address
```
When you've verified and imported all of your keys `rm -rf ~/export_keys`.
If you've been encountering `INVALID block #xxxx` run a `geth upgradedb` after you've updated (ignore any errors it encounters). When it's done re-run geth as you normally do.
#### Resources
- [Geth console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)
- [Geth CLI](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)
- [Geth account management](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
- [WIP Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/)

## [v0.9.21](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.21) (2015-05-15)

Olympic release comes with many improvements to the CLI tools and includes a fully programmable JavaScript interface and JavaScript REPL (**R**ead **E**valuate **P**rint **L**oop). We've also added support for solidity using the solidity stand-alone compiler: `solc`.
The CLI has seen a massive overhaul in usability and CLI utilities. For a complete list of utils and tools please run `geth help` or `get help topic` for a detailed description.
We're also in the process of writing a  [Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/). Please note that this book is a work in progress and will serve as the main guide for frontier once released.
#### Installation instructions
Build instructions for OSX, Windows and Linux can be found [here](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum). 
#### Upgrade from <0.9.20
If you have accounts in your wallet that you'd like to keep you need export them first first with the `gethkey` tool found [here](https://github.com/ethereum/gethkey). Download and install and run the following for each of the accounts you'd like to keep:
```
mkdir -p ~/export_keys
gethkey <account_address> ~/export_keys/account_address
geth account import ~/export_keys/account_address
```
When you've verified and imported all of your keys `rm -rf ~/export_keys`.
If you've been encountering `INVALID block #xxxx` run a `geth upgradedb` after you've updated (ignore any errors it encounters). When it's done re-run geth as you normally do.
#### Resources
- [Geth console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)
- [Geth CLI](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)
- [Geth account management](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
- [WIP Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/)

## [v0.9.20](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.20) (2015-05-12)

Olympic release comes with many improvements to the CLI tools and includes a fully programmable JavaScript interface and JavaScript REPL (**R**ead **E**valuate **P**rint **L**oop). We've also added support for solidity using the solidity stand-alone compiler: `solc`.
The CLI has seen a massive overhaul in usability and CLI utilities. For a complete list of utils and tools please run `geth help` or `get help topic` for a detailed description.
We're also in the process of writing a  [Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/). Please note that this book is a work in progress and will serve as the main guide for frontier once released.
#### Installation instructions
Build instructions for OSX, Windows and Linux can be found [here](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum). 
#### Upgrade from <0.9.20
If you have accounts in your wallet that you'd like to keep you need export them first first with the `gethkey` tool found [here](https://github.com/ethereum/gethkey). Download and install and run the following for each of the accounts you'd like to keep:
```
mkdir -p ~/export_keys
gethkey <account_address> ~/export_keys/account_address
geth account import ~/export_keys/account_address
```
When you've verified and imported all of your keys `rm -rf ~/export_keys`.
If you've been encountering `INVALID block #xxxx` run a `geth upgradedb` after you've updated (ignore any errors it encounters). When it's done re-run geth as you normally do.
#### Resources
- [Geth console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)
- [Geth CLI](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options)
- [Geth account management](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
- [WIP Frontier GitBook](http://ethereum.gitbooks.io/frontier-guide/content/)

## [v0.9.18](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.18) (2015-05-09)

### Update on the release can be found [here](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.20)

## [v0.8.5](https://github.com/ethereum/go-ethereum/releases/tag/v0.8.5) (2015-02-22)

![Mist](https://dl.dropboxusercontent.com/u/4270001/pocs/8.png)
## General changes
- Improved UI and UX
- Multithreaded miner
- Finalised RPC interface
- Improved networking & peer discovery
- Moved from WebKit to Chromium (QtWebEngine)
## Other
- Moved away from a asynchronous javascript api to a synchronous (using RPC)
- Qt5.4
- DApp listing

## [v0.8.4](https://github.com/ethereum/go-ethereum/releases/tag/v0.8.4) (2015-02-20)

The next in the proof of concept series, No. 8
Changes in the release include:
- Improved networking (P2P version 2)
- Improved Mist UI
- New Browser
- DApp listing

## [v0.6.8](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.8) (2014-10-07)

This release _fixes_ & _improves_ the following:
- Improved block downloading
- Added black listing when kicking of bad peers
- Removed old windows from GUI

## [v0.6.7](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.7) (2014-09-26)

This release _fixes_ & _improves_ the following:
- Messaging API. Previous version didn't follow the correct rules when filtering messages;
- Added display for connected peer's capabilities.

## [v0.6.6](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.6) (2014-09-24)

This release _fixes_ & _improves_ the following:
- Block chain download speed
- Block organisational stuff
- JefCoin fixes ;-)
- Mine without tx
[Previous release notes](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.5)

## [v0.6.5](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.5) (2014-09-23)

# Mist (Ether Browser)
**note**: Windows binaries will be uploaded **soon**™
![](https://dl.dropboxusercontent.com/u/4270001/Screenshot%202014-09-22%2023.33.52.png)
This is the first release of the **proof of concept** ether browser named **Mist**. Mist is **PoC6** ready and has the following _improvements_ and _features_:
- [HTML ĐApp](https://github.com/ethereum/go-ethereum/wiki/PoC6-JS-API) development.
**JavaScript** API now aligns better with the regular calling convention that C++ uses (`getMethod` vs `method`) while still remaining fully **async** using `Promise`es.
- [QML ĐApp](https://github.com/ethereum/go-ethereum/wiki/QML-PoC6-API) development.
The **QML** API is far from finished but shares common methods with the **JavaScript** API (just without the promises) and a few additional methods. A sample application is provided in Mist; **JeffCoin**. The source code can be found in the [mist repo](https://github.com/ethereum/go-ethereum/tree/master/mist/assets/qml/views/jeffcoin).
- [Mutan 0.5](https://github.com/ethereum/go-ethereum/wiki/Mutan-0.5#basic-syntax)
  -  `{ /* code */ }` style to define a new local scope.
  - `else if` statement.
  - Logical `and` and `or` (`&&` `||`)
  - `this` changed in favour of `message`
- Improved debugger (Mutan stack info)
- Embedded HTML renderer.

## [v0.6.0](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.0) (2014-07-25)

![wqz0qsidvoykkp7ngbrqfpftbinb3i927gh6xu3jkk4](https://cloud.githubusercontent.com/assets/6264126/3699761/47fd03fe-13d8-11e4-8509-4aa713e10419.png)
This release _fixes_ and _improves_ the following:
- Improved catching up. Previous build would sometimes fail to catch up or stop for no obvious reason. This should now be fixed
- Fixed the VM. This caused issues with processing certain contracts.
- If a connection fails to a peer it will continue trying for a couple of times.
- Peers can be pushed towards clients even when the clients hasn't asked for it. Previously builds would ignore the peer server's `peer message`.
- Fixed retina issue
- Faster startup time
Additionally I've decided to give some flavour to the releases; names of satellites.

## [0.5.19](https://github.com/ethereum/go-ethereum/releases/tag/0.5.19) (2014-07-23)

Fixed zero division error

## [0.5.18](https://github.com/ethereum/go-ethereum/releases/tag/0.5.18) (2014-07-22)

PoC5 Release

