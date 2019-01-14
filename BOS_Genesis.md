# BOS Genesis - BOS Mainnet Launch Sequence

## Step 0: The keys to the world of BOS 
```
EOSIO_PUB="EOS6YFfrYZ3z7bNnzoZF4V2zJStudMJ4NJBBE1JXGzebVzMG9aLc7"    # TODO: need to change 
BOS_PUB="EOS5tpFnykJ8CMeodDaarMUMNmtr5vMCA2NUabeLDXVDLW41aQg59"      # TODO: need to change 

# Schema: account ownerkey activekey
BOS_KEYS=("bos" "EOS6T27NLVPxSJxQHrjRVCa1w8LPjAFdWMjvAT8jwcZWXjJtAvLxZ" "EOS6T27NLVPxSJxQHrjRVCa1w8LPjAFdWMjvAT8jwcZWXjJtAvLxZ")  # TODO: need to change 
ABP_KEYS=("bos.abp" "EOS7Dx8ZsijpAD7HTRmtCMpHKys5T5XEmYmWBLmoyr6gygtEhXBmM")  # TODO: need to change 

# the Core Teams' accounts and keys
MERCURY_KEYS=("mercury" "EOS8Ybz56pR5b4Rq2nvJ3ZT9CehG78pXEYdwQ9LHKPm4ZFuwM777W" "EOS8KGrtNbB13u3vt5FY84w6777EAJqCbP2ywxcgD1LFC9MGxUgh6")
VENUS_KEYS=("venus" "EOS8iN2NZR29sKjfgraREWMZCTSUTuULMRiL3QmU1McCHxDBLeRcL" "EOS5xcrat34D5vkPj8vx1WRCS3Rd5PxjQbHrz7pmyLKLTsDDW91nn")
EARTH_KEYS=("earth" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT")
MARS_KEYS=("mars" "EOS8DmeNbNDJc8wMNd7C2Upz3zDRhc6P87QHiW8EbssjKSLSGTrbs" "EOS71u9BFuat4CtxzbqzFGzacgdgDdtujpiRm95Kyw24MUuMvWUNX")
JUPITER_KEYS=("jupiter" "EOS7do6GqRAJEpcJY536zG6j3VqXMMdpMKtDCkFXzYYNCKLB73xVU" "EOS5yajrR2u7XxDcnjXE2H6WYjDabi2Z44zRrYz6pK3fZtFAyrnt5")
URANUS_KEYS=("uranus" "EOS84TUrXoKKWYdUMoTtGKqKge9oqBDKSqxKfX7oHmkxD5LbGLT2z" "EOS6bNYimeRE6QQcw5yMdMVSotZ87gX3QN5XEQTbwvsKAVVs7JTiH")
NEPTUNE_KEYS=("neptune" "EOS7YyA18xX8zdDaWGSB7d7aV5qjUoG6YE7murr2Nv7TbyS6WNSCy" "EOS68aR5GxMugpxoFnQyA3k6EXiBYFvjqXRMiTDy9HFynLxqr4LEh")
SATURN_KEYS=("saturn" "EOS6VbgttSHcQBsQnpnNZGR67JYBcbV7gsNRyjx3JTEoPXRCCvg8n" "EOS6DYnE3PNg4su8fZyDffUvXDKvFDVxbBdJ8fMBjdp7CkroUgN7F")
SUN_KEYS=("sun" "EOS6VbgttSHcQBsQnpnNZGR67JYBcbV7gsNRyjx3JTEoPXRCCvg8n" "EOS6DYnE3PNg4su8fZyDffUvXDKvFDVxbBdJ8fMBjdp7CkroUgN7F")
```

## Step 1: Prepare config.ini and genesis.json by start a node with eosio
```

nodeos --config-dir /data/eos-config -d /data/eos-data --genesis-json /data/eos-config/genesis.json
```

## Step 2: Prepare wallet and import keys
```
alias cleos='cleos -u http://127.0.0.1:9999'
cleos wallet create 
cleos wallet import --private-key <private-key>
```

## Step 3: Set contract eosio.bios
```
CONTRACTS_FOLDER='./bos.contract-prebuild' 
cleos set contract eosio ${CONTRACTS_FOLDER}/eosio.bios -p eosio
```

