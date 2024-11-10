# EVAA Protocol
Welcome to EVAA Protocol smart cotntracts GitHub repository! This repository contains the smartcontracts for the first lending protocol on TON blockchain.

# License
This project is licensed under the **Business Source License (BUSL) 1.1**. The full text of the license can be found in the [LICENSE.md](./LICENSE.md) file.

# Audit
EVAA contracts was audited by quantstamp. Read more here: [certificate](https://certificate.quantstamp.com/full/evaa/df7aa699-793b-49f7-b348-1f78e9ca9870/index.html).

*Contracts code version same as gh commit 55096cf1fd091629ff8dad783f71fb4758eded46 on original repo.*

*You can verify that code inside ./contracts folder is the one that was audited, you can compare the hashes of each file in the ./contracts folder with the hash that is in the Quantstamp certificate.*

# Links
- Evaa [SDK](https://github.com/evaafi/sdk) 
- Evaa liquidation [bot](https://github.com/evaafi/liquidator-bot-v2-pub) 
- Evaa [web](https://evaa.finance)
- Evaa web [app](https://app.evaa.finance)
- Evaa telegram [bot](https://evaaappbot.t.me) 
- Evaa telegram [channel](https://evaaprotocol.t.me)
- Evaa on [X](https://x.com/evaaprotocol) 

# Technical smart-contract README
TLB schemas for transactions body and storage can be found in the ./schema folder. 

# Folder structure in ./contracts
- `/` - root code files, each compiling to a separate smart contract
- `/core` - main code of the protocol (rcv opcode -> parse incoming tx -> execute some logic (call functions from /logic folder) -> send outcoming tx)
- `/logic` - main logic functions of the protocol
- `/data` - functions to work with data: packers, unpackers, `.store_X`, `~load_X`, etc. - everything intermediate, which is not used as a "final" type (storage or message)
- `/storage` - functions for pack / unpack & save / read storage of user & master sc 
- `/messages` - "final" types, which represent messages and function to work with them
- `/constants` - files with constants (op codes, errors, fees), a lot of them
- `/external` - everything not directly related to EVAA

# Main data types

## Master sc

### asset_config_collection
Dict: asset_id -> `asset_config`
Configuration of a particular asset, which is set during its initialization and is not changed (admin can only change it if very necessary).

### asset_dynamics_collection
Dict: asset_id -> `asset_dynamics`
Information about current changing data (sRate, bRate, etc.), related to a specific asset.

## User sc 

### user_principals
Dictionary: asset -> balance (positive for deposits and negative for debts).

### user_rewards 
Dictionary: asset -> tracking_indexes & tracking_accrued (information about how many rea user will receive for positions in a particular token).

## Various
`asset_id` ≈ `sha256 from ticker of the token (e.g. 'TON' 'jUSDT' 'jUSDC')`
