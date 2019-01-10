# The BOS Genesis

# Step 0: The keys to the world of BOS 
```
EOSIO_PUB="EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop"    # TODO: need to change 
BOS_PUB="EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop"      # TODO: need to change 

# Schema: account ownerkey activekey
BOS_KEYS=("bos" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop")  # TODO: need to change 
ABP_KEYS=("bos.abp" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop")  # TODO: need to change 

# the Core Teams' accounts and keys
MERCURY_KEYS=("mercury" "EOS8Ybz56pR5b4Rq2nvJ3ZT9CehG78pXEYdwQ9LHKPm4ZFuwM777W" "EOS8Ybz56pR5b4Rq2nvJ3ZT9CehG78pXEYdwQ9LHKPm4ZFuwM777W")
VEUNS_KEYS=("veuns" "EOS8iN2NZR29sKjfgraREWMZCTSUTuULMRiL3QmU1McCHxDBLeRcL" "EOS5xcrat34D5vkPj8vx1WRCS3Rd5PxjQbHrz7pmyLKLTsDDW91nn")
EARTH_KEYS=("earth" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT")
MARS_KEYS=("mars" "EOS8DmeNbNDJc8wMNd7C2Upz3zDRhc6P87QHiW8EbssjKSLSGTrbs" "EOS71u9BFuat4CtxzbqzFGzacgdgDdtujpiRm95Kyw24MUuMvWUNX")
JUPITER_KEYS=("jupiter" "EOS7do6GqRAJEpcJY536zG6j3VqXMMdpMKtDCkFXzYYNCKLB73xVU" "EOS5yajrR2u7XxDcnjXE2H6WYjDabi2Z44zRrYz6pK3fZtFAyrnt5")
URANUS_KEYS=("uranus" "EOS84TUrXoKKWYdUMoTtGKqKge9oqBDKSqxKfX7oHmkxD5LbGLT2z" "EOS6bNYimeRE6QQcw5yMdMVSotZ87gX3QN5XEQTbwvsKAVVs7JTiH")
NEPTUNE_KEYS=("neptune" "EOS7YyA18xX8zdDaWGSB7d7aV5qjUoG6YE7murr2Nv7TbyS6WNSCy" "EOS68aR5GxMugpxoFnQyA3k6EXiBYFvjqXRMiTDy9HFynLxqr4LEh")
```

# Step 1: Prepare config.ini and genesis.json by start a node with eosio
```

nodeos --config-dir /data/eos-config -d /data/eos-data --genesis-json /data/eos-config/genesis.json
```

# Step 2: Prepare wallet and import keys
```
alias cleos='cleos -u http://127.0.0.1:9999'
cleos wallet create 
cleos wallet import --private-key <private-key>
```

# Step 3: Set contract eosio.bios
```
CONTRACTS_FOLDER='~/bos.contract-prebuild' 
cleos set contract eosio ${CONTRACTS_FOLDER}/eosio.bios -p eosio
```