## Step 4: Create system accounts
```
SYS_ACTS=(
    "eosio.bpay" "eosio.msig" "eosio.names" "eosio.ram" "eosio.ramfee" 
    "eosio.saving" "eosio.stake" "eosio.token" "eosio.vpay" "eosio.wrap" 
    "bos.dev" "bos.gov" "bos.bhole"
)
for account in ${SYS_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    cleos create account eosio ${account} ${EOSIO_PUB}; 
    sleep 1; 
done

FEATURE_ACTS=(
    "redpacket" "btc.bos" "eth.bos" "eos.bos" "usdt.bos" "bos.adrop" 
    "bos.op" "bos.angel" "bos.eco" "bos.pioneer" "io" "bosibc.io"
)
for account in ${FEATURE_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    cleos create account eosio ${account} ${BOS_PUB}; 
    sleep 1; 
done
```

## Step 5: Create the BOS Core Team accounts
```
cleos create account eosio ${BOS_KEYS[0]} ${BOS_KEYS[1]} ${BOS_KEYS[2]}
cleos create account eosio ${ABP_KEYS[0]} ${ABP_KEYS[1]} ${ABP_KEYS[1]}

cleos create account eosio ${MERCURY_KEYS[0]} ${MERCURY_KEYS[1]} ${MERCURY_KEYS[2]}
cleos create account eosio ${VENUS_KEYS[0]} ${VENUS_KEYS[1]} ${VENUS_KEYS[2]}
cleos create account eosio ${EARTH_KEYS[0]} ${EARTH_KEYS[1]} ${EARTH_KEYS[2]}
cleos create account eosio ${MARS_KEYS[0]} ${MARS_KEYS[1]} ${MARS_KEYS[2]}
cleos create account eosio ${JUPITER_KEYS[0]} ${JUPITER_KEYS[1]} ${JUPITER_KEYS[2]}
cleos create account eosio ${URANUS_KEYS[0]} ${URANUS_KEYS[1]} ${URANUS_KEYS[2]}
cleos create account eosio ${NEPTUNE_KEYS[0]} ${NEPTUNE_KEYS[1]} ${NEPTUNE_KEYS[2]}
cleos create account eosio ${SATURN_KEYS[0]} ${SATURN_KEYS[1]} ${SATURN_KEYS[2]}
cleos create account eosio ${SUN_KEYS[0]} ${SUN_KEYS[1]} ${SUN_KEYS[2]}
```

## Step 6: Assign the ABP 
```
cleos push action eosio setprods '{"schedule":[{"producer_name":"'${ABP_KEYS[0]}'","block_signing_key":"'${ABP_KEYS[1]}'"}]}' -p eosio
```

## Step 7: Set token,msig,wrap contract
```
cleos set contract eosio.token ${CONTRACTS_FOLDER}/eosio.token -p eosio.token 
cleos set contract eosio.msig ${CONTRACTS_FOLDER}/eosio.msig -p eosio.msig
cleos set contract eosio.wrap ${CONTRACTS_FOLDER}/eosio.wrap -x 1000 -p eosio.wrap
```

# Setp 8: Setting privileged account for eosio.msig
```
cleos push action eosio setpriv '{"account": "eosio.msig", "is_priv": 1}' -p eosio
```

## Step 9: Create and issue token
```
cleos push action eosio.token create '["eosio", "10000000000.0000 BOS"]' -p eosio.token 
cleos push action eosio.token issue '["eosio", "1000158000.0000 BOS", "BOSCore"]' -p eosio  # 158000 to create account
```

## Step 10: Set contract eosio.system
```
cleos set contract eosio ${CONTRACTS_FOLDER}/eosio.system -x 1000 -p eosio
cleos push action eosio init '{"version": 0, "core": "4,BOS"}' -p eosio
```

## Step 11: Allocate token 
```
cleos transfer eosio bos          "200000000.0000 BOS" "Lock 4 years, unlock daily"
cleos transfer eosio bos.op       "100000000.0000 BOS" "BOS operation"
cleos transfer eosio bos.angel    "200000000.0000 BOS" "4 times of angel investment"
cleos transfer eosio bos.eco      "400000000.0000 BOS" "BOS eco-cultivation"
cleos transfer eosio bos.adrop    "50158000.0000 BOS"  "Airdrop for EOS directly"
cleos transfer eosio bos.pioneer  "50000000.0000 BOS"  "For better BPs and DApps"
```

## Step 12: Delegate BOS
```
# buyram for accounts
cleos system buyram bos.adrop bos "10.0000 BOS"
cleos system buyram bos.adrop bos.op "10.0000 BOS"
cleos system buyram bos.adrop bos.angel "10.0000 BOS"
cleos system buyram bos.adrop bos.eco  "10.0000 BOS"
cleos system buyram bos.adrop bos.pioneer "10.0000 BOS"

# delegate 
cleos system delegatebw bos bos              "100000000.0000 BOS" "100000000.0000 BOS"
cleos system delegatebw bos.op bos.op        "50000000.0000 BOS" "50000000.0000 BOS" 
cleos system delegatebw bos.angel bos.angel  "100000000.0000 BOS" "100000000.0000 BOS"
cleos system delegatebw bos.eco bos.eco      "200000000.0000 BOS" "200000000.0000 BOS"
cleos system delegatebw bos.pioneer bos.pioneer  "25000000.0000 BOS" "25000000.0000 BOS"
```

