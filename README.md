# Ethereum Go Client P2P-Related Changes  
  
## [v1.15.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.9) (2025-04-21)  
  
- - Geth will now occasionally drop peers at random after being fully synced. (#31476)  
- - Peer disconnect metrics are improved. (#31629, #31621)  
  
## [v1.15.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.8) (2025-04-11)  
  
- #### P2P networking  
- - The discv5 'talk request' API has been changed to pass `*enode.Node` to handlers. This is a breaking change, but the only known user of this API is the shisui portal network client. (#31075)  
- - A flaw in the recently added discv5 challenge resend logic was fixed. (#31543)  
- - The eth protocol now properly handles very large `skip` values when processing `GetBlockHeaders` messages from peers. This is not a security fix, despite looking like one, it's more about correctness. (#31522)  
  
## [v1.15.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.7) (2025-03-31)  
  
- This is a bug fix release. We are putting is out specifically to address a critical issue that could break archive node databases.  
- - Fixed an issue for `--state.scheme=hash` where the log indexer would accidentally delete trie nodes. (#31525)  
  
## [v1.15.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.6) (2025-03-25)  
  
- 🚧  **Note: we are investigating an issue with this release that affects archive nodes (--gcmode=archive).  
- - [abigen v2](https://geth.ethereum.org/docs/developers/dapp-developer/native-bindings-v2) is finally here. abigen is a tool for creating Go bindings for Solidity contracts. In v1, the generated bindings presented an API for sending transactions, filtering logs, and performing read-only calls as Go methods on the contract object. In the new version, we have updated the interface of the generated code to focus purely on encoding and decoding ABI payloads. Generic helper functions are provided in a library package to enable the same interactions as before, but you can also use your own custom method of signing & sending transactions. Generated bindings are also significantly smaller. (#31379)  
- - The performance of transaction signature validation has improved significantly (#31242, #31258, #31434)  
  
## [v1.15.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.5) (2025-03-05)  
  
- The Sepolia testnet may take a short while to recover while the update is adopted by nodes. An incident report will be published once the network has fully recovered. ❤️‍🩹   
## [v1.15.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.4) (2025-03-01)  
  
- **Note: you need to upgrade to v1.15.3 or this release to be compatible with the Pectra fork on the Sepolia network (activates Wed, Mar 5 at 07:29:36 UTC)**.  
- - Certain invalid blob transactions no longer cause disconnect issues in the p2p layer. (#31219)  
  
## [v1.15.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.3) (2025-02-25)  
  
- Oh look, another hotfix release! We are issuing this Geth release to correct the predefined fork configuration of the Holesky and Sepolia testnets. The deposit contract address was missing in the configuration for these networks, causing a chain validation failure.  
- This issue was discovered on the Holesky network after it had already forked into Pectra (Prague). As a reminder, the Sepolia network will fork to Pectra at slot 7118848 (Wed, Mar 5 at 07:29:36 UTC). You need to upgrade to Geth v1.15.3 until then in order to use the testnet after the fork.  
- - A peer-finding issue with discovery v5 is fixed in this release. (#31251)  
- - An invalid `--discovery.dns` flag value will now cause an error at Geth startup. (#31233)  
- - Encoding of nested byte arrays in EIP-712 signature processing was fixed. (#31049)  
  
## [v1.15.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.2) (2025-02-17)  
  
- - Discv5 and DNS peer discovery has been restored. It was accidentally disabled in v1.14.9. ([#31185](https://github.com/ethereum/go-ethereum/pull/31185))  
- - An edge-case for `geth dumpconfig` related to the `P2P.NAT` setting in TOML is fixed. ([#31192](https://github.com/ethereum/go-ethereum/pull/31192))  
  
## [v1.15.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.1) (2025-02-13)  
  
- This release enables the Prague fork for the Holesky and Sepolia testnets. It is a required upgrade if you want to participate in these networks.  
- - A v1.15.0 regression was identified in genesis handling for custom networks, related to the new `blobSchedule` setting. Geth v1.15.1 has been updated no longer crash in this case, and we will have an improved fix in v1.15.2 (#31171)  
- - The supranational/blst dependency has been upgraded to v0.3.14 in order to allow building Geth with Go 1.24. (#31165)  
  
## [v1.15.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.15.0) (2025-02-06)  
  
- - Similarly, you can now set the NAT traversal method (`--nat` flag) with the config file. ([#31041](https://github.com/ethereum/go-ethereum/pull/31041))  
- - The `bootnode` utility has been removed. ([#30813](https://github.com/ethereum/go-ethereum/pull/30813))  
- ### Core Library & Networking  
- - The transaction pool's notion of "locals" has been changed. This is a breaking change in some ways, and requires some explanation. Geth used to accept transactions at any fee level via RPC, storing them into the node's pool, and marking the sender as "local". When creating a new block (or the pending block), it would choose "local" transactions first. Finally, Geth tracks "local" transactions in a persistent journal file, and restores them on startup. The effect of all this is that once you had sent a transaction to Geth via RPC, it would keep prioritizing the sender account and its transactions (even ones received through p2p).  
- - For static nodes (from configuration or RPC `admin_addPeer`), the node URL can contain a DNS name. Geth will now resolve these names when attempting to connect to the peer, instead of at configuration parsing time. This is useful for setting up testing networks in Docker or Kubernetes where the IP address of a node might not be known ahead of time. ([#30822](https://github.com/ethereum/go-ethereum/pull/30822))  
- - Geth can now use the STUN protocol to figure out its own IP. There is a built-in list of public servers, or you can specify your own. Use `--nat=stun` to enable. ([#31064](https://github.com/ethereum/go-ethereum/pull/31064))  
- - Some minor bugs in the p2p protocol implementation got fixed. ([#30855](https://github.com/ethereum/go-ethereum/pull/30855), [#30918](https://github.com/ethereum/go-ethereum/pull/30918))  
  
## [v1.14.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.13) (2025-01-30)  
  
- **Please update your nodes ASAP.**  
  
## [v1.14.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.11) (2024-10-01)  
  
- - Ability to disable `FINDNODE` liveness checks in tests [#30512](https://github.com/ethereum/go-ethereum/pull/30512)  
  
## [v1.14.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.10) (2024-09-27)  
  
- Geth v1.14.10 is a hotfix release to fix a [blob pool regression](https://github.com/ethereum/go-ethereum/pull/30518) introduced in v1.14.9. Users running the previous bad version should update ASAP. That said, there is no immediate danger to these users, just to the health of blob transaction propagation and inclusion in the network.  
  
## [v1.14.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.9) (2024-09-18)  
  
- * Fetch transactions from a peer in the order they were announced in order to minimize nonce gap, causing blob txs to rejected. A special rule is applied to blob transactions: they are retrieved from the network upon reception of the announcement, as blob transactions are never broadcast over the p2p network (https://github.com/ethereum/go-ethereum/pull/30125)  
- ## Networking  
- * Add support for quic entry in ENR (https://github.com/ethereum/go-ethereum/pull/30283)  
- * fix Write method in metered connection (https://github.com/ethereum/go-ethereum/pull/30355)  
- * Enable discv5 by default (https://github.com/ethereum/go-ethereum/pull/30327)  
- * Dial nodes from discv5 (https://github.com/ethereum/go-ethereum/pull/30302)  
  
## [v1.14.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.8) (2024-08-12)  
  
- - `core/types.Transaction.ChainID` had a bug where it modified the signature for very large ChainID (>= 2^64). ([#30157](https://github.com/ethereum/go-ethereum/pull/30157))  
- - `ethclient.Client.NetworkID` now supports values returned in hex format by the server. ([#30263](https://github.com/ethereum/go-ethereum/pull/30263))  
- - The package `p2p/simulations` has been removed. ([#30250](https://github.com/ethereum/go-ethereum/pull/30250))  
- ### Networking  
- - The new discovery node revalidation could hot-spin in certain rare scenarios. ([#30239](https://github.com/ethereum/go-ethereum/pull/30239))  
- - Configuring an external IP using `--nat=extip:...` could lead to invalid discovery packets being generated. ([#30234](https://github.com/ethereum/go-ethereum/pull/30234))  
  
## [v1.14.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.6) (2024-07-02)  
  
-  * Set a 2KB hard limit for p2p handshake messages ([#30029](https://github.com/ethereum/go-ethereum/pull/30029))  
-  * Add missing lock in peer discovery ([#29960](https://github.com/ethereum/go-ethereum/pull/29960))  
  
## [v1.14.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.5) (2024-06-06)  
  
- Geth v1.14.5 is a hotfix release that addresses a regression introduced in v1.14.4, which prevented the node from discovering other peers in certain networking setups ([#29944](https://github.com/ethereum/go-ethereum/pull/29944)). It is otherwise identical to [v1.14.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.4).  
- - Reduce the default required minimum miner tip from 1 gwei to 0.001 gwei to cater better for network conditions ([#29895](https://github.com/ethereum/go-ethereum/pull/29895)).  
- - Load trie nodes concurrently with trie updates, speeding up block import by 5-7% ([#29519](https://github.com/ethereum/go-ethereum/pull/29519), [#29768](https://github.com/ethereum/go-ethereum/pull/29768), [#29919](https://github.com/ethereum/go-ethereum/pull/29919)).  
- - Improve the discovery protocol's node revalidation ([#29572](https://github.com/ethereum/go-ethereum/pull/29572), [#29864](https://github.com/ethereum/go-ethereum/pull/29864), [#29836](https://github.com/ethereum/go-ethereum/pull/29836)).  
- - Fix a iteration order when using a trie node iterator ([#27838](https://github.com/ethereum/go-ethereum/pull/27838)).  
- - Fix a TCP/UDP discovery port test in cmd/devp2p ([#29879](https://github.com/ethereum/go-ethereum/pull/29879)).  
- - Fix IPv6 endpoint determination ([#29801](https://github.com/ethereum/go-ethereum/pull/29801), [#29827](https://github.com/ethereum/go-ethereum/pull/29827)).  
  
## [v1.14.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.4) (2024-06-05)  
  
- - Reduce the default required minimum miner tip from 1 gwei to 0.001 gwei to cater better for network conditions ([#29895](https://github.com/ethereum/go-ethereum/pull/29895)).  
- - Load trie nodes concurrently with trie updates, speeding up block import by 5-7% ([#29519](https://github.com/ethereum/go-ethereum/pull/29519), [#29768](https://github.com/ethereum/go-ethereum/pull/29768), [#29919](https://github.com/ethereum/go-ethereum/pull/29919)).  
- - Improve the discovery protocol's node revalidation ([#29572](https://github.com/ethereum/go-ethereum/pull/29572), [#29864](https://github.com/ethereum/go-ethereum/pull/29864), [#29836](https://github.com/ethereum/go-ethereum/pull/29836)).  
- - Fix a iteration order when using a trie node iterator ([#27838](https://github.com/ethereum/go-ethereum/pull/27838)).  
- - Fix a TCP/UDP discovery port test in cmd/devp2p ([#29879](https://github.com/ethereum/go-ethereum/pull/29879)).  
- - Fix IPv6 endpoint determination ([#29801](https://github.com/ethereum/go-ethereum/pull/29801), [#29827](https://github.com/ethereum/go-ethereum/pull/29827)).  
  
## [v1.14.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.3) (2024-05-09)  
  
- - `eth_createAccessList` now honors request cancellation and terminates background work ([#29686](https://github.com/ethereum/go-ethereum/pull/29686))  
  
## [v1.14.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.2) (2024-05-08)  
  
- - `eth_createAccessList` now honors request cancellation and terminates background work ([#29686](https://github.com/ethereum/go-ethereum/pull/29686))  
  
## [v1.14.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.14.0) (2024-04-24)  
  
- - Geth v1.14.0 switches over the default state trie representation from `hash` mode to `path` mode (i.e. `--state.scheme` flipped form `hash` to `path`) ([#29108](https://github.com/ethereum/go-ethereum/pull/29108)). This change does not affect Geth instances with pre-existing databases, in the case of which Geth continues to use whatever the existing database's format is. If no previous database exists however, for full nodes, Geth will now default to `pathdb`. The main advantage is built-in, online historical state pruning; no more runaway state growth.  
-   - **If you want to force the old behaviour, you can run Geth with `--state.scheme=hash` for now.** That said, we will be dropping `hash` mode sooner rather than later, so we advise everyone running full nodes to gradually switch, if they haven't yet.  
-   - **Archive mode support is not yet finalised for `path` mode, so archive nodes will still run in `hash` mode.** Naturally, hash mode will not be dropped until a full `path` archive lands and people have enough time to switch to it.  
- - Geth v1.14.0 introduces a brand new *live-tracing* feature, where one or more transaction tracers might be injected into the block processing pipeline, ensuring that tracing and execution happen in lockstep ([#29189](https://github.com/ethereum/go-ethereum/pull/29189)). Since Go does not have a cross platform, OS native plugin infrastructure, adding live tracers needs to be done at the Geth source code level, and Geth itself subsequently rebuilt. That said, the advantage is that such tracers have full execution flexibility to do whatever they like and however they like. Please see the live-tracer [changelog](https://github.com/ethereum/go-ethereum/blob/master/core/tracing/CHANGELOG.md) and [docs](https://geth.ethereum.org/docs/developers/evm-tracing/live-tracing) for details.  
-   - **Live tracing runs in lockstep with block execution, one waiting for the other.** You should never run a live tracer on a validating node, or any other where latency is important. The recommended practice is to have tracers collect and export the bare minimum data needed and do any post-processing in external systems where latency is not relevant.  
-   - **The live tracer work required a number of breaking internal API changes.** If you had your own native tracers implemented before this change, the [changelog](https://github.com/ethereum/go-ethereum/blob/master/core/tracing/CHANGELOG.md) contains the necessary steps needed to update your old code for the new APIs.  
- - Geth v1.14.0 replaces the completely random transaction propagation paths (in the Ethereum P2P network) with pseudo-random ones, that ensure transactions from the same account follow the same path in the network (as long as no peer churn happens). The purpose is to allow a burst of transactions from the same account to ripple through the network on the same connections, minimising reordering. Whilst this doesn't provide any guarantees, nor does it replace the potential need for more complex routing logic, it should make nonce gaps significantly less likely, reducing the probability of dropped transactions during bursts ([#29034](https://github.com/ethereum/go-ethereum/pull/29034)).  
-   - **This change only applies to networking, so other clients are not required to follow suit.** That said, the network would behave more consistently overall if all clients implemented some similar logic (the exact routing algorithm is irrelevant, as long as it's stable in the accounts).  
- - Geth v1.14.0 drops support for running pre-merge networks ([#29169](https://github.com/ethereum/go-ethereum/pull/29169)). This does not mean that Geth will not be able to process or validate pre-merge blocks, rather that starting v1.14.0, Geth *must* have a consensus client drive it's chain selection. Geth drops the *forward-sync* mode of operation, where it imports blocks based on PoW (ethash) or PoA (clique) difficulties.  
-   - **Non-merged networks will need to stick to Geth v1.13.x until they transition to post-merge.** As Ethereum has transitioned towards a post-merge world, we cannot maintain a significant chunk of code that's been dead for 2 years now on mainnet.  
-   - **Post-merge networks must be marked so with the `terminalTotalDifficultyPassed: true` field in their `genesis.json`.** This requirement will be dropped (along with even caring at all about this field) in a few releases, but for now this field produces a clear error message for the user of what's wrong with their pre-merge network.  
- - Geth v1.14.0 stops automatically constructing the pending block ([#28623](https://github.com/ethereum/go-ethereum/pull/28623)). Performance wise there is a significant cost to creating a potential pending block, yet most node operators do not care about it. Not even validators need the pending block. This work also drops support for mining/signing a block "off-demand" (i.e. not via the engine API, rather by Geth itself), meaning Clique signing is also removed going forward.  
- - Geth v1.14.0 ships a *beacon chain* light client ([#28822](https://github.com/ethereum/go-ethereum/pull/28822), [#29308](https://github.com/ethereum/go-ethereum/pull/29308), [#29335](https://github.com/ethereum/go-ethereum/pull/29335), [#29532](https://github.com/ethereum/go-ethereum/pull/29532), [#29567](https://github.com/ethereum/go-ethereum/pull/29567)). It uses the REST API of beacon nodes and requires the `beacon / light_client namespace`, currently supported by Lodestar and Nimbus ([specs](https://ethereum.github.io/beacon-APIs/#/Beacon)).  
-   - **The beacon light client provides access to consensus layer data, not execution layer data.** This currently means that it can follow the beacon chain as a light client and extract the latest head and finalised execution layer block. That can be used to drive a full execution node without running an own beacon node. There is no support yet for a full execution light client.  
-   -  **Validation and staking is not possible with a beacon light client.** Since the sync committee signatures on each beacon head are canonised in the next block, the beacon light client can only follow the chain with one slot delay.  
-   - **The beacon light client exists both as a standalone executable and integrated into Geth.** The `blsync` executable can drive any execution layer node through the standard engine API or can just do a "test run" printing block numbers and hashes to the console. The integrated mode can do the same inside Geth. If you specify at least one suitable consensus layer REST API endpoint with the `--beacon.api` flag, then Geth will run without a beacon node connected through the engine API.  
- - Switch to using Go's native `log/slog` package instead of `golang/exp` ([#29302](https://github.com/ethereum/go-ethereum/pull/29302)).  
- - Track not only total peer count, but also how many are inbound or outbound ([#29424](https://github.com/ethereum/go-ethereum/pull/29424)).  
- - Fix the `devp2p` command to not ignore specified bootnodes ([#29091](https://github.com/ethereum/go-ethereum/pull/29091)).  
- - Fix an ENR RLP decoding issue in the `devp2p` command ([#29257](https://github.com/ethereum/go-ethereum/pull/29257)).  
- - Fix ancient header reader to hard cap at 2MB (network soft limit) ([#29534](https://github.com/ethereum/go-ethereum/pull/29534)).  
  
## [v1.13.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.14) (2024-02-27)  
  
- - Disallow blob transactions below the protocol minimum of 1 wei to enter the pool ([#29081](https://github.com/ethereum/go-ethereum/pull/29081)).  
  
## [v1.13.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.12) (2024-02-09)  
  
- This release embeds the mainnet fork number for Cancun, scheduled to go live on 13th March, 2024 (unix `1710338135`). The specification can be read [here](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/cancun.md), and it contains the following changes:  
  
## [v1.13.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.11) (2024-01-24)  
  
- This release fixes a few bugs and enables the Cancun upgrade for the Sepolia and Holesky networks; Sepolia will upgrade on Jan 31, and Holesky on Feb 7, and naturally this is a required upgrade if you intend to follow either chain.    
  
## [v1.13.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.10) (2024-01-11)  
  
- This release fixes a few issues and **enables the Cancun upgrade for the Goerli network** at block timestamp 1705473120 ([#28719](https://github.com/ethereum/go-ethereum/pull/28719)) which is 6:32:am 17. Jan. 2024 UTC.  
  
## [v1.13.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.9) (2024-01-10)  
  
- This release fixes a few issues and **enables the Cancun upgrade for the Goerli network** at block timestamp 1705473120 ([#28719](https://github.com/ethereum/go-ethereum/pull/28719)) which is 6:32:am 17. Jan. 2024 UTC.  
  
## [v1.13.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.8) (2023-12-22)  
  
- This is a hotfix release for a regression which affects v1.13.6 and v1.13.7: if the node is shut down during sync, the node will refuse to start, with the error message `Fatal: Failed to register the Ethereum service: waiting for sync` ([#28718](https://github.com/ethereum/go-ethereum/pull/28718), [#28724](https://github.com/ethereum/go-ethereum/pull/28724)).  
  
## [v1.13.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.6) (2023-12-18)  
  
- Geth v1.13.6 is a scheduled maintenance release, but it also contains some changes which might affect node operators, concerning logging.   
-  The external-facing API is largely the same as the existing Geth logger.  Method signatures remain unchanged. A small semantic difference is that a `Handler` can only be set once per `Logger` and not changed dynamically.  This just means that a new logger must be instantiated every time the handler of the root logger is changed.  
- * Fixes an issue where node liveness was not always verified correctly in discv5 ([#28686](https://github.com/ethereum/go-ethereum/pull/28686))  
  
## [v1.13.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.4) (2023-10-17)  
  
- Geth v1.13.4 is a non-urgent hotfix release. The previous version of Geth (v1.13.3) introduced a warning log for bad transaction announcements, and on mainnet it generated too much logging noise due to a protocol violation in Erigon. To prevent overwhelming logging systems, Geth v1.13.4 lower the log to a more reasonable level until the bug in Erigon is fixed [#28356](https://github.com/ethereum/go-ethereum/pull/28356).  
  
## [v1.13.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.3) (2023-10-12)  
  
- - Drop support for `eth/66` (Cancun will require `eth/68` anyway) ([#28239](https://github.com/ethereum/go-ethereum/pull/28239)).  
- - Lower `snap` missing `eth` protocol warning to debug level ([#28249](https://github.com/ethereum/go-ethereum/pull/28249)).  
- - Enforce transaction metadata announcements in `eth/68` ([#28261](https://github.com/ethereum/go-ethereum/pull/28261)).  
- - Enable blob transaction propagation and mining in Cancun networks ([#28243](https://github.com/ethereum/go-ethereum/pull/28243)).  
  
## [v1.13.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.2) (2023-09-28)  
  
- - Fix various pathdb corruption corner-cases during snap sync node restart ([#28171](https://github.com/ethereum/go-ethereum/pull/28171), [#28163](https://github.com/ethereum/go-ethereum/pull/28163)).  
- - Fix `--bootnodes` flag if the list is also configured in the toml file ([#28095](https://github.com/ethereum/go-ethereum/pull/28095)).  
  
## [v1.13.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.13.0) (2023-09-12)  
  
- Still, just to quickly recap, Geth v1.13.0 finally ships a new database model which supports proper, full pruning of historical states; meaning you will never need to take your node offline again to resync or to manually prune. The new database model is optional for now (you need to enable it via `--state.scheme=path`) and does require resyncing the state, since we need to store it completely different (you can keep your ancients, no need to resync the chain too).  
- The path database will become the default eventually, but for safety reasons, we're keeping it opt-in for the moment. The old database model is not going away soon, though long term - unless there's something fundamentally wrong with the path db -  it will. As for archive node users, we're working on a new model there too, but it does need a bit more work on top, so that's for another release.  
- ***The all important disclaimer: Geth's new path-based storage is considered stable and production ready, but was obviously not battle tested yet outside of the team. Everyone is welcome to use it, but if you have significant risks if your node crashes or goes out of consensus, you might want to wait a bit to see if anyone with a lower risk profile hits any issues.***  
- - Fix forkid computation for genesis-merged non-zero timestamp networks ([#27895](https://github.com/ethereum/go-ethereum/pull/27895), [#28034](https://github.com/ethereum/go-ethereum/pull/28034)).  
  
## [v1.12.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.12.1) (2023-08-10)  
  
- - We have also implemented an entirely new mempool -- the blobpool -- for EIP-4844 transactions. The new pool is backed by an on-disk database and has an overall simpler design than the txpool we use for regular transactions. The blobpool is not yet used by p2p networking and is still being tested in the Cancun devnets. ([#26940](https://github.com/ethereum/go-ethereum/pull/26940), [#27429](https://github.com/ethereum/go-ethereum/pull/27429), [#27463](https://github.com/ethereum/go-ethereum/pull/27463), [#27481](https://github.com/ethereum/go-ethereum/pull/27481), [#27790](https://github.com/ethereum/go-ethereum/pull/27790), [#27822](https://github.com/ethereum/go-ethereum/pull/27822))  
- - `Node.Attach` no longer returns an error. This is a breaking Go API change. ([#27450](https://github.com/ethereum/go-ethereum/pull/27450))  
- - All block creation activity is now paused while the node is syncing ([#27218](https://github.com/ethereum/go-ethereum/pull/27218))  
- #### Networking  
- - Large transactions (> 4kB) are no longer broadcasted to peers. This resolves a potential network congestion issue ([#27618](https://github.com/ethereum/go-ethereum/pull/27618))  
- - The p2p networking layer has learned to announce alternate ports returned by UPnP/NAT-PMP ([#26359](https://github.com/ethereum/go-ethereum/pull/26359))  
- - The p2p server now properly tracks all peer goroutines ([#27887](https://github.com/ethereum/go-ethereum/pull/27887))  
- - Networking initialization now really disables all discovery when `--nodiscover` is used ([#27518](https://github.com/ethereum/go-ethereum/pull/27518))  
- - Obsolete parts of the LES protocol implementation, which is currently non-functional, have been removed ([#27737](https://github.com/ethereum/go-ethereum/pull/27737))  
- - Discovery bootstrap nodes will now be filtered by the netrestrict setting, like all other nodes ([#27701](https://github.com/ethereum/go-ethereum/pull/27701))  
- - We now provide additional metrics around p2p dialing, making it possible to measure the efficieny of peer discovery ([#27621](https://github.com/ethereum/go-ethereum/pull/27621))  
- - A very rare crash related to peer connection tracking is resolved ([#27665](https://github.com/ethereum/go-ethereum/pull/27665))  
- - It is now possible to configure certain discovery internals for experimentation ([#27387](https://github.com/ethereum/go-ethereum/pull/27387))  
  
## [v1.11.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.6) (2023-04-20)  
  
- - New sepolia bootnodes managed by EF devops are added ([#27099](https://github.com/ethereum/go-ethereum/pull/27099))  
  
## [v1.11.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.5) (2023-03-21)  
  
- - Changes needed use overlay protocols in discv5 ([#26699](https://github.com/ethereum/go-ethereum/pull/26699))  
  
## [v1.11.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.4) (2023-03-10)  
  
- - EF bootstrap nodes on MS Azure have been removed because they will be decommissioned soon. ([#26828](https://github.com/ethereum/go-ethereum/pull/26828))  
  
## [v1.11.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.3) (2023-03-07)  
  
- - `head` and `difficulty` have been removed in `admin_peerInfo` responses. ([#26804](https://github.com/ethereum/go-ethereum/pull/26804))  
  
## [v1.11.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.11.0) (2023-02-15)  
  
- Most of the code for the [Shanghai fork][shanghai] is merged into 1.11, but activation of the fork is not configured. The Shanghai protocol upgrade will contain the ability to make withdrawals from the consensus-layer (beacon chain). Withdrawals are specified in [EIP-4895: Beacon chain push withdrawals as operations](https://eips.ethereum.org/EIPS/eip-4895).  
- [shanghai]: https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/shanghai.md  
- We have removed support for the `ropsten` and `kiln` test networks. We have also removed libraries for mobile development and the `puppeth` tool.  
- - `static-nodes.json`  
- - `trusted-nodes.json`  
- This change does not affect proof-of-stake networks because the fee recipient address is provided by the consenus-layer and not configured in Geth anymore.  
- We have added a method `debug_setTrieFlushInterval` to make it possible to set the trie flush interval via RPC ([#24785](https://github.com/ethereum/go-ethereum/pull/24785)). Essentially, this makes it possible to configure the node to be more or less "archive:ish", and without restarting the node while reconfiguring it.  
- - p2p/discover: add more packet information in logs ([#26307](https://github.com/ethereum/go-ethereum/pull/26307))  
-   - Implement `eth/68` to support larger transactions over the wire ([#25980](https://github.com/ethereum/go-ethereum/pull/25980))  
-  - p2p/discover: improve discv5 NODES response packing ([#26033](https://github.com/ethereum/go-ethereum/pull/26033))  
- - node: drop support for static & trusted node list files ([#25610](https://github.com/ethereum/go-ethereum/pull/25610))  
- - node, rpc: add JWT auth support in client ([#24911](https://github.com/ethereum/go-ethereum/pull/24911))  
- - eth/fetcher: throttle peers which deliver many invalid transactions ([#25573](https://github.com/ethereum/go-ethereum/pull/25573))  
- - eth/tracers: fix gasUsed for native and JS tracers ([#26048](https://github.com/ethereum/go-ethereum/pull/26048))  
- - graphql, node, rpc: improve HTTP write timeout handling ([#25457](https://github.com/ethereum/go-ethereum/pull/25457))  
- - p2p/discover: fix handling of distance 256 in lookupDistances ([#26087](https://github.com/ethereum/go-ethereum/pull/26087))  
- - eth/tracers: fix gasUsed for native and JS tracers ([#26048](https://github.com/ethereum/go-ethereum/pull/26048))  
- - node: fix HTTP server always force closing ([#25755](https://github.com/ethereum/go-ethereum/pull/25755))  
- - eth/protocols/snap: throttle trie heal requests when peers DoS us ([#25666](https://github.com/ethereum/go-ethereum/pull/25666))  
- - eth/protocols/snap: fix problems due to idle-but-busy peers  
  
## [v1.10.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.25) (2022-09-15)  
  
- Geth v1.10.25 is a tiny update to flip the mainnet chain configuration to be post-merge. This disables legacy sync and will prevent Geth from even starting sync until a consensus client is attached and sends forkchoice updates. The update prevents bad PoW actors from trying to get the node to - temporarily, but annoyingly - download bad state data.  
  
## [v1.10.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.23) (2022-08-24)  
  
- If anyone updated to v1.10.22 in these past couple of days, there is a fairly high probability that some state data might have gone missing from your node. Doing a full check on the state is possible `geth snapshot traverse-state`, but will likely take a day and the fix is all the same anyway.  
- To ensure that your node has all the data, please rewind your local chain to a block before you updated (if unsure, just pick a block before the release time) with `debug.setHead("0xblock-number-in-hex")` via the Geth console (on IPC), or `debug_setHead` via JSON RPC (you might need to *temporarilly* expose the `debug` namespace to do that). The brute force alternative of course is to resync after an update, which you can do by deleting your `chaindata` folder (but please leave the `ancient` folder within to keep the blocks).  
- - Storage of trie node hash preimages is now disabled by default. You can enable it again using the `--cache.preimages` flag. ([#25287](https://github.com/ethereum/go-ethereum/pull/25287), [#25538](https://github.com/ethereum/go-ethereum/pull/25538), [#25533](https://github.com/ethereum/go-ethereum/pull/25533))  
- - The ethash mining implemenation now removes temporary DAG files, which could be left of disk when geth was interrupted while generating a DAG. ([#25381](https://github.com/ethereum/go-ethereum/pull/25381))  
- - The eth wire protocol test suite now supports protocol version eth/67. ([#25306](https://github.com/ethereum/go-ethereum/pull/25306))  
- - RLP-decoding of trie nodes is ~33% faster due to reduced allocations in the decoder. ([#25357](https://github.com/ethereum/go-ethereum/pull/25357))  
  
## [v1.10.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.22) (2022-08-22)  
  
- - Storage of trie node hash preimages is now disabled by default. You can enable it again using the `--cache.preimages` flag. ([#25287](https://github.com/ethereum/go-ethereum/pull/25287), [#25538](https://github.com/ethereum/go-ethereum/pull/25538), [#25533](https://github.com/ethereum/go-ethereum/pull/25533))  
- - The ethash mining implemenation now removes temporary DAG files, which could be left of disk when geth was interrupted while generating a DAG. ([#25381](https://github.com/ethereum/go-ethereum/pull/25381))  
- - The eth wire protocol test suite now supports protocol version eth/67. ([#25306](https://github.com/ethereum/go-ethereum/pull/25306))  
- - RLP-decoding of trie nodes is ~33% faster due to reduced allocations in the decoder. ([#25357](https://github.com/ethereum/go-ethereum/pull/25357))  
  
## [v1.10.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.21) (2022-07-27)  
  
- - The `--netrestrict` option is now also applied for discv5. ([#25304](https://github.com/ethereum/go-ethereum/pull/25304))  
- - DNS discovery is now enabled for the Sepolia testnet. ([#25288](https://github.com/ethereum/go-ethereum/pull/25288))  
- - The HTTP RPC server will no longer hang on shutdown even with very busy connections. ([#25258](https://github.com/ethereum/go-ethereum/pull/25258))  
  
## [v1.10.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.20) (2022-06-29)  
  
- - The new `--discovery.port` flag allows configuring a separate port for the UDP listener. ([#24979](https://github.com/ethereum/go-ethereum/pull/24979))  
- - Setting p2p bootstrap nodes in the config file now works even when a pre-defined network is selected on the command-line. ([#25174](https://github.com/ethereum/go-ethereum/pull/25174))  
  
## [v1.10.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.19) (2022-06-15)  
  
- - Updates related to The Merge on Ropsten, which is now a proof-of-stake network ([#25018](https://github.com/ethereum/go-ethereum/pull/25018), [#24975](https://github.com/ethereum/go-ethereum/pull/24975), [#25078](https://github.com/ethereum/go-ethereum/pull/25078))!  
- - Introduce `eth/67` protocol, dropping support for GetNodeData ([#24093](https://github.com/ethereum/go-ethereum/pull/24093)).  
  
## [v1.10.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.18) (2022-05-25)  
  
- Please ensure you have a beacon chain node configured for the transition. You can find more information about preparing for the Merge in our guide: <https://geth.ethereum.org/docs/interface/merge>.  
- - The new `geth db check-state-content` command checks integrity of trie nodes in the database. ([#24840](https://github.com/ethereum/go-ethereum/pull/24840))  
- - The Goerli testnet has new bootstrap nodes. ([#24900](https://github.com/ethereum/go-ethereum/pull/24900))  
- - The snap protocol server no longer includes superfluous account proofs when a storage response hits the size limit. ([#24885](https://github.com/ethereum/go-ethereum/pull/24885))  
- - The snap client now de-duplicates trie node heal requests better, sorting them by the requested state trie path. This can slightly reduce the overall amount of data transferred during snap sync. ([#24779](https://github.com/ethereum/go-ethereum/pull/24779))  
- - The eth block fetcher now disconnects peers that repeatedly fail to reply to header requests. ([#24652](https://github.com/ethereum/go-ethereum/pull/24652))  
- - The trie implementation can now optionally trace nodes which were deleted/overwritten by state updates. ([#24403](https://github.com/ethereum/go-ethereum/pull/24403))  
- - ethclient: `Client` now has a `PeerCount` method. ([#24849](https://github.com/ethereum/go-ethereum/pull/24849))  
- - signer/fourbyte: 4byte signatures have been updated. ([#22865](https://github.com/ethereum/go-ethereum/pull/22865), [#24842](https://github.com/ethereum/go-ethereum/pull/24842))  
- - **API change**: In all tracers (JS, native, structlog), memory content is now captured before the EVM expands it. Previously, tracing would see memory after it had already been affected by the current instruction. ([#24867](https://github.com/ethereum/go-ethereum/pull/24867))  
- - In native (Go) tracers, the txhash/blockhash being traced is now available. ([#24679](https://github.com/ethereum/go-ethereum/pull/24679))  
  
## [v1.10.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.16) (2022-02-16)  
  
- -  The `devp2p` binary now supports doing `snap/v1` protocol testing against a remote node, which can be used for Hive-testing ([#24276](https://github.com/ethereum/go-ethereum/pull/24276)).  
- - Tracing was improved by making the `prestate` tracer be a native tracer ([#24320](https://github.com/ethereum/go-ethereum/pull/24320), [#24268](https://github.com/ethereum/go-ethereum/pull/24268)).  
  
## [v1.10.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.15) (2022-01-05)  
  
- This release resolves a few regressions introduced by the previous release. Most importantly, it fixes an issue that could cause peer-to-peer 'eth' connections to lock up.   
  
## [v1.10.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.14) (2021-12-23)  
  
- #### Networking  
- - The eth protocol implementation now uses request IDs (added by eth/66) internally. ([#23576](https://github.com/ethereum/go-ethereum/pull/23576))  
- - Serving ancient headers to other peers has been optimized. ([#23105](https://github.com/ethereum/go-ethereum/pull/23105))  
- - The discv4 test suite is more robust and logs received packets better. ([#23966](https://github.com/ethereum/go-ethereum/pull/23966))  
- - There are now fuzz tests for the snap protocol message handler. ([#23957](https://github.com/ethereum/go-ethereum/pull/23957))  
  
## [v1.10.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.13) (2021-11-24)  
  
- - Implement the 4byte tracer natively in Go ([#23882](https://github.com/ethereum/go-ethereum/pull/23882), [#23916](https://github.com/ethereum/go-ethereum/pull/23916)).  
- - Fix log retrievals for users with very old archive nodes having legacy database formats ([#23879](https://github.com/ethereum/go-ethereum/pull/23879)).  
- - Fix a snap sync issue where a malicious response could crash the syncing node ([#23960](https://github.com/ethereum/go-ethereum/pull/23960)).  
- - Fix DNS discovery entry TTLs on Clouflare ([#23885](https://github.com/ethereum/go-ethereum/pull/23885)).  
  
## [v1.10.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.12) (2021-11-08)  
  
- **The release enables the [Arrow Glacier hard-fork](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/arrow-glacier.md), scheduled approximately for the 8th of December. The sole change is to [postpone the difficulty-bomb](https://eips.ethereum.org/EIPS/eip-4345) until summer 2022, by which time hopefully The Merge will have happened.**  
- Lastly, the release also contains a brand new call tracer implemented in Go, which should be significantly (**2.5x**) faster than the one currently used. You can use the new tracer via `debug.traceTransaction("0xhash", {tracer: "callTracer"})`. The original JavaScript tracer is still available for fallback purposes called `callTracerLegacy`. The latter will be dropped if nobody reports issues with the native one.  
- - Bake in support for the Sepolia PoW test network ([#23730](https://github.com/ethereum/go-ethereum/pull/23730)).  
- - Switch the call tracer to a fast native Go implementation ([#23867](https://github.com/ethereum/go-ethereum/pull/23867), [#23708](https://github.com/ethereum/go-ethereum/pull/23708)).  
- - Fix a memory leak in Clique if the network temporarilly halts ([#23861](https://github.com/ethereum/go-ethereum/pull/23861)).  
  
## [v1.10.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.9) (2021-09-29)  
  
- - When initializing Clique-based private networks, zero-length extradata in genesis.json now prints an error message instead of crashing. ([#23538](https://github.com/ethereum/go-ethereum/pull/23538))  
- - Contract bindings created by accounts/abi/bind now validate log event signatures. This prevents accidentally decoding events with the wrong signature. ([#23230](https://github.com/ethereum/go-ethereum/pull/23230))  
- - Broken WebSocket connections are now detected better and their subscriptions report an error instead of hanging indefinitely. ([#23556](https://github.com/ethereum/go-ethereum/pull/23556))  
- - For clique blocks returned by RPC, the "miner" field once again contains the actual block coinbase field instead of the derived block signer. This fixes a regression where clients would no longer be able to verify the block seal signature. ([#23466](https://github.com/ethereum/go-ethereum/pull/23466))  
- #### Networking  
- - The eth/65 peer-to-peer protocol is no longer supported. Geth only supports eth/66 as of this release. ([#23456](https://github.com/ethereum/go-ethereum/pull/23456))  
- - The cross-client eth protocol tests suite better distinguishes eth/65 and eth/66. ([#23568](https://github.com/ethereum/go-ethereum/pull/23568))  
- - ENR sequence numbers are now initialized as a timestamp. This prevents issues when the p2p nodes database is dropped/re-created while keeping the nodekey the same. ([#19903](https://github.com/ethereum/go-ethereum/pull/19903))  
- - Several data races are resolved in packages p2p and p2p/enode. ([#23434](https://github.com/ethereum/go-ethereum/pull/23434))  
- - The 'node' package no longer depends on wallet backends. Specifically, this removes the dependencies on libusb for contract bindings and other uses of the go-ethereum library. If you are using package node, you must now register required account manager backends individually. ([#23019](https://github.com/ethereum/go-ethereum/pull/23019))  
  
## [v1.10.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.8) (2021-08-24)  
  
- The exact attack vector will be provided at a later date to give node operators and dependent downstream projects time to update their nodes and software. All Geth versions supporting the London hard fork are vulnerable (the bug is older than London), so all users should update.  
- Credits for the discovery go to @guidovranken (working for [Sentnl](https://sentnl.io/) during an audit of the [Telos EVM](https://www.telos.net/evm)) and reported via bounty@ethereum.org.  
  
## [v1.10.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.7) (2021-08-12)  
  
- - **The `-miner.gastarget` CLI flag was deprecated and is a noop.** This flag is already a noop for networks running the London hard-fork, since it London miners only take into account the `-miner.gaslimit` flag. For non-London private networks and Geth forks, this might result in a gas bump depending on how the miners are configured.  
  
## [v1.10.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.6) (2021-07-22)  
  
- During testing of the London hardfork on Ropsten, a consensus failure occurred in block 10679538, leading to a network split between OpenEthereum/Besu and Geth/Nethermind. The block contained a transaction from an account with enough funds to cover the effective fee, but too little funds for the transaction's maximum gas price. EIP-1559 mandates that such transactions should be rejected. Geth's implementation of EIP-1559 did not perform the check correctly and accepted the transaction.  
- - The Go API function `Node.Close()` has been fixed to stop the WebSocket server correctly ([#23211](https://github.com/ethereum/go-ethereum/pull/23211))  
  
## [v1.10.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.5) (2021-07-14)  
  
- - **For wallet providers:** The default transaction price calculation algorithm for EIP-1559 (`eth_maxPriorityFeePerGas`) follows the old mechanism, setting the `max priority fee` to the `effective price` paid on the network minus the current `base fee`; and setting the `max fee` to the `priority fee + 2x base fee`. This ensures that the total price paid per gas remains the same after the London transition if no explicit user involvement has been made.  
-   Alternatively, Geth exposes a new [`eth_feeHistory(blocks, head, percentiles)`](https://github.com/ethereum/eth1.0-specs/blob/b4ebe3d6056a2f5edb6f9411da58e042f7a95d2a/json-rpc/spec.json#L200) API endpoint which allows the user to query recent statistical infos about the amount of tips paid to miners and fees burned by transactions. Wallets are recommended to use this endpoint to give users multiple fee options to choose from ([#23033](https://github.com/ethereum/go-ethereum/pull/23033)).  
- - Alternate builders for docker images ([#23069](https://github.com/ethereum/go-ethereum/pull/23069), [#23078](https://github.com/ethereum/go-ethereum/pull/23078), [#23082](https://github.com/ethereum/go-ethereum/pull/23082), [#23083](https://github.com/ethereum/go-ethereum/pull/23083)).  
- - Fix a compatibility issue between old Geth nodes and new abigen code ([#23102](https://github.com/ethereum/go-ethereum/pull/23102)).  
  
## [v1.10.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.4) (2021-06-17)  
  
- - For private networks and future public testnets, the initial EIP-1559 basefee can also be set in genesis.json using the `baseFeePerGas` key. ([#23013](https://github.com/ethereum/go-ethereum/pull/23013), [#23039](https://github.com/ethereum/go-ethereum/pull/23039))  
- - The geth `--ethstats` option now supports special characters such as `@` in the node name portion of the URL, specifically so you can put your Twitter handle in there. ([#21640](https://github.com/ethereum/go-ethereum/pull/21640))  
- - JSON-RPC and GraphQL APIs have been extended for EIP-1559, following the [official OpenRPC specification]. ([#22964](https://github.com/ethereum/go-ethereum/pull/22964), [#23010](https://github.com/ethereum/go-ethereum/pull/23010), [#23028](https://github.com/ethereum/go-ethereum/pull/23028), [#23027](https://github.com/ethereum/go-ethereum/pull/23027), [#23050](https://github.com/ethereum/go-ethereum/pull/23050))  
- #### Networking  
- - Snap sync now tracks peer latency/capacity and adjusts request sizes accordingly.  
- - The RLPx wire protocol implementation has been optimized, reducing memory allocations and system call load. ([#22899](https://github.com/ethereum/go-ethereum/pull/22899))  
- - The eth protocol cross-client test suite has been further extended and is more reliable. ([#22843](https://github.com/ethereum/go-ethereum/pull/22843), [#22535](https://github.com/ethereum/go-ethereum/pull/22535), [#22957](https://github.com/ethereum/go-ethereum/pull/22957))  
- - Spurious warning logs about failure to 'unregister' eth peers are gone now. ([#22908](https://github.com/ethereum/go-ethereum/pull/22908))  
- - DNS discovery no longer crashes when geth is started and immediately shut down again. ([#22906](https://github.com/ethereum/go-ethereum/pull/22906))  
- - A long-standing issue in the validation of eth/64 fork IDs is resolved. ([#22879](https://github.com/ethereum/go-ethereum/pull/22879))  
- - The `elliptic.Curve` implementation provided by crypto/secp256k1 is now more correct regarding the point at infinity. This change is not relevant for go-ethereum itself because the affected curve operations are not used, but may be an improvement for alternative/experimental uses of the secp256k1 package. ([#22621](https://github.com/ethereum/go-ethereum/pull/22621))  
- [London specification document]: https://github.com/ethereum/eth1.0-specs/blob/master/network-upgrades/mainnet-upgrades/london.md  
- [official OpenRPC specification]: https://playground.open-rpc.org/?uiSchema%5BappBar%5D%5Bui:splitView%5D=false&schemaUrl=https://raw.githubusercontent.com/ethereum/eth1.0-specs/master/json-rpc/spec.json&uiSchema%5BappBar%5D%5Bui:input%5D=false  
  
## [v1.10.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.3) (2021-05-05)  
  
- #### Networking  
- - Support for eth/64 has been removed. The minimum protocol version is now eth/65 ([#22636](https://github.com/ethereum/go-ethereum/pull/22636))  
- - DNS discovery for the snap protocol now uses the eth protocol node list. The snap-specific node list will be retired later. This is possible because more than 75% of all eth nodes support serving snap ([#22808](https://github.com/ethereum/go-ethereum/pull/22808))  
- - For eth and snap, the protocol handlers now report additional metrics about response latency ([#22608](https://github.com/ethereum/go-ethereum/pull/22608), [#22751](https://github.com/ethereum/go-ethereum/pull/22751), [#22753](https://github.com/ethereum/go-ethereum/pull/22753)). A Grafana dashboard incorporating the new metrics is available [here](https://gist.github.com/karalabe/1e26f9ea5c842fb118584edadc454e18).  
- - Several new tests have been added in the cross-client eth protocol test suite. The tests are now more reliable and run as part of pull request CI on Travis ([#22698](https://github.com/ethereum/go-ethereum/pull/22698), [#22630](https://github.com/ethereum/go-ethereum/pull/22630), [#22757](https://github.com/ethereum/go-ethereum/pull/22757), [#22749](https://github.com/ethereum/go-ethereum/pull/22749), [#22754](https://github.com/ethereum/go-ethereum/pull/22754), [#22801](https://github.com/ethereum/go-ethereum/pull/22801))  
- - The discv5 message handler now reflects IPv4-in-IPv6 addresses correctly when handling PING ([#22703](https://github.com/ethereum/go-ethereum/pull/22703))  
- - The DNS node list tools in cmd/devp2p now support setting a size limit for node lists. This was added because the list of mainnet snap protocol nodes overflowed our AWS Route53 account ([#22694](https://github.com/ethereum/go-ethereum/pull/22694), [#22695](https://github.com/ethereum/go-ethereum/pull/22695))  
  
## [v1.10.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.2) (2021-04-08)  
  
- - `geth dumpgenesis` and the `geth db` commands now support the `--datadir` flag and the network selection flags (`--rinkeby`, etc.) ([#22406](https://github.com/ethereum/go-ethereum/pull/22406), [#22407](https://github.com/ethereum/go-ethereum/pull/22407))  
- - In LES server RPC APIs, the node ID can now be supplied as an `enode://` URL or 32-byte hex ID ([#22423](https://github.com/ethereum/go-ethereum/pull/22423))  
- #### Networking  
- - When metrics are enabled, the 'eth' protocol handler now measures the latency of message handling ([#22581](https://github.com/ethereum/go-ethereum/pull/22581), [#22586](https://github.com/ethereum/go-ethereum/pull/22586))  
- - The DNS discovery client was fixed to avoid hot-spinning when a DNS node trees contains many invalid/incompatible ENRs ([#22566](https://github.com/ethereum/go-ethereum/pull/22566))  
- - The DNS discovery deployer tool has been updated to resolve some long-standing bugs. Publishing of large trees is now much more efficient because needless updates are avoided, and better batching means the deployer waits less for the server. The tool now uses the latest AWS and CloudFlare SDK versions. ([#22572](https://github.com/ethereum/go-ethereum/pull/22572), [#22538](https://github.com/ethereum/go-ethereum/pull/22538), [#22537](https://github.com/ethereum/go-ethereum/pull/22537), [#22588](https://github.com/ethereum/go-ethereum/pull/22588), [#22360](https://github.com/ethereum/go-ethereum/pull/22360))  
- - The branch factor of DNS discovery trees has been reduced to ensure that all TXT records fit into the 512-byte limit of DNS/UDP packets ([#22533](https://github.com/ethereum/go-ethereum/pull/22533))  
- - `devp2p nodeset filter -les-server` was fixed to deal with the new format of the "les" ENR entry ([#22565](https://github.com/ethereum/go-ethereum/pull/22565))  
- - The cross-client test suite of the 'eth' protocol has been improved a bit, and now skips eth/66 tests when the node under test does not support protocol version 66 ([#22460](https://github.com/ethereum/go-ethereum/pull/22460), [#22508](https://github.com/ethereum/go-ethereum/pull/22508), [#22482](https://github.com/ethereum/go-ethereum/pull/22482), [#22474](https://github.com/ethereum/go-ethereum/pull/22474))  
- - LES connection pre-negotiation via UDP now works correctly ([#22451](https://github.com/ethereum/go-ethereum/pull/22451))  
  
## [v1.10.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.1) (2021-03-08)  
  
- Geth v1.10.1 is a minor release with the sole purpose of enabling the [***Berlin hard-fork***](https://github.com/ethereum/eth1.0-specs/blob/master/network-upgrades/berlin.md)! This hard-fork takes a step towards making opcodes fairer and lays the groundwork to new types of transactions, with lots of interesting features to be built on top.  
- The Ethereum Foundation will have a dedicated blog post for Berlin. The essential parts from Geth's perspective is that v1.10.1 is ***required*** for Berlin on all testnets and the mainnet too. Below you can find the fork blocks for the different networks and their expected schedules. Please ensure you are upgraded well in advance of the forks to ensure a smooth transition.    
  
## [v1.10.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.10.0) (2021-03-03)  
  
- This is a new sync mode, which is a replacement for 'fast sync'. In snap sync, the node downloads Ethereum state data much more efficiently than fast sync ever could. With snap sync, we can also finally provide a progress indicator for the state download. Since this is a new feature, and few peers will support snap sync initially, snap sync is not yet enabled by default. We will make it the default in a couple of weeks. ([#21482](https://github.com/ethereum/go-ethereum/pull/21482), [#22171](https://github.com/ethereum/go-ethereum/pull/22171), [#22235](https://github.com/ethereum/go-ethereum/pull/22235), [#22272](https://github.com/ethereum/go-ethereum/pull/22272), [#22334](https://github.com/ethereum/go-ethereum/pull/22334))  
- ### eth/66 protocol  
- Geth now supports eth protocol version 66, which adds request IDs. While the new protocol version is supported on the server side, Geth does not use request IDs yet. ([#22241](https://github.com/ethereum/go-ethereum/pull/22241))  
- We have also added a cross-client test suite for the new protocol version in the `devp2p` tool. ([#22363](https://github.com/ethereum/go-ethereum/pull/22363))  
- ### les/4 protocol  
- Geth 1.10.0 updates the light client protocol to version 4. The new protocol version uses the eth2 Discovery v5 DHT, has better support for servers which can't serve old transactions, and adds support for EIP-2364 ForkID in the handshake. ([#21909](https://github.com/ethereum/go-ethereum/pull/21909), [#22321](https://github.com/ethereum/go-ethereum/pull/22321), [#22357](https://github.com/ethereum/go-ethereum/pull/22357), [#22343](https://github.com/ethereum/go-ethereum/pull/22343), [#22125](https://github.com/ethereum/go-ethereum/pull/22125), [#22347](https://github.com/ethereum/go-ethereum/pull/22347), [#21940](https://github.com/ethereum/go-ethereum/pull/21940), [#22349](https://github.com/ethereum/go-ethereum/pull/22349), [#21930](https://github.com/ethereum/go-ethereum/pull/21930))  
- - Geth will now terminate gracefully when the disk is almost full, to avoid database corruption. ([#22103](https://github.com/ethereum/go-ethereum/pull/22103))  
- We have made several backwards-incompatible changes to GraphQL APIs to better match the specification. In cases where the specification was vague, we have coordinated with the Besu development team to match their implementation.  
- - go-ethereum no longer depends on github.com/aristanetworks/goarista ([#22211](https://github.com/ethereum/go-ethereum/pull/22211))  
- - eth/protocols/snap: speed up hash checks ([#22023](https://github.com/ethereum/go-ethereum/pull/22023))  
- - cmd/devp2p: fix documentation for eth-test ([#22298](https://github.com/ethereum/go-ethereum/pull/22298))  
- - eth/protocols/eth: fix slice resize flaw ([#22181](https://github.com/ethereum/go-ethereum/pull/22181))  
- - eth/protocols/snap: track reverts when peer rejects request ([#22016](https://github.com/ethereum/go-ethereum/pull/22016))  
- - node: always show websocket url in logs ([#22307](https://github.com/ethereum/go-ethereum/pull/22307))  
- - p2p/dnsdisc: fix hot-spin when all trees are empty ([#22313](https://github.com/ethereum/go-ethereum/pull/22313))  
  
## [v1.9.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.25) (2020-12-11)  
  
- - The `devp2p nodeset filter` command can now find snap-enabled nodes ([#21950](https://github.com/ethereum/go-ethereum/pull/21950))  
- - The eth protocol test suite has been extended with tests for transaction announcements and malicious announce behavior ([#21857](https://github.com/ethereum/go-ethereum/pull/21857), [#21792](https://github.com/ethereum/go-ethereum/pull/21792))  
- - We now offer signify/minisign signatures for Geth binary downloads as an alternative to PGP. This is experimental, and not yet advertised on the downloads page ([#21798](https://github.com/ethereum/go-ethereum/pull/21798))  
- - Light client peer discovery now uses DNS ([#21906](https://github.com/ethereum/go-ethereum/pull/21906))  
- - The peer connection acceptor doesn't hot-spin anymore when geth runs out of file descriptors ([#21878](https://github.com/ethereum/go-ethereum/pull/21878))  
- - A rare deadlock in Discovery v5 message dispatch is fixed ([#21858](https://github.com/ethereum/go-ethereum/pull/21858))  
  
## [v1.9.24](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.24) (2020-11-12)  
  
- - Further polishes on the black-box `eth` protocol tester ([#21782](https://github.com/ethereum/go-ethereum/pull/21782)).  
- - Implement TAP output for p2p protocol test suites ([#21760](https://github.com/ethereum/go-ethereum/pull/21760)).  
- - Fix a regression that cause the console to terminate on Ctrl+C ([#21660](https://github.com/ethereum/go-ethereum/pull/21660)).  
- - Fix a peer disconnection issue between unsynced LES servers ([#21761](https://github.com/ethereum/go-ethereum/pull/21761)).  
  
## [v1.9.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.23) (2020-10-15)  
  
- - Peer-to-peer client names are now truncated in logs to prevent log spam ([#21698](https://github.com/ethereum/go-ethereum/pull/21698))  
- - go-ethereum now implements Node Discovery Protocol v5.1 ([#21647](https://github.com/ethereum/go-ethereum/pull/21647))  
- - The cmd/faucet utility now uses DNS discovery to find LES servers ([#21636](https://github.com/ethereum/go-ethereum/pull/21636))  
- - The 'eth' peer-to-peer protocol test suite now works with more client implementations ([#21615](https://github.com/ethereum/go-ethereum/pull/21615))  
- - The Görli testnet bootnode list has been updated ([#21659](https://github.com/ethereum/go-ethereum/pull/21659))  
  
## [v1.9.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.22) (2020-09-28)  
  
- - Start implementing network protocol testers ([#21598](https://github.com/ethereum/go-ethereum/pull/21598), [#21603](https://github.com/ethereum/go-ethereum/pull/21603), [#21604](https://github.com/ethereum/go-ethereum/pull/21604)).  
- - Extract `rlpx` into it's own package for easier protocol tests ([#21464](https://github.com/ethereum/go-ethereum/pull/21464)).  
- - Fix a light client regression that crashed the node after a sync failure ([#21537](https://github.com/ethereum/go-ethereum/pull/21537)).  
  
## [v1.9.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.21) (2020-09-09)  
  
- - Cap the number of in-memory trie nodes during fast sync, fix crasher memory leak ([#21491](https://github.com/ethereum/go-ethereum/pull/21491)).  
  
## [v1.9.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.20) (2020-08-25)  
  
- - Discovery DHT bootstrapping now works properly in very small private networks. ([#21396](https://github.com/ethereum/go-ethereum/pull/21396))  
- - Metrics collection is now lock-free, causing less lock contention among peer connections ([#21446](https://github.com/ethereum/go-ethereum/pull/21446), [#21470](https://github.com/ethereum/go-ethereum/pull/21470))  
- - The GetNodeData operation in the eth peer-to-peer protocol now uses the fast-sync bloom filter  
  
## [v1.9.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.19) (2020-08-11)  
  
- - Swap out the block fetcher of `les/x` to use the same mechanism as `eth/6x` ([#20692](https://github.com/ethereum/go-ethereum/pull/20692)).  
- - Refactor node lifecycle management, merge GraphQL onto HTTP endpoint ([#21105](https://github.com/ethereum/go-ethereum/pull/21105)).  
- - Print `enode` ID of a node too when doing an ENR dump via `devp2p` ([#21270](https://github.com/ethereum/go-ethereum/pull/21270)).  
  
## [v1.9.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.16) (2020-07-10)  
  
- - Protocol message metrics now count the number of messages in addition to bandwidth. ([#21256](https://github.com/ethereum/go-ethereum/pull/21256))  
- - cmd/devp2p: the devp2p tool now contains a test suite for Discovery v4 ([#21163](https://github.com/ethereum/go-ethereum/pull/21163))  
- - cmd/devp2p: the new `devp2p key` command family provides node key management tools ([#21202](https://github.com/ethereum/go-ethereum/pull/21202))  
- - The LES server no longer queries the checkpoint contract for every peer connection, reducing CPU usage. ([#21285](https://github.com/ethereum/go-ethereum/pull/21285))  
- - cmd, node: dump empty value config ([#21296](https://github.com/ethereum/go-ethereum/pull/21296))  
  
## [v1.9.15](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.15) (2020-06-08)  
  
- Geth v1.9.15 is a maintenance release containing bug fixes as well as implementations of all EIPs currently scheduled for the upcoming Berlin fork. A *temporary* test network for these EIPs has also been launched at https://yolonet.xyz/ and can be joined via Geth with `--yolov1` flag.  
- - The LES 'server pool' was rewritten and can now use DNS discovery to find light servers ([#20758](https://github.com/ethereum/go-ethereum/pull/20758)).  
- - Argument checking for natively-implemented console functions is improved ([#21081](https://github.com/ethereum/go-ethereum/pull/21081), [#21160](https://github.com/ethereum/go-ethereum/pull/21160)).  
- - The RPC client now sends WebSocket ping frames when connection is idle ([#21142](https://github.com/ethereum/go-ethereum/pull/21142)).  
  
## [v1.9.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.14) (2020-05-13)  
  
- Ethereum mainnet currently contains over 700M transactions. Each full node maintains a search index, stating that transactions with `hash H` is stored in `block B`. This allows you to look up an arbitrary transaction from the past (at a significant storage cost). But how often do you look up transactions from years ago?  
- Deleting transaction indexes locally is fine since they are not used in consensus nor in synchronization, so network health is unaffected. Light servers for now do need to maintain the full index since light clients rely on them. Huge props to @rjl493456442 and @holiman for this work ([#20302](https://github.com/ethereum/go-ethereum/pull/20302)).  
- - Make light client chain selection logic similar to full nodes on Clique PoA ([#20931](https://github.com/ethereum/go-ethereum/pull/20931)).  
- - Remove legacy v5 discovery bootnodes used by LES ([#20949](https://github.com/ethereum/go-ethereum/pull/20949)).  
- - Fix the TCP P2P dialer to reject enode IDs that advertise TCP port 0 ([#21008](https://github.com/ethereum/go-ethereum/pull/21008)).  
- - Fix a data race in the snapshot iteration (upcoming protocol) ([#20948](https://github.com/ethereum/go-ethereum/pull/20948)).  
- - Fix a freezer termination annoyance that printed warnings ([#21010](https://github.com/ethereum/go-ethereum/pull/21010)).  
- - Fix accidental snapshot creation on archive nodes ([#21025](https://github.com/ethereum/go-ethereum/pull/21025)).  
  
## [v1.9.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.13) (2020-04-16)  
  
- - Change DNS discovery record TTLs to saner values ([#20801](https://github.com/ethereum/go-ethereum/pull/20801), [#20819](https://github.com/ethereum/go-ethereum/pull/20819), [#20820](https://github.com/ethereum/go-ethereum/pull/20820)).  
- - General cleanups in the `ecies` crypto package used by `RLPx`([#20836](https://github.com/ethereum/go-ethereum/pull/20836)).  
- - Improve node shutdown, avoiding a crash in fast sync ([#20695](https://github.com/ethereum/go-ethereum/pull/20695)).  
  
## [v1.9.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.11) (2020-02-18)  
  
-  * [DNS-based peer discovery](https://eips.ethereum.org/EIPS/eip-1459) is now enabled in Geth ([#20592](https://github.com/ethereum/go-ethereum/pull/20592), [#20660](https://github.com/ethereum/go-ethereum/pull/20660)). From now on Geth nodes have two independent mechanisms to find peers. The DNS lists serve as a fallback mechanism when peers cannot be found through the DHT.   
-    DNS-based discovery is a centralized mechanism, but we have tried to make the operation of this mechanism as transparent and permissionless as possible. The public lists used by default are generated by crawling the discovery DHT. At this time, there are ~1150 publicly routed Ethereum mainnet nodes in the default list. Our public lists also serve the Ropsten, Goerli and Rinkeby test networks. Nodes running any Ethereum client which implements [EIP-868](https://eips.ethereum.org/EIPS/eip-868) and [EIP-2124](https://eips.ethereum.org/EIPS/eip-2124) will appear in the public lists automatically.  
-    You can disable the use of DNS-based discovery using the `--discovery.dns ""` flag combination.   
-    If you want to create a DNS-based node list for your private or public network, please check out our [DNS Discovery Setup guide](https://geth.ethereum.org/docs/developers/dns-discovery-setup). We hope that organizations other than Ethereum Foundation will provide public lists in the future and will happily integrate those lists into the default one using the 'link' feature of EIP-1459.  
-  * Transaction announcements via `eth/65` ([EIP 2464](https://eips.ethereum.org/EIPS/eip-2464)) is now implemented and Geth<->Geth connections should use significantly less bandwidth ([#20234](https://github.com/ethereum/go-ethereum/pull/20234)) for exchanging transactions. Final numbers are anybody's guess though, as we need to wait for widespread network deployment. This feature depends on the [`eth/64`](https://eips.ethereum.org/EIPS/eip-2364) and [`eth/65`](https://eips.ethereum.org/EIPS/eip-2464) protocol updates, which are not yet supported in all Ethereum client implementations. While connections between compatible clients will use the new protocol, geth will remain compatible with `eth/63` until the new protocol versions are sufficiently adopted by the public network.  
-  * Add `geth dumpgenesis` to print the full genesis and chain config of a node ([#20191](https://github.com/ethereum/go-ethereum/pull/20191)).  
-  * Fix an RPC connectivity issue around flaky connections ([#20414](https://github.com/ethereum/go-ethereum/pull/20414)).  
-  * Clean up C++ mainnet and Geth Görli bootnodes ([#20610](https://github.com/ethereum/go-ethereum/pull/20610)).  
  
## [v1.9.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.10) (2020-01-20)  
  
- - Integrate DNS discovery, don't activate yet ([#20437](https://github.com/ethereum/go-ethereum/pull/20437), [#20524](https://github.com/ethereum/go-ethereum/pull/20524)).  
- - Fix discovery bootnode-fallback on blocked UDP ([#20573](https://github.com/ethereum/go-ethereum/pull/20573)).  
  
## [v1.9.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.9) (2019-12-06)  
  
- - Fixes a data race in the new iterator based discovery code ([#20421](https://github.com/ethereum/go-ethereum/pull/20421)).  
  
## [v1.9.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.8) (2019-11-26)  
  
- *Note: [Go modules](https://blog.golang.org/using-go-modules) were introduced in Go 1.11 and greatly polished throughout Go 1.13. Although Geth builds fine with Go 1.11 and Go 1.12 too, we recommend running at least Go 1.13 as it is more flexible in handling various circumstances. If you are using `go-ethereum` as a library, converting your project to Go modules via `go mod init` will make dependency management a lot less of a hassle than the vendor approach. Be aware, however, that Go modules require network access if the dependencies aren't yet cached locally.*  
- - Add `clique_status` API to quickly glance the health of Clique networks ([#20103](https://github.com/ethereum/go-ethereum/pull/20103)).  
  
## [v1.9.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.7) (2019-11-07)  
  
- **This release is a bit more special, however, as it finally [initializes](https://github.com/ethereum/go-ethereum/pull/20222) the Ethereum mainnet Istanbul fork block 9069000, targeted to arrive around the 4th of December! Please update your mainnet Geth nodes to v1.9.7 (or newer) as soon as possible to avoid surprises!**  
- - Enforce proper fork orders to avoid partially initialized private networks ([#20169](https://github.com/ethereum/go-ethereum/pull/20169)).  
- - Deploy `eth/64`, extending the protocol handshake with a `forkid` field ([#20140](https://github.com/ethereum/go-ethereum/pull/20140)).  
- - Integrate Istanbul configuration support into `puppeth` for private networks ([#19926](https://github.com/ethereum/go-ethereum/pull/19926)).  
- - Continue work towards the DNS and ENR based secondary discovery protocol ([#20168](https://github.com/ethereum/go-ethereum/pull/20168), [#20132](https://github.com/ethereum/go-ethereum/pull/20132)).  
  
## [v1.9.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.6) (2019-10-03)  
  
- - LES servers now advertise the les capability via ENR ([#20145](https://github.com/ethereum/go-ethereum/pull/20145))  
- - New metrics for p2p bandwidth per protocol message ([#20133](https://github.com/ethereum/go-ethereum/pull/20133))  
  
## [v1.9.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.4) (2019-09-19)  
  
- Geth v1.9.4 is a special maintenance release, that besides fixing issues as usual, also [locks in](https://github.com/ethereum/go-ethereum/pull/20090) the Istanbul hard fork block numbers for the Ropsten, Rinkeby and Görli test networks:  
- **Please update your nodes on the above test networks before the deadlines, otherwise you will end up either on a wrong chain (Ropsten), or stuck altogether (Rinkeby and Görli). The `--override.istanbul` CLI flag can be used to bail out of the fork even last minute if things go sour.**  
- - Fix a USB HID discovery regression on Windows ([#20092](https://github.com/ethereum/go-ethereum/pull/20092)).  
- - Fix some RLP decoding issues around `nil`s for discv5 ([#20064](https://github.com/ethereum/go-ethereum/pull/20064)).  
- - Fix P2P metric types so they work properly with Influx/Grafana ([#20047](https://github.com/ethereum/go-ethereum/pull/20047)).  
  
## [v1.9.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.9.3) (2019-09-03)  
  
- - Support DNS names in enode URLs during `admin.addPeer` ([#18524](https://github.com/ethereum/go-ethereum/pull/18524)).  
- - Reduce light client default peer count from 100 to 10 ([#19933](https://github.com/ethereum/go-ethereum/pull/19933)).  
- - Update README with the latest fork configs for private networks ([#19983](https://github.com/ethereum/go-ethereum/pull/19983), [#20002](https://github.com/ethereum/go-ethereum/pull/20002)).  
- - Separate light client and light server P2P handlers internally ([#19639](https://github.com/ethereum/go-ethereum/pull/19639), [#20010](https://github.com/ethereum/go-ethereum/pull/20010)).  
  
## [v1.8.27](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.27) (2019-04-17)  
  
- Geth v1.8.27 is a hotfix release to counter an active attack on mainnet, mounted against newly joining Geth nodes doing fast sync ([#19473](https://github.com/ethereum/go-ethereum/pull/19473)).  
-  * If you are already in sync with the network, you **don't** need to update.  
-  * Archive nodes, light clients and full-sync full nodes **don't** need to update.  
-  * If you are joining the network with a new node, then you **do need** to update.  
- Fast sync is susceptible to a grieving version of an eclipse attack, where a malicious remote node attempts to get a new Geth node to fast sync to some small chain, before a real heavy chain is discovered in the network. This results in Geth falling back to full sync for the main chain, taking too much time.  
- This attack can only be meaningfully mounted against nodes which are properly exposed on a public IP address (i.e. not firewalled, not NATed). Even then, it's a race against the node finding good peers fast enough, in which case the attack doesn't work any more.  
- There is no economic advantage in pulling this attack off, only causing sync annoyance. That said, there is currently a number of (at least 4 identified, maybe more) Parity nodes at `207.148.5.229`, which are doing variations of this attack. It might be deliberate, or it might also be leftover nodes from some experiment that only have a few blocks on their chain and subsequently disabled sync. A bit unprobable.  
- This release repurposes the DAO challenge to do a checkpoint challenge based on the recently hard coded CHTs. It also makes the challenge stricter, in that while the node is doing a fast-sync, remote peers are not permitted to be synced below the checkpoint block. This should also help sync the chain faster, getting rid of stalling or useless peers.  
  
## [v1.8.25](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.25) (2019-04-09)  
  
-  * Updated light client CHTs for quicker sync times on all built-in networks.  
-  * Fixes a couple of networking issues in the DHT and `eth`.  
  
## [v1.8.24](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.24) (2019-04-08)  
  
-  * Updated light client CHTs for quicker sync times on all built-in networks.  
-  * Fixes a couple of networking issues in the DHT and `eth`.  
  
## [v1.8.23](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.23) (2019-02-20)  
  
- The release also back-ports native support for the Görli testnet via the `--goerli` flag (https://github.com/ethereum/go-ethereum/pull/19029/commits/2072c26a96badbe45d6df56a4cd68ffd1b6fb12e).  
- - Improved peer suggestion engine ([#18404](https://github.com/ethereum/go-ethereum/pull/18404)).  
- - Bootnode mode and new bootnodes ([#18498](https://github.com/ethereum/go-ethereum/pull/18498), [#18509](https://github.com/ethereum/go-ethereum/pull/18509)).  
- - Network snapshot generator ([#18453](https://github.com/ethereum/go-ethereum/pull/18453)).  
- - Saturation check for healthy networks ([#19071](https://github.com/ethereum/go-ethereum/pull/19071)).  
- - Added debugging APIs to make it easier to explore chunks on nodes ([#18980](https://github.com/ethereum/go-ethereum/pull/18980), [#19002](https://github.com/ethereum/go-ethereum/pull/19002), [#19008](https://github.com/ethereum/go-ethereum/pull/19008)).  
  
## [v1.8.22](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.22) (2019-01-31)  
  
- to disable EIP-1283. This procedure is meant to ease the transition on networks like Ropsten where  
- On the main network, Constantinople and Petersburg activate at the same time.  
- configured otherwise. **If you are running a private network, you must schedule the Petersburg   
- activated on your network.**  
- In addition to the consensus changes, this release contains peer-to-peer networking security  
- improvements in the peer discovery subsystem.  
- - p2p/discover, p2p/enode: rework endpoint proof handling, packet logging (#18963)  
- - p2p/discover: improve table addition code (#18974)  
  
## [v1.8.21](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.21) (2019-01-15)  
  
-  * Fix a discovery protocol error when looking up remote peers ([#18309](https://github.com/ethereum/go-ethereum/pull/18309)).  
  
## [v1.8.20](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.20) (2018-12-11)  
  
-  * Enforce lowercase network names on Puppeth private networks ([#18235](https://github.com/ethereum/go-ethereum/pull/18235)).  
-  * P2P simulations snapshot improvements ([18220](https://github.com/ethereum/go-ethereum/pull/18220)).  
  
## [v1.8.19](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.19) (2018-11-28)  
  
-  * Rework downloader common ancestor lookup, reducing network bandwidth ([#18085](https://github.com/ethereum/go-ethereum/pull/18085)).  
-  * Improve the logs when the node encounters a bad block ([#18156](https://github.com/ethereum/go-ethereum/pull/18156)).  
-  * Fix a light client issue that stored invalid servers in its node database ([#18093](https://github.com/ethereum/go-ethereum/pull/18093)).  
-  * Fix Kademlia neighborhood depth in network package ([18066](https://github.com/ethereum/go-ethereum/pull/18066)).  
  
## [v1.8.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.18) (2018-11-14)  
  
-  * Start generating digitally signed Ethereum node records ([#17753](https://github.com/ethereum/go-ethereum/pull/17753)).  
-  * Fix bootnode enode output on `-writeaddress` ([#17932](https://github.com/ethereum/go-ethereum/pull/17932)).  
-  * Fix a deadlock in the p2p simulator framework ([#17891](https://github.com/ethereum/go-ethereum/pull/17891)).  
-  * Swarm light node mode: disable sync, retrieve, subscription messages ([17899](https://github.com/ethereum/go-ethereum/pull/17899)).  
-  * Bugfix: Swarm Feed update with empty signature returns HTTP 200 ([18008](https://github.com/ethereum/go-ethereum/pull/18008)).  
-  * Initial P2P accounting for message exchange ([17951](https://github.com/ethereum/go-ethereum/pull/17951)).  
- *Note that a bug was fixed within the local Swarm database implementation, which requires a cleanup procedure to be run upon Swarm startup. This means that your Swarm node might take a while to start once you upgrade to this version.*  
  
## [v1.8.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.17) (2018-10-09)  
  
-  * Expose the `enode` URL of a peer through the `admin.peers` endpoint ([#17838](https://github.com/ethereum/go-ethereum/pull/17838)).  
-  * Implement a lower bound on block propagation targets to 4 peers ([#17725](https://github.com/ethereum/go-ethereum/pull/17725)).  
-  * Fix flag parsing to allow archive nodes serving light clients ([#17803](https://github.com/ethereum/go-ethereum/pull/17803)).  
-  * Fix a `puppeth` regression causing invalid enode errors ([#17802](https://github.com/ethereum/go-ethereum/pull/17802)).  
-  * Add stream peer servers limit ([17747](https://github.com/ethereum/go-ethereum/pull/17747)).  
  
## [v1.8.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.16) (2018-09-24)  
  
-  * Refactored and simplified Kademlia code ([17641](https://github.com/ethereum/go-ethereum/pull/17641)).  
  
## [v1.8.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.14) (2018-08-22)  
  
- * Support trusted peer management (addition/removal) via RPC API function calls ([#16333](https://github.com/ethereum/go-ethereum/pull/16333)).  
- * Support specifying pubkey identity files into puppeth connection strings ([#17407](https://github.com/ethereum/go-ethereum/pull/17407)).  
-  * Updated Swarm bootnodes ([#17414](https://github.com/ethereum/go-ethereum/pull/17414)).  
  
## [v1.8.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.13) (2018-07-31)  
  
-  * Fix a puppeth node id retrieval issue on <2GB RAM machines ([#17281](https://github.com/ethereum/go-ethereum/pull/17281)).  
-  * Client-side MRU signatures: create and update mutable resources through any node, including Web3 support (Mist, Metamask, etc) ([#17231](https://github.com/ethereum/go-ethereum/pull/17231)).  
-  * Refactored network simulation framework (https://github.com/ethereum/go-ethereum/pull/17241).  
  
## [v1.8.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.12) (2018-07-05)  
  
- Networking changes:  
- - Discovery ping/pong logic uses less goroutines ([#17048](https://github.com/ethereum/go-ethereum/pull/17048))  
- - Network ID is set correctly when -dev is set ([#16833](https://github.com/ethereum/go-ethereum/pull/16833))  
  
## [v1.8.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.11) (2018-06-12)  
  
-  * Reduce full-node database rate of growth by 28% ([#16810](https://github.com/ethereum/go-ethereum/pull/16810)).  
-  * Enforce RPC timeouts to avoid stalling HTTP connections ([#16880](https://github.com/ethereum/go-ethereum/pull/16880)).  
  
## [v1.8.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.10) (2018-05-30)  
  
-  * Fix an issue where slow peers could cause goroutine leaks and out-of-memory crashes ([#16769](https://github.com/ethereum/go-ethereum/pull/16769)).  
-  * Ensure Ethereum Node Records are compatible with discovery v4 ([#16679](https://github.com/ethereum/go-ethereum/pull/16679)).  
  
## [v1.8.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.9) (2018-05-28)  
  
-  * Fix an issue where slow peers could cause goroutine leaks and out-of-memory crashes ([#16769](https://github.com/ethereum/go-ethereum/pull/16769)).  
-  * Ensure Ethereum Node Records are compatible with discovery v4 ([#16679](https://github.com/ethereum/go-ethereum/pull/16679)).  
  
## [v1.8.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.8) (2018-05-14)  
  
-  * Fix crash when using an invalid private network genesis ([#16682](https://github.com/ethereum/go-ethereum/pull/16682)).  
  
## [v1.8.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.7) (2018-05-02)  
  
- Geth Titanium (v1.8.7) is a hotfix release to address a leveldb crash affecting archive nodes.  
  
## [v1.8.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.6) (2018-04-23)  
  
-  * Fix a downloader deadlock which froze sync on master peer drop ([#16546](https://github.com/ethereum/go-ethereum/pull/16546)).  
- The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**  
  
## [v1.8.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.5) (2018-04-23)  
  
-  * Fix a downloader deadlock which froze sync on master peer drop ([#16546](https://github.com/ethereum/go-ethereum/pull/16546)).  
- The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**  
  
## [v1.8.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.4) (2018-04-17)  
  
- The v1.8.4 release also contains the first pre-alpha incarnation of Clef, our standalone signer ([#16154](https://github.com/ethereum/go-ethereum/pull/16154)). **It is a work in progress developer preview. Please consider it unsafe until otherwise announced.**  
  
## [v1.8.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.3) (2018-03-27)  
  
- - The 128kB size limit for RPC requests is now also applied to WebSocket connections and HTTP requests using chunked transfer encoding  
  
## [v1.8.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.2) (2018-03-05)  
  
-  * Support gracefully terminating on SIGTERM too, not just SIGINT (https://github.com/ethereum/go-ethereum/pull/16142).  
-  * Flush in-memory tries more flexibly on graceful termination (https://github.com/ethereum/go-ethereum/pull/16143).  
-  * Track number of downloaded state trie nodes across restarts (https://github.com/ethereum/go-ethereum/pull/16224).  
-  * Disallow hyphens in Puppeth network names (https://github.com/ethereum/go-ethereum/pull/16157).  
-  * Allow exceeding max peers for trusted peers (https://github.com/ethereum/go-ethereum/pull/16189).  
  
## [v1.8.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.1) (2018-02-19)  
  
-  * Fix a discovery issue that incorrectly measured peer bond time ([16109](https://github.com/ethereum/go-ethereum/pull/16109)).  
-   - `geth attach` accepts `--datadir`, `--rinkeby`, `--testnet` as an alternative to supplying the IPC path ([#15597](https://github.com/ethereum/go-ethereum/pull/15597), [#15686](https://github.com/ethereum/go-ethereum/pull/15686), [#15517](https://github.com/ethereum/go-ethereum/pull/15517))  
- ### ETH Protocol  
- Note: State pruning is enabled on all `--syncmode` variations (including `--syncmode=full`). If you are running an archive node where you would like to retain all historical data, disable pruning via `--gcmode=archive`.  
- Tracing and pruning: By default, state for the last 128 blocks kept in memory. Most states are garbage collected. If you are running a block explorer or other service relying on transaction tracing without an archive node (`--gcmode=archive`), you need to trace within this window! Alternatively, specify the `"reexec"` tracer option to allow regenerating historical state; and ideally switch to chain tracing which amortizes overhead across all traced blocks.  
-   - The new `personal_signTransaction` method signs a transaction without sending it to the network ([#15971](https://github.com/ethereum/go-ethereum/pull/15971))  
-   - `admin_nodeInfo` returns the chain configuration in light client mode ([#15732](https://github.com/ethereum/go-ethereum/pull/15732))  
- Note: the experimental peer discovery v5 protocol has changed, breaking backwards compatibility with previous releases. Geth 1.8 will not discover LES servers running geth 1.7.  
- The peer discovery protocols now share the same UDP port (30303 by default). If you are doing manual peer management and using the light client, you may need to ensure your v1.8 clients are pointed to port 30303 and not 30304 as previously.  
-   - Light client peer count is now properly limited ([#16010](https://github.com/ethereum/go-ethereum/pull/16010))  
-   - Whisper protocol v6 as described in [EIP-627](https://github.com/ethereum/EIPs/pull/627) is implemented ([#15666](https://github.com/ethereum/go-ethereum/pull/15666), [#15701](https://github.com/ethereum/go-ethereum/pull/15701), [#15802](https://github.com/ethereum/go-ethereum/pull/15802), [#15578](https://github.com/ethereum/go-ethereum/pull/15578), [#15631](https://github.com/ethereum/go-ethereum/pull/15631), [#16046](https://github.com/ethereum/go-ethereum/pull/16046))  
-   - The `wnode` command speaks Whisper v6 ([#16051](https://github.com/ethereum/go-ethereum/pull/16051))  
- ### Networking  
-   - The peer discovery protocol removes dead nodes more aggressively ([#16069](https://github.com/ethereum/go-ethereum/pull/16069))  
-   - Discovery v4 and the experimental discovery v5 protocol now run on the same UDP port ([#15200](https://github.com/ethereum/go-ethereum/pull/15200))  
-   - Several issues in the experimental discovery v5 protocol are resolved ([#15995](https://github.com/ethereum/go-ethereum/pull/15995), [#16036](https://github.com/ethereum/go-ethereum/pull/16036))  
-   - We have added more bootstrap nodes for the ropsten testnet, including a few parity nodes ([#16008](https://github.com/ethereum/go-ethereum/pull/16008), [#16029](https://github.com/ethereum/go-ethereum/pull/16029))  
-   - The crypto package now supports public key compression and signature verification ([#15615](https://github.com/ethereum/go-ethereum/pull/15615), [#15626](https://github.com/ethereum/go-ethereum/pull/15626))  
-   - The new p2p/enr package implements [EIP-778](https://github.com/ethereum/EIPs/pull/778) ([#15585](https://github.com/ethereum/go-ethereum/pull/15585))  
  
## [v1.8.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.8.0) (2018-02-14)  
  
-   - `geth attach` accepts `--datadir`, `--rinkeby`, `--testnet` as an alternative to supplying the IPC path ([#15597](https://github.com/ethereum/go-ethereum/pull/15597), [#15686](https://github.com/ethereum/go-ethereum/pull/15686), [#15517](https://github.com/ethereum/go-ethereum/pull/15517))  
- ### ETH Protocol  
- Note: State pruning is enabled on all `--syncmode` variations (including `--syncmode=full`). If you are running an archive node where you would like to retain all historical data, disable pruning via `--gcmode=archive`.  
- Tracing and pruning: By default, state for the last 128 blocks kept in memory. Most states are garbage collected. If you are running a block explorer or other service relying on transaction tracing without an archive node (`--gcmode=archive`), you need to trace within this window! Alternatively, specify the `"reexec"` tracer option to allow regenerating historical state; and ideally switch to chain tracing which amortizes overhead across all traced blocks.  
-   - The new `personal_signTransaction` method signs a transaction without sending it to the network ([#15971](https://github.com/ethereum/go-ethereum/pull/15971))  
-   - `admin_nodeInfo` returns the chain configuration in light client mode ([#15732](https://github.com/ethereum/go-ethereum/pull/15732))  
- Note: the experimental peer discovery v5 protocol has changed, breaking backwards compatibility with previous releases. Geth 1.8 will not discover LES servers running geth 1.7.  
- The peer discovery protocols now share the same UDP port (30303 by default). If you are doing manual peer management and using the light client, you may need to ensure your v1.8 clients are pointed to port 30303 and not 30304 as previously.  
-   - Light client peer count is now properly limited ([#16010](https://github.com/ethereum/go-ethereum/pull/16010))  
-   - Whisper protocol v6 as described in [EIP-627](https://github.com/ethereum/EIPs/pull/627) is implemented ([#15666](https://github.com/ethereum/go-ethereum/pull/15666), [#15701](https://github.com/ethereum/go-ethereum/pull/15701), [#15802](https://github.com/ethereum/go-ethereum/pull/15802), [#15578](https://github.com/ethereum/go-ethereum/pull/15578), [#15631](https://github.com/ethereum/go-ethereum/pull/15631), [#16046](https://github.com/ethereum/go-ethereum/pull/16046))  
-   - The `wnode` command speaks Whisper v6 ([#16051](https://github.com/ethereum/go-ethereum/pull/16051))  
- ### Networking  
-   - The peer discovery protocol removes dead nodes more aggressively ([#16069](https://github.com/ethereum/go-ethereum/pull/16069))  
-   - Discovery v4 and the experimental discovery v5 protocol now run on the same UDP port ([#15200](https://github.com/ethereum/go-ethereum/pull/15200))  
-   - Several issues in the experimental discovery v5 protocol are resolved ([#15995](https://github.com/ethereum/go-ethereum/pull/15995), [#16036](https://github.com/ethereum/go-ethereum/pull/16036))  
-   - We have added more bootstrap nodes for the ropsten testnet, including a few parity nodes ([#16008](https://github.com/ethereum/go-ethereum/pull/16008), [#16029](https://github.com/ethereum/go-ethereum/pull/16029))  
-   - The crypto package now supports public key compression and signature verification ([#15615](https://github.com/ethereum/go-ethereum/pull/15615), [#15626](https://github.com/ethereum/go-ethereum/pull/15626))  
-   - The new p2p/enr package implements [EIP-778](https://github.com/ethereum/EIPs/pull/778) ([#15585](https://github.com/ethereum/go-ethereum/pull/15585))  
  
## [v1.7.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.3) (2017-11-21)  
  
-  * Roll out v2 of the `les` light client protocol (https://github.com/ethereum/go-ethereum/pull/14970, https://github.com/ethereum/go-ethereum/pull/15367, https://github.com/ethereum/go-ethereum/pull/15391).  
-  * Improve EVM jump destination analysis for worst case scenarios (https://github.com/ethereum/go-ethereum/pull/14582).  
-  * Private network faucet support Facebook, Twitter and Google+ authentication (https://github.com/ethereum/go-ethereum/pull/15313).  
-  * Support encrypted SSH keys in the Puppeth network manager (https://github.com/ethereum/go-ethereum/pull/15443).  
  
## [v1.7.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.1) (2017-10-04)  
  
- **This release enables the Byzantium hard fork transition at block number 4370000 (~17th October) on the mainnet and block number 1035301 (~9th October) on the Rinkeby test network. Please update well before these dates to ensure a smooth transition.**  
- * p2p: messages are compressed using Snappy. See [EIP706](https://github.com/ethereum/EIPs/pull/706) for more information. ([#15106](https://github.com/ethereum/go-ethereum/issues/15106))  
  
## [v1.7.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.7.0) (2017-09-14)  
  
- The Go Ethereum team is proud to announce the next release family of Geth, the first incarnation of which focuses on laying the groundwork for the upcoming Metropolis hard forks (Byzantium and Constantinople) and consists of [125+ code contributions](https://github.com/ethereum/go-ethereum/milestone/47?closed=1) to various parts of the project.  
- The current incarnation of Geth contains all the Byzantium EIPs implemented and also features the fork block number **1,700,000** for the Ropsten testnet transition. The block numbers for Rinkeby and the main Ethereum network will be finalized when Ropsten is deemed stable.  
- You can find details about individual protocol updates at the following locations:  
- * Transaction and receipt storage was completely reworked, cutting the data storage requirements of a fast synced node in half, from 26.3GB to 14.9GB at the time of the implementation ([#14801](https://github.com/ethereum/go-ethereum/pull/14801)).  
- * Upgrading the base peer-to-peer protocol used by all Ethereum sub-protocols, cutting the bandwidth needed for a fast sync from 33.6GB to 13.5GB ([#15106](https://github.com/ethereum/go-ethereum/pull/15106)). This upgrade will improve the general bandwidth requirement of the network as well as light clients too.  
- The new pool features a special exemption mechanism for local accounts so that a user's own transactions are always prioritized over remote ones, even if they are under-priced compared to everyone else's. This ensures that cheap transactions don't get flushed out of the network during heavy usage (e.g. ICO) as long as the originating node remains online.  
- Geth 1.7.0 takes this protective measure a step forward by journaling all locally created transactions to disk, and loading them back up on a node restart. This ensures that even if the originating node goes offline, cheap transactions still have a chance to be included when the node comes back ([#14784](https://github.com/ethereum/go-ethereum/pull/14784)).  
- The transaction journal can be an enormous help for node operators during software upgrades by not having to worry about local transactions going missing. Furthermore, the journal also acts as a resiliency mechanism against node crashes, ensuring that no transaction data is lost.  
- Lastly we're extremely happy to announce that [Infura became an active player](https://blog.infura.io/infuras-signer-and-bootnode-on-rinkeby-440de6f70961) in the Rinkeby test network by aiding the community both with their own bootnode as well as running an authorized signer node. This should make the Rinkeby network even more robust and resilient.  
  
## [v1.6.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.7) (2017-07-12)  
  
- **Important notice to projects running public nodes (MyEtherWallet, Etherscan, Infura, etc): The sender address of any transaction submitted via RPC will be considered a local address, and all transactions from such addresses will be exempt of the gas price and pool size enforcement. To avoid DOS attacks due to the exemption algorithm, please run with `--txpool.nolocals` to disable special exemptions for local accounts.**  
  
## [v1.6.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.4) (2017-06-01)  
  
-  * Reduce egress network traffic sent to ethstats to avoid overloads (https://github.com/ethereum/go-ethereum/pull/14563).  
  
## [v1.6.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.3) (2017-06-01)  
  
-  * Reduce egress network traffic sent to ethstats to avoid overloads (https://github.com/ethereum/go-ethereum/pull/14563).  
  
## [v1.6.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.1) (2017-05-04)  
  
- Highlights of the release are the capability to iterate over a contract's storage entries via the RPC; transitioning [Whisper to protocol v5](https://github.com/ethereum/go-ethereum/wiki/Whisper) along with bundled diagnostics tools; and shorthand connectivity to the Rinkeby test network.  
-  * Switch Whisper to protocol v5 (https://github.com/ethereum/go-ethereum/pull/14387), ship diagnostic tool (https://github.com/ethereum/go-ethereum/pull/14414).  
-  * When generating bootnode keys, print and exit, don't start up node (https://github.com/ethereum/go-ethereum/pull/14372).  
-  * Make network IDs uniform uint64 everywhere (https://github.com/ethereum/go-ethereum/pull/14377).  
-  * Fix data race in during native dapp node restarts (https://github.com/ethereum/go-ethereum/pull/14379).  
-  * Fix `clique` miner ugly error log when starting a new network (https://github.com/ethereum/go-ethereum/pull/14411).  
  
## [v1.6.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.6.0) (2017-04-14)  
  
- * The 'clique' Proof of Authority consensus engine can be used for private networks (#3753).  
- * The new `puppeth` command can be used to deploy private networks (#13854).  
- * For `geth --dev` chains, Ethereum protocol upgrades (Homestead, etc.) are enabled at block 0 (#3697).  
- * The current head is announced to peers when sync completes (#13883).  
- * Transactions are accepted as soon as CPU mining starts. This fixes transaction inclusion issues for small private networks (#13882).  
- * Discovery bootstrap nodes are used as a last-resort dial target (#13874).  
- * The RPC server delivers all pending responses before closing the connection (#3814).  
  
## [v1.5.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.9) (2017-02-13)  
  
- - `Browser support` needs to be turned off from the Ethereum app `Settings` (different protocol).  
  
## [v1.5.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.8) (2017-02-01)  
  
- - A new developer tool, `cmd/wnode`, allows testing the Whisper v5 protocol. (#3580)  
  
## [v1.5.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.6) (2017-01-09)  
  
- - Go functions dealing with signatures now expect a V value of 0 or 1. `crypto.SignEthereum` and related APIs have been removed. You can convert the signature to ethereum format with a V value of 27 or 28 using  `sig[64] += 27`. #3455   
  
## [v1.5.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.5) (2016-12-14)  
  
- The brave soul/sole feature of the release is support for exporting the blockchain to- and importing it from gzipped data streams (#3427) too. This can be useful for private network development purposes to back up and restore snapshots of the chain and for debugging/testing purposes. You can do compressed export/import operation simply via specifying a chain output file name ending in `.gz`.  
- The built in netstats client was fixed to report a few infos that were not sent to the netstats server in the previous release due to an oversight (#3370, #3373, #3390). This should help sort out the issues seen on the netstats page that certain charts had missing data in them. Further it adds support for historical data queries that are relevant mostly for private netstats servers with only Geth nodes reporting (#3425).  
  
## [v1.5.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.4) (2016-11-28)  
  
- Geth now includes a built-in netstats reporter. Use `--ethstats "<nodename>:<secret>@ethstats.net"` to get listed on ethstats.net. (#3336) **Assignment**: find secret Skype room with ethstats password :wink:   
- Network communication can now be restricted to a list of IP subnetworks. This feature is intended for private chains (and meetups!). For example, `geth --netrestrict 192.168.0.0/16` only allows connections in the commonly used LAN range. (#3325)   
  
## [v1.5.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.5.3) (2016-11-24)  
  
- With 1.5.3 `--testnet` now selects the Ropsten network. If you have a blockchain database  
- for the Morden network, run `geth --testnet removedb` to remove it.  
  
## [v1.4.18](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.18) (2016-10-15)  
  
- **Note for users running geth on the Ethereum Classic network (--oppose-dao-fork):**  
  
## [v1.4.17](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.17) (2016-10-10)  
  
- This release limits the number of transactions per-user and globally so as to limit maximum memory consumption and egress network traffic.  
  
## [v1.4.16](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.16) (2016-10-06)  
  
- The DoS issue that began affecting the network two days ago has been remediated by adding state journalling to Geth. Journalling means that we no longer need to copy a transaction's state when a call is made; this is the root cause of many of the recently identified issues. Journalling, along with other recent improvements in response to other attacks, should also make Geth significantly faster at importing transactions.  
  
## [v1.4.14](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.14) (2016-09-28)  
  
- This is a Geth pre-release to counter the network DoS attacks from yesterday and those detected today. It is a prerelease (to help people get back on track fast) with further updates coming soon. As always, please keep an eye on your nodes while using as it features a new bleeding edge caching mechanism.  
- _Disclaimer: All of these binaries have been cross-compiled from Linux. Their primary goal is to provide access to unsupported or experimental platforms. We cannot guarantee that a cross compiler will produce the same performing code as a native build will. For any issues found, please contact [@karalabe](https://github.com/karalabe)._  
  
## [v1.4.13](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.13) (2016-09-26)  
  
- This patch is a hotfix release to mitigate last weeks DoS attacks, and those ongoing currently against Geth nodes in the network.  
- This patch should still be considered bleeding-edge. We advise exchanges and other users running business-critical applications that intend to run this release to simultaneously run a different client node and trigger a circuit breaker in case there is another DoS vulnerability or consensus failure. Users should continue to be on the alert for new patches and attacks against the network. Once enough nodes run the patch and the situation sufficiently stabilizes we will advise miners to slowly raise the gas limit.  
  
## [v1.4.12](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.12) (2016-09-19)  
  
- Hotfix release for a network DOS attack! Please update ASAP.  
  
## [v1.4.11](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.11) (2016-08-18)  
  
- - Cancel DAO challenge on peer drop #2833   
- - Removed eth/61 protocol #2842  
  
## [v1.4.10](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.10) (2016-07-16)  
  
- On startup (on the main network) Geth will print the currently configured choice, which you can freely change with the appropriate flag at any time. If neither of the above flags is specified, the previous configuration is used. **If no fork choice was ever provided, Geth will default to supporting the fork [per the majority vote](http://carbonvote.com/).**  
  
## [v1.4.9](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.9) (2016-06-29)  
  
- Geth 1.4.9 is a reversal release to undo the code changes that went into the 1.4.8 "DAO Wars" soft-fork release, as the soft-fork was deemed too vulnerable to DOS attacks, opening up the entire Ethereum network to resource abuse by malicious users.  
- Most importantly, this release reverts a data race introduced by the rushed soft-fork that can lead to miners crashing if they are running transactions simultaneously with importing blocks from the network. Thank you @9600- for finding this!  
  
## [v1.4.8](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.8) (2016-06-29)  
  
- Miners who do not update by definition vote against the soft-fork as they will continue the current logic of keeping the gas limit above the vote threshold. If the soft-fork is accepted by the majority, non-updating miners will still accept blocked transactions. In that case, non-updating miners will either fork off their own Ethereum network, diverging from the majority, or will forfeit any blocks they mined (since it's not accepted by the majority, overruling the minority blocks).  
- ### Should non-miners (nodes, wallets, mist, etc) update?  
- From the perspective of non-miners, this update has little relevance. Either outcome of the vote is equally valid from a plain node's perspective, so plain nodes will accept the heavier chain miners decide on without having to know anything about the soft-fork mechanism or results.  
- This release implements a **soft-fork**. A soft-fork is perfectly compatible with all protocol rules and requires only the consensus of the majority of miners to enact. It is temporary and can be removed/amended at any point in time upon miner consensus. It does not break protocol rules; it does not roll back any executed transactions/blocks; and it does not change any blockchain state outside of the original protocol capabilities.  
- _Note: This release does not represent a consent to hard-fork the network. It is a means to give people more time to come up with the best solution._  
  
## [v1.4.7](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.7) (2016-06-15)  
  
- - Fix ABI number encoding issues for native Dapps https://github.com/ethereum/go-ethereum/pull/2653 and https://github.com/ethereum/go-ethereum/pull/2680  
  
## [v1.4.6](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.6) (2016-06-06)  
  
- As Ethereum got more popular, the peer-to-peer networking infrastructure became extremely versatile (we have 3G peers in New Zealand and Satellite peers in the US). Certain assumptions that Geth made about the network when we've launched became untrue in the last months, especially with TheDAO bringing in an unexpected number of new users and environments. In this newly expanded network, the original assumptions caused Geth to be too aggressive with which peers are useful and which are not.  
- Geth 1.4.6 "EDGE" aims to solve the synchronization issues resulting from this newly found heterogeneity: it introduces a lot more download concurrency to avoid bottlenecks caused by badly connected remote machines, and also introduces adaptive quality-of-service tuning to make peer selection less aggressive for users with more restricted connectivity.  
- The result is a newly polished sync mechanism that can churn out a 7-8MB/s download speed in the current Ethereum main network (if your connectivity allows it), but also provide a solid and stable stream for all users, verified for as low as [**EDGE**](https://en.wikipedia.org/wiki/Enhanced_Data_Rates_for_GSM_Evolution) connections (440ms latency, 200 kbps upstream, 220kbps downstream).  
  
## [v1.4.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.4) (2016-05-17)  
  
- eth protocol vulnerabilities have been discovered by bounty hunters.  
  
## [v1.4.3](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.3) (2016-05-10)  
  
- We've added a [Geth release oracle](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code) contract to the network which holds the current version of `geth` with the release hash. All updated clients will check the contract and fetch the version number and check whether the version number is greater than the version you're running. It will emit a notice telling you there's a new client available. More info on #2497 .  
- #### Native Go DApp Development  
- With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.  
- #### Private networks  
- With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.  
- Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.  
- - Node refactoring to allow using Geth as a library  
  
## [v1.4.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.1) (2016-05-04)  
  
- We've added a [Geth release oracle](http://etherscan.io/address/0xFA7B9770Ca4cb04296Cac84F37736d4041251CDF#code) contract to the network which holds the current version of `geth` with the release hash. All updated clients will check the contract and fetch the version number and check whether the version number is greater than the version you're running. It will emit a notice telling you there's a new client available. More info on #2497 .  
- #### Native Go DApp Development  
- With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.  
- #### Private networks  
- With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.  
- Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.  
- - Node refactoring to allow using Geth as a library  
  
## [v1.4.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.4.0) (2016-04-20)  
  
- #### Native Go DApp Development  
- With the introduction of package `accounts/abi` which allowed you to create bindings between Ethereum Contracts and Go we've taken it one step further and developed a new tool which can do all the bindings for you given a ABI json structure. Our `abigen` (which is available in `cmd/abigen`) can create fully automated Go Bindings to any Ethereum contract without the need for manual fiddling. See our [step-by-step](https://github.com/ethereum/go-ethereum/wiki/Native-DApps:-Go bindings-to-Ethereum-contracts) tutorial for more information.  
- #### Private networks  
- With the interest of private networking growing we've added some new configuration utilities to `geth` and deprecated some flags that will be removed as of 1.5.  
- Geth should be buildable for iOS and Android platforms. While it's certainly not anywhere near feasible to run Geth in a production environment on an iPhone or on an Android device with the current full node implementation, however this will serve as a foundation for future updates, which will include LES (Light Ethereum Subprotocol) capabilities.  
- - Node refactoring to allow using Geth as a library  
  
## [v1.3.5](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5) (2016-03-03)  
  
- This release contains a crucial network hotfix #2285   
  
## [v1.3.4](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.4) (2016-02-29)  
  
- **UPDATE: Please use the [`1.3.5` release](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.5) which contains a network hotfix**  
- This is the first minor protocol upgrade that will set in at `1.150.000`'s block (Testnet = `494.000`). This release includes the following changes to the protocol and fixes:  
- ### Protocol changes  
-   2. All transaction signatures whose s-value is greater than `secp256k1n/2` are now considered invalid;  
- - Fixed windows p2p discover bug #2146   
- - Implement [EIP-8](https://github.com/ethereum/EIPs/pull/49) p2p protocol upgrade #2091   
  
## [v1.3.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.2) (2015-11-25)  
  
- Polish protocol gathering #1934   
- Peer download selection algorithm #1953   
  
## [v1.3.1](https://github.com/ethereum/go-ethereum/releases/tag/v1.3.1) (2015-11-03)  
  
- - Ethereum fast sync, eth/63 #1889  
  
## [v1.2.2](https://github.com/ethereum/go-ethereum/releases/tag/v1.2.2) (2015-10-02)  
  
- This release also fixes an issue with clients syncing on eth/62 occasionally dropping older eth/61 peers, though healthy peers will never drop anyone #1866   
  
## [1.2.1](https://github.com/ethereum/go-ethereum/releases/tag/1.2.1) (2015-10-01)  
  
- - Ethereum Wire Protocol version 62 #1701 ¹  
- ¹ While we've been running eth/62 for the last month or so untouched and haven't notices issues, a small local environment is very different from the live network. If you experience any sync problems, you can force geth to sync on the previous eth/61 protocol via `--eth=61`.  
  
## [v1.1.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.1.0) (2015-08-25)  
  
- - REPL NATSPEC/password prompts (user agents) #1635  
  
## [v1.0.0](https://github.com/ethereum/go-ethereum/releases/tag/v1.0.0) (2015-07-29)  
  
- - Protocol changes:  
  
## [v0.9.36](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.36) (2015-07-07)  
  
- - Improved block downloading & implemented Protocol version 61 #1333  
  
## [v0.9.32](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.32) (2015-06-23)  
  
-   - Attach to a running geth node with `geth attach`  
  
## [v0.9.30](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.30) (2015-06-15)  
  
- - P2P improvements [1261](https://github.com/ethereum/go-ethereum/pull/1261)  
- - Transaction pool improvement [1260](https://github.com/ethereum/go-ethereum/pull/1260). This issue fixes network load.  
  
## [v0.9.26](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.26) (2015-05-28)  
  
- This is a mandatory update and helps to improve the network's stability through several upgrade mechanism and download policies.  
  
## [v0.9.25](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.25) (2015-05-27)  
  
- This update also includes a re-work of the `p2p` dialing mechanism.  
  
## [v0.9.24](https://github.com/ethereum/go-ethereum/releases/tag/v0.9.24) (2015-05-26)  
  
- This update also includes a re-work of the `p2p` dialing mechanism.  
  
## [v0.8.5](https://github.com/ethereum/go-ethereum/releases/tag/v0.8.5) (2015-02-22)  
  
- - Improved networking & peer discovery  
  
## [v0.8.4](https://github.com/ethereum/go-ethereum/releases/tag/v0.8.4) (2015-02-20)  
  
- - Improved networking (P2P version 2)  
  
## [v0.6.8](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.8) (2014-10-07)  
  
- - Added black listing when kicking of bad peers  
  
## [v0.6.7](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.7) (2014-09-26)  
  
- - Added display for connected peer's capabilities.  
  
## [v0.6.0](https://github.com/ethereum/go-ethereum/releases/tag/v0.6.0) (2014-07-25)  
  
- - If a connection fails to a peer it will continue trying for a couple of times.  
- - Peers can be pushed towards clients even when the clients hasn't asked for it. Previously builds would ignore the peer server's `peer message`.
