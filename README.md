# Data Highway

The Data Highway Substrate-based blockchain node.

# Table of contents

* [Build and run blockchain](#chapter-5f0881)
* [Interact with blockchain using Polkadot.js Apps UI](#chapter-6d9058)
* [Maintain dependencies, rebuild, and add new runtime modules](#chapter-e16e68)
* [Debugging](#chapter-93c645)
* [Create custom blockchain configuration](#chapter-b1b53c)
* [Run multiple node PoA testnet using custom blockchain configuration](#chapter-f21efd)

Note: Generate a new chapter with `openssl rand -hex 3`

## Build and run blockchain <a id="chapter-5f0881"></a>

### Build blockchain node

* Fork and clone the repository

* Install Rust and dependencies. Build the WebAssembly binary from all code

```bash
curl https://getsubstrate.io -sSf | bash -s -- --fast && \
./scripts/init.sh && \
cargo build --release
```

### Run blockchain node (full node)

* Remove all existing blockchain testnet database and keys

```bash
./target/release/node purge-chain --dev --base-path /tmp/polkadot-chains/alice
```

* Connect to development testnet (`--chain development`)

```bash
./target/release/node \
  --base-path /tmp/polkadot-chains/alice \
  --name "Data Highway Testnet" \
  --dev \
  --telemetry-url ws://telemetry.polkadot.io:1024
```

Important: Setup Custom Types prior to submitting extrinsics otherwise StorageMap values from storage will not be displayed.

Detailed logs output by prefixing the above with: `RUST_LOG=debug RUST_BACKTRACE=1`

## Interact with blockchain using Polkadot.js Apps UI <a id="chapter-6d9058"></a>

* Interact with node when running it:
  * Go to Polkadot.js Apps "Settings" tab at https://polkadot.js.org/apps/#/settings
  * General > remote node/endpoint to connect to > Local Node (127.0.0.1:9944)

* Important:
  * Input parameter quirk: Sometimes it is necessary to modify the value of one of the input parameters to allow you to click "Submit Transaction" (i.e. if the first arguments input value is already 0 and appears valid, but the "Submit Transaction" button appears disabled, just delete the 0 value and re-enter 0 again)
  * Prior to being able to submit extrinics at https://polkadot.js.org/apps/#/extrinsics (i.e. roaming > createNetwork()) or to view StorageMap values, it is necessary to add the Custom Types to https://polkadot.js.org/apps/#/settings/developer, as follows, otherwise the "Submit Transaction" button will not work.

```
{
  "RoamingOperator": "[u8; 16]",
  "RoamingOperatorIndex": "u64",
  "RoamingNetwork": "[u8; 16]",
  "RoamingNetworkIndex": "u64",
  "RoamingOrganization": "[u8; 16]",
  "RoamingOrganizationIndex": "u64",
  "RoamingNetworkServer": "[u8; 16]",
  "RoamingNetworkServerIndex": "u64",
  "RoamingDevice": "[u8; 16]",
  "RoamingDeviceIndex": "u64",
  "RoamingRoutingProfile": "[u8; 16]",
  "RoamingRoutingProfileIndex": "u64",
  "RoamingRoutingProfileAppServer": "Text",
  "RoamingServiceProfile": "[u8; 16]",
  "RoamingServiceProfileIndex": "u64",
  "RoamingServiceProfileUplinkRate": "u32",
  "RoamingServiceProfileDownlinkRate": "u32",
  "RoamingAccountingPolicy": "[u8; 16]",
  "RoamingAccountingPolicyIndex": "u64",
  "RoamingAccountingPolicyType": "Text",
  "RoamingAccountingPolicyUplinkFeeFactor": "u32",
  "RoamingAccountingPolicyDownlinkFeeFactor": "u32",
  "RoamingAccountingPolicyConfig": {
    "policy_type": "Text",
    "subscription_fee": "Balance",
    "uplink_fee_factor": "u32",
    "downlink_fee_factor": "u32"
  },
  "RoamingAgreementPolicy": "[u8; 16]",
  "RoamingAgreementPolicyIndex": "u64",
  "RoamingAgreementPolicyActivationType": "Text",
  "RoamingAgreementPolicyExpiry": "Moment",
  "RoamingAgreementPolicyConfig": {
    "policy_activation_type": "Text",
    "policy_expiry": "u64"
  },
  "RoamingNetworkProfile": "[u8; 16]",
  "RoamingNetworkProfileIndex": "u64",
  "RoamingDeviceProfile": "[u8; 16]",
	"RoamingDeviceProfileIndex": "u64",
	"RoamingDeviceProfileDevAddr": "Text",
	"RoamingDeviceProfileDevEUI": "Text",
	"RoamingDeviceProfileJoinEUI": "Text",
	"RoamingDeviceProfileVendorID": "Text",
  "RoamingDeviceProfileConfig": {
    "device_profile_devaddr": "Text",
    "device_profile_deveui": "Text",
    "device_profile_joineui": "Text",
    "device_profile_vendorid": "Text"
  },
  "RoamingSession": "[u8; 16]",
  "RoamingSessionIndex": "u64",
  "RoamingSessionJoinRequestRequestedAt": "Moment",
  "RoamingSessionJoinRequestAcceptExpiry": "Moment",
  "RoamingSessionJoinRequestAcceptAcceptedAt": "Moment",
  "RoamingSessionJoinRequest": {
    "session_network_server_id": "Moment",
    "session_join_request_requested_at": "Moment"
  },
  "RoamingSessionJoinAccept": {
    "session_join_request_accept_expiry": "Moment",
    "session_join_request_accept_accepted_at": "Moment"
  },
  "RoamingBillingPolicy": "[u8; 16]",
  "RoamingBillingPolicyIndex": "u64",
  "RoamingBillingPolicyNextBillingAt": "Moment",
  "RoamingBillingPolicyFrequencyInDays": "u64",
  "RoamingBillingPolicyConfig": {
    "policy_next_billing_at": "Moment",
    "policy_frequency_in_days": "u64"
  },
  "RoamingChargingPolicy": "[u8; 16]",
  "RoamingChargingPolicyIndex": "u64",
  "RoamingChargingPolicyNextChargingAt": "Moment",
  "RoamingChargingPolicyDelayAfterBillingInDays": "u64",
  "RoamingChargingPolicyConfig": {
    "policy_next_charging_at": "Moment",
    "policy_delay_after_billing_in_days": "u64"
  },
  "RoamingPacketBundle": "[u8; 16]",
  "RoamingPacketBundleIndex": "u64",
  "RoamingPacketBundleReceivedAtHome": "bool",
  "RoamingPacketBundleReceivedPacketsCount": "u64",
  "RoamingPacketBundleReceivedPacketsOkCount": "u64",
  "RoamingPacketBundleReceivedStartedAt": "Moment",
  "RoamingPacketBundleReceivedEndedAt": "Moment",
  "RoamingPacketBundleExternalDataStorageHash": "Hash",
  "RoamingPacketBundleReceiver": {
    "packet_bundle_received_at_home": "bool",
    "packet_bundle_received_packets_count": "u64",
    "packet_bundle_received_packets_ok_count": "u64",
    "packet_bundle_received_started_at": "Moment",
    "packet_bundle_received_ended_at": "Moment",
    "packet_bundle_external_data_storage_hash": "Hash"
  },
}
```

## Maintain dependencies, rebuild, and add new runtime modules <a id="chapter-e16e68"></a>

### Add new runtime module

```bash
substrate-module-new <module-name> <author>
```

### Update Rust and dependencies

```bash
curl https://getsubstrate.io -sSf | bash && \
./scripts/init.sh
```

### Re-build runtime after purge chain database of all blocks.

```bash
./target/release/node purge-chain --dev
cargo build --release
```

### All Tests

```bash
cargo test -p node-runtime &&
cargo test -p roaming-operators &&
cargo test -p roaming-networks &&
cargo test -p roaming-organizations &&
cargo test -p roaming-network-servers &&
cargo test -p roaming-devices &&
cargo test -p roaming-routing-profiles &&
cargo test -p roaming-service-profiles &&
cargo test -p roaming-accounting-policies &&
cargo test -p roaming-agreement-policies &&
cargo test -p roaming-network-profiles &&
cargo test -p roaming-device-profiles &&
cargo test -p roaming-sessions &&
cargo test -p roaming-billing-policies &&
cargo test -p roaming-charging-policies &&
cargo test -p roaming-packet-bundles
```

## Integration Tests

```
cargo test -p node-runtime
```

### Check

```
cargo check
```

### Install Specific Dependencies

```
cargo install cargo-edit
cargo add ...
```

### Upgrade runtime

https://www.youtube.com/watch?v=0aTnxHrV_j4&list=PLOyWqupZ-WGt3mA_d9wu74vVe0bM37-39&index=9&t=0s

## Debugging <a id="chapter-93c645"></a>

### Simple Debugging

* Add to Cargo.toml of runtime module:
```yaml
...
    'log/std',
...
[dependencies.log]
version = "0.4.8"
```

* Add to my-module/src/lib.rs
```rust
use log::{error, info, debug, trace};
...
log::debug!("hello {:?}", world); // Only shows in terminal in debug mode
log::info!("hello {:?}", world); // Shows in terminal in release mode
```

### Detailed Debugging

```bash
RUST_LOG=debug RUST_BACKTRACE=1 ./target/release/node ...
```


## Create custom blockchain configuration <a id="chapter-b1b53c"></a>

* Create latest chain specification code changes of local chain

```bash
mkdir -p ./src/chain-spec-templates
./target/release/node build-spec \
  --chain=local > ./src/chain-spec-templates/chainspec_latest.json
```

* Create template chain specification from default local chain

```bash
mkdir -p ./src/chain-spec-templates
./target/release/node build-spec \
  --chain=local > ./src/chain-spec-templates/chainspec_default.json
```

* Edit chain specification according to cryptocurrency design requirements

* Edit WebAssembly code blob with latest chain changes by copying the "code" section from chainspec_latest.json and pasting it into the "code" field of chainspec_dh.json

* Build "raw" chain definition for the new chain

```bash
mkdir -p ./src/chain-definition-custom
./target/release/node build-spec \
  --chain ./src/chain-spec-templates/chainspec_dh.json \
  --raw > ./src/chain-definition-custom/chain.json
```

## Run multiple node PoA testnet using custom blockchain configuration <a id="chapter-f21efd"></a>

* Run custom Substrate-based blockchain on local machine testnet with multiple terminals:
  * Imported custom chain definition for custom testnet
  * Use default accounts Alice and Bob as the two initial authorities of the genesis configuration that have been endowed with testnet units that will run validator nodes
  * Multiple authority nodes using the Aura consensus to produce blocks

Terminal 1: Alice's Substrate-based node on default TCP port 30333 with her chain database stored locally at `/tmp/polkadot-chains/alice` and where the bootnode ID of her node is `Local node identity is: QmZ5kgdoLCx3Qfy8nJAiP1U9i6iY3qeiDNSCdHmHRJtSnF` (peer id), which is generated from the `--node-key` value specified below and shown when the node is running. Note that `--alice` provides Alice's session key that is shown when you run `subkey -e inspect //Alice`, alternatively you could provide the private key to that is necessary to produce blocks with `--key "bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice"`. In production the session keys are provided to the node using RPC calls `author_insertKey` and `author_rotateKeys`. 
If you explicitly specify a `--node-key` when you start your validator node, the logs will still display your peer id with `Local node identity is: Qxxxxxx`, and you could then include it in the chainspec.json file under "bootNodes". Also the peer id is listed when you go to view the list of full nodes and authority nodes at Polkadot.js Apps https://polkadot.js.org/apps/#/explorer/node:

```bash
./target/release/node --validator \
  --base-path /tmp/polkadot-chains/alice \
  --keystore-path "/tmp/polkadot-chains/alice/keys" \
  --chain ./src/chain-definition-custom/chain.json \
  --key "bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice" \
  --node-key 88dc3417d5058ec4b4503e0c12ea1a0a89be200fe98922423d4334014fa6b0ee \
  --port 30333 \
  --telemetry-url ws://telemetry.polkadot.io:1024
```

Terminal 2: Bob's Substrate-based node on a different TCP port of 30334, and with his chain database stored locally at `/tmp/polkadot-chains/alice`. We'll specify a value for the `--bootnodes` option that will connect his node to Alice's bootnode ID on TCP port 30333:

```bash
./target/release/node --validator \
  --base-path /tmp/polkadot-chains/bob \
  --bootnodes /ip4/127.0.0.1/tcp/30333/p2p/QmZ5kgdoLCx3Qfy8nJAiP1U9i6iY3qeiDNSCdHmHRJtSnF \
  --chain ./src/chain-definition-custom/chain.json \
  --bob \
  --port 30334 \
  --telemetry-url ws://telemetry.polkadot.io:1024
```

* View on [Polkadot Telemetry](https://telemetry.polkadot.io/#list/My%20Testnet)

* Distribute the custom chain specification to allow others to synchronise and validate if they are an authority

* Add session keys for other account(s) to be configured as authorities (validators)
