# EVAA Protocol

Welcome to the EVAA Protocol smart contracts GitHub repository! This repository contains the smart contracts for the first lending protocol on the TON blockchain.

# License

This project is licensed under the **Business Source License (BUSL) 1.1**. The full text of the license can be found in the [LICENSE.md](./LICENSE.md) file.

# Audit

The EVAA contracts have been audited by Quantstamp. You can read the full audit report here: [certificate](https://certificate.quantstamp.com/full/evaa/df7aa699-793b-49f7-b348-1f78e9ca9870/index.html).

*The version of the contract code matches the GitHub commit `55096cf1fd091629ff8dad783f71fb4758eded46` from the original repository.*

*To verify that the code in the `./contracts` folder is the same as the audited version, compare the hashes of each file in this folder with the hashes listed in the Quantstamp certificate.*

*If you'd like to compare the hash of a specific file with the one in the audit report, check out the previous commit `1fb4e31dd7874391e34bae2cdfa5dd0d48b5d181`. This (last) commit contains the same code, with no logic changes, only updated comments. Then, generate the hash for the file and compare it with the one in the audit certificate.*

# Links

- EVAA [SDK](https://github.com/evaafi/sdk) 
- EVAA liquidation [bot](https://github.com/evaafi/liquidator-bot-v2-pub) 
- EVAA [website](https://evaa.finance)
- EVAA [web app](https://app.evaa.finance)
- EVAA telegram [bot](https://evaaappbot.t.me) 
- EVAA telegram [channel](https://evaaprotocol.t.me)
- EVAA on [X](https://x.com/evaaprotocol)

# Technical Smart Contract README

TLB schemas for transaction bodies and storage can be found in the `./schema` folder.

# Folder Structure in `./contracts`

- `/` - Root code files, each compiling to a separate smart contract.
- `/core` - Main code of the protocol (rcv opcode → parse incoming tx → execute some logic (call functions from `/logic` folder) → send outgoing tx).
- `/logic` - Main logic functions of the protocol.
- `/data` - Functions to work with data: packers, unpackers, `.store_X`, `~load_X`, etc. – everything intermediate that is not used as a "final" type (storage or message).
- `/storage` - Functions for packing/unpacking & saving/reading storage of user & master smart contracts.
- `/messages` - "Final" types representing messages and functions to work with them.
- `/constants` - Files with constants (opcodes, errors, fees), many of them.
- `/external` - Everything not directly related to EVAA.

# Main Data Types

## Master Smart Contract

### `asset_config_collection`
Dictionary: `asset_id` → `asset_config`
Configuration of a particular asset, which is set during its initialization and cannot be changed (the admin can only change it if absolutely necessary).

### `asset_dynamics_collection`
Dictionary: `asset_id` → `asset_dynamics`
Information about current changing data (such as `sRate`, `bRate`, etc.) related to a specific asset.

## User Smart Contract

### `user_principals`
Dictionary: `asset` → `balance` (positive for deposits and negative for debts).

### `user_rewards`
Dictionary: `asset` → `tracking_indexes` & `tracking_accrued` (information about how much a user will receive for positions in a particular token).

## Various

`asset_id` ≈ `sha256` from the ticker of the token (e.g., 'TON', 'jUSDT', 'jUSDC').