# Step 4: Create system accounts
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
    "redpacket" "bos.btc" "bos.eth" "bos.eos" "bos.adrop"
    "bos.op" "bos.angel" "bos.eco" "bos.pioneer" 
)
for account in ${FEATURE_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    cleos create account eosio ${account} ${BOS_PUB}; 
    sleep 1; 
done
```

# Step 5: Create the BOS Core Team accounts
```
cleos create account eosio ${BOS_KEYS[0]} ${BOS_KEYS[1]} ${BOS_KEYS[2]}
cleos create account eosio ${ABP_KEYS[0]} ${ABP_KEYS[1]} ${ABP_KEYS[1]}

cleos create account eosio ${MERCURY_KEYS[0]} ${MERCURY_KEYS[1]} ${MERCURY_KEYS[2]}
cleos create account eosio ${VEUNS_KEYS[0]} ${VEUNS_KEYS[1]} ${VEUNS_KEYS[2]}
cleos create account eosio ${EARTH_KEYS[0]} ${EARTH_KEYS[1]} ${EARTH_KEYS[2]}
cleos create account eosio ${MARS_KEYS[0]} ${MARS_KEYS[1]} ${MARS_KEYS[2]}
cleos create account eosio ${JUPITER_KEYS[0]} ${JUPITER_KEYS[1]} ${JUPITER_KEYS[2]}
cleos create account eosio ${URANUS_KEYS[0]} ${URANUS_KEYS[1]} ${URANUS_KEYS[2]}
cleos create account eosio ${NEPTUNE_KEYS[0]} ${NEPTUNE_KEYS[1]} ${NEPTUNE_KEYS[2]}
```

# Step 6: Assign the ABP 
```
cleos push action eosio setprods '{"schedule":[{"producer_name":"'${ABP_KEYS[0]}'","block_signing_key":"'${ABP_KEYS[1]}'"}]}' -p eosio
```

# Step 7: Set token,msig,wrap contract
```
cleos set contract eosio.token ${CONTRACTS_FOLDER}/eosio.token -p eosio.token 
cleos set contract eosio.msig ${CONTRACTS_FOLDER}/eosio.msig -p eosio.msig
cleos set contract eosio.wrap ${CONTRACTS_FOLDER}/eosio.wrap -x 1000 -p eosio.wrap
```

# Setp 8: Setting privileged account for eosio.msig
```
cleos push action eosio setpriv '{"account": "eosio.msig", "is_priv": 1}' -p eosio
```

# Step 9: Create and issue token
```
cleos push action eosio.token create '["eosio", "10000000000.0000 BOS"]' -p eosio.token 
cleos push action eosio.token issue '["eosio", "1000158000.0000 BOS", "BOSCore"]' -p eosio  # 158000 to create account
```

# Step 10: Set contract eosio.system
```
cleos set contract eosio ${CONTRACTS_FOLDER}/eosio.system -x 1000 -p eosio
cleos push action eosio init '{"version": 0, "core": "4,BOS"}' -p eosio
```

# Step 11: Change BOS accounts to multisig
```
MSIG_ACTIVE_RULE='{"threshold":5,"keys":[],"accounts":[{"permission":{"actor":"earth","permission":"active"},"weight":1},{"permission":{"actor":"jupiter","permission":"active"},"weight":1},{"permission":{"actor":"mars","permission":"active"},"weight":1},{"permission":{"actor":"mercury","permission":"active"},"weight":1},{"permission":{"actor":"neptune","permission":"active"},"weight":1},{"permission":{"actor":"uranus","permission":"active"},"weight":1},{"permission":{"actor":"veuns","permission":"active"},"weight":1}],"waits":[]}'

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

# Step 12: Allocate token 
```
cleos transfer eosio bos          "200000000.0000 BOS" "Lock 4 years, unlock daily"
cleos transfer eosio bos.op       "100000000.0000 BOS" "BOS operation"
cleos transfer eosio bos.angel    "200000000.0000 BOS" "4 times of angel investment"
cleos transfer eosio bos.eco      "400000000.0000 BOS" "BOS eco-cultivation"
cleos transfer eosio bos.adrop    "50000000.0000 BOS"  "Airdrop for EOS directly"
cleos transfer eosio bos.pioneer  "50000000.0000 BOS"  "For better BPs and DApps"
```

# Step 13: Resign all system accounts
```
for account in ${SYS_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    cleos push action eosio updateauth '{"account": "'$account'", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@active
    cleos push action eosio updateauth '{"account": "'$account'", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@owner
    sleep 1; 
done

#resign eosio
cleos push action eosio updateauth '{"account": "eosio", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@active 
cleos push action eosio updateauth '{"account": "eosio", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@owner

#check again
for account in ${SYS_ACTS[*]}
do 
    echo --- ${account} --- && cleos get account ${account} && sleep 1; 
done

cleos get account eosio
```

# Step 14: Import the accounts from snapshot and airdrop 
The top 50 EOSMainnet BPs' accounts will be kept by replacing **eos** into **bos**.  
This account list be taken at 2019-01-10 14:30 UTC+13
```
eoshuobipool, starteosiobp, eoslaomaocom, zbeosbp11111, eosflytomars, eosliquideos, eosiosg11111, bitfinexeos1, atticlabeosb, jedaaaaaaaaa, cochainworld, eosnewyorkio, eoscannonchn, eosbixinboot, eos42freedom, eosbeijingbp, eoscanadacom, eoshenzhenio, eosauthority, eosriobrazil, eosnationftw, eosdacserver, eosswedenorg, teamgreymass, cypherglasss, eospaceioeos, games.eos(gamesbosssss), oraclegogogo, unlimitedeos, helloeoscnbp, eostitanprod, eoscafeblock, eosiomeetone, libertyblock, eospacificbp, cryptolions1, eosasia11111, argentinaeos, eosfishrocks, eoszhizhutop, eosyskoreabp, eosfengwocom, eosgenblockp, eosvolgabpru, alohaeosprod, eostribeprod, eos.fish(fishbossssss), eosstorebest, eosdotwikibp, moonxwitheos
```

The snapshot files can be found at: 
- [Snapshot Readme](https://github.com/boscore/bos-airdrop-snapshots)
- [accounts_info_bos_snapshot.airdrop.msig.json](https://github.com/boscore/bos-airdrop-snapshots/blob/master/accounts_info_bos_snapshot.airdrop.msig.json)
- [accounts_info_bos_snapshot.airdrop.normal.csv](https://github.com/boscore/bos-airdrop-snapshots/blob/master/accounts_info_bos_snapshot.airdrop.normal.csv)

# Step 15: Validate the airdrop by community

# Step 16: Hello BOS