## Step 13: Change BOS accounts to multisig
```
MSIG_ACTIVE_RULE='{"threshold":5,"keys":[],"accounts":[{"permission":{"actor":"earth","permission":"active"},"weight":1},{"permission":{"actor":"jupiter","permission":"active"},"weight":1},{"permission":{"actor":"mars","permission":"active"},"weight":1},{"permission":{"actor":"mercury","permission":"active"},"weight":1},{"permission":{"actor":"neptune","permission":"active"},"weight":1},{"permission":{"actor":"uranus","permission":"active"},"weight":1},{"permission":{"actor":"venus","permission":"active"},"weight":1}],"waits":[]}'

MSIG_OWNER_RULE='{"threshold":2,"keys":[],"accounts":[{"permission":{"actor":"mars","permission":"owner"},"weight":1},{"permission":{"actor":"mercury","permission":"owner"},"weight":1},{"permission":{"actor":"uranus","permission":"owner"},"weight":1}],"waits":[]}'

MSIG_ACTS=("bos" "bos.op" "bos.angel" "bos.eco" "bos.pioneer")
for account in ${MSIG_ACTS[*]}
do
    echo -e "\n multisig setup $account \n"; 
    cleos set account permission ${account} active ${MSIG_ACTIVE_RULE} owner -p ${account}@owner
    cleos set account permission ${account} owner ${MSIG_OWNER_RULE} -p ${account}@owner
    sleep 1; 
done
```

## Step 14: Resign all system accounts
```
for account in ${SYS_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    cleos push action eosio updateauth '{"account": "'$account'", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@active
    cleos push action eosio updateauth '{"account": "'$account'", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@owner
    sleep 1; 
done

# resign eosio
cleos push action eosio updateauth '{"account": "eosio", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@active 
cleos push action eosio updateauth '{"account": "eosio", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@owner

# check again
for account in ${SYS_ACTS[*]}
do 
    echo --- ${account} --- && cleos get account ${account} && sleep 1; 
done

cleos get account eosio
```

## Step 15: Import the accounts from snapshot and airdrop 

- Use the snapshot import tool to airdrop by the `bos.adrop`.
- Transfer the extra BOS into the black hole account `bos.bhole` and the `bos.adrop` should be zero
```
    cleos transfer bos.adrop bos.bhole "xxxxx BOS"
```

The top 50 EOSMainnet BPs' accounts will be kept by replacing **eos** into **bos**.  
This account list be taken at 2019-01-10 14:30 UTC+13
```
eoshuobipool, starteosiobp, eoslaomaocom, zbeosbp11111, eosflytomars, eosliquideos, eosiosg11111, bitfinexeos1, atticlabeosb, jedaaaaaaaaa, cochainworld, eosnewyorkio, eoscannonchn, eosbixinboot, eos42freedom, eosbeijingbp, eoscanadacom, eoshenzhenio, eosauthority, eosriobrazil, eosnationftw, eosdacserver, eosswedenorg, teamgreymass, cypherglasss, eospaceioeos, games.eos(gamesbosssss), oraclegogogo, unlimitedeos, helloeoscnbp, eostitanprod, eoscafeblock, eosiomeetone, libertyblock, eospacificbp, cryptolions1, eosasia11111, argentinaeos, eosfishrocks, eoszhizhutop, eosyskoreabp, eosfengwocom, eosgenblockp, eosvolgabpru, alohaeosprod, eostribeprod, eos.fish(fishbossssss), eosstorebest, eosdotwikibp, moonxwitheos
```

The snapshot files can be found at: 
- [Snapshot Readme](https://github.com/boscore/bos-airdrop-snapshots)
- [accounts_info_bos_snapshot.airdrop.msig.json](https://github.com/boscore/bos-airdrop-snapshots/blob/master/accounts_info_bos_snapshot.airdrop.msig.json)
- [accounts_info_bos_snapshot.airdrop.normal.csv](https://github.com/boscore/bos-airdrop-snapshots/blob/master/accounts_info_bos_snapshot.airdrop.normal.csv)

## Step 16: Validate the airdrop by community
```
    # pause the ABP to produce block to wait the community validation
    curl http://127.0.0.1:8888/v1/producer/pause

    # after the validation, continue to produce block
    curl http://127.0.0.1:8888/v1/producer/resume
```

## Step 17: Hello BOS
