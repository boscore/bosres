# The BOS Genesis
```
# Step 1: Prepare config.ini and genesis.json by start a node with eosio

nodeos --config-dir /data/eos-config -d /data/eos-data --genesis-json /data/eos-config/genesis.json


# Step 2: Prepare wallet and import keys
$cleos1 wallet create 
$cleos1 wallet import --private-key <private-key>


cleos1='cleos -u http://127.0.0.1:9999'

# Step 0: The keys to the world of BOS 
EOSIO_PUB="EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop"
BOS_PUB="EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop"

# Schema: account ownerkey activekey signkey
BOS_KEYS=("bos" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop")  
ABP_KEYS=("bos.abp" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop" "EOS7hHHDtnPRbhMmfHJHUEKQyiutKrt9wZPdy1JbaATVLyxpCkrop")  

# the Core Teams' accounts and keys
MERCURY_KEYS=("mercury" "EOS8Ybz56pR5b4Rq2nvJ3ZT9CehG78pXEYdwQ9LHKPm4ZFuwM777W" "EOS8Ybz56pR5b4Rq2nvJ3ZT9CehG78pXEYdwQ9LHKPm4ZFuwM777W")
VEUNS_KEYS=("veuns" "EOS8iN2NZR29sKjfgraREWMZCTSUTuULMRiL3QmU1McCHxDBLeRcL" "EOS5xcrat34D5vkPj8vx1WRCS3Rd5PxjQbHrz7pmyLKLTsDDW91nn")
EARTH_KEYS=("earth" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT" "EOS58Mzpd2hEp5Q4MqDFBgX9EbWU6gZcCQZ7Soa9HsuBYw1zvNGxT")
MARS_KEYS=("mars" "EOS8DmeNbNDJc8wMNd7C2Upz3zDRhc6P87QHiW8EbssjKSLSGTrbs" "EOS71u9BFuat4CtxzbqzFGzacgdgDdtujpiRm95Kyw24MUuMvWUNX")
JUPITER_KEYS=("jupiter" "EOS7do6GqRAJEpcJY536zG6j3VqXMMdpMKtDCkFXzYYNCKLB73xVU" "EOS5yajrR2u7XxDcnjXE2H6WYjDabi2Z44zRrYz6pK3fZtFAyrnt5")
URANUS_KEYS=("uranus" "EOS84TUrXoKKWYdUMoTtGKqKge9oqBDKSqxKfX7oHmkxD5LbGLT2z" "EOS6bNYimeRE6QQcw5yMdMVSotZ87gX3QN5XEQTbwvsKAVVs7JTiH")
NEPTUNE_KEYS=("neptune" "EOS7YyA18xX8zdDaWGSB7d7aV5qjUoG6YE7murr2Nv7TbyS6WNSCy" "EOS68aR5GxMugpxoFnQyA3k6EXiBYFvjqXRMiTDy9HFynLxqr4LEh")



# Step 3: Set contract eosio.bios
CONTRACTS_FOLDER='../bos.contract-prebuild' 
$cleos1 set contract eosio ${CONTRACTS_FOLDER}/eosio.bios -p eosio

# Step 4: Create system accounts
SYS_ACTS=(
    "eosio.bpay" "eosio.msig" "eosio.names" "eosio.ram" "eosio.ramfee" 
    "eosio.saving" "eosio.stake" "eosio.token" "eosio.vpay" "eosio.wrap" 
    "bos.dev" "bos.gov" "bos.bhole" "bos.boost"
)
for account in ${SYS_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    $cleos1 create account eosio ${account} ${EOSIO_PUB}; 
    sleep 1; 
done

FEATURE_ACTS=(
    "redpacket" "bos.btc" "bos.eth" "bos.eos" "bos.adrop"
    "bos.op" "bos.angel" "bos.eco" "bos.bonus" "bos.dapp"
)
for account in ${FEATURE_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    $cleos1 create account eosio ${account} ${BOS_PUB}; 
    sleep 1; 
done

# Step 5: Create the BOS Core Team accounts
$cleos1 create account eosio ${BOS_KEYS[0]} ${BOS_KEYS[1]} ${BOS_KEYS[2]}
$cleos1 create account eosio ${ABP_KEYS[0]} ${ABP_KEYS[1]} ${ABP_KEYS[2]}

$cleos1 create account eosio ${MERCURY_KEYS[0]} ${MERCURY_KEYS[1]} ${MERCURY_KEYS[2]}
$cleos1 create account eosio ${VEUNS_KEYS[0]} ${VEUNS_KEYS[1]} ${VEUNS_KEYS[2]}
$cleos1 create account eosio ${EARTH_KEYS[0]} ${EARTH_KEYS[1]} ${EARTH_KEYS[2]}
$cleos1 create account eosio ${MARS_KEYS[0]} ${MARS_KEYS[1]} ${MARS_KEYS[2]}
$cleos1 create account eosio ${JUPITER_KEYS[0]} ${JUPITER_KEYS[1]} ${JUPITER_KEYS[2]}
$cleos1 create account eosio ${URANUS_KEYS[0]} ${URANUS_KEYS[1]} ${URANUS_KEYS[2]}
$cleos1 create account eosio ${NEPTUNE_KEYS[0]} ${NEPTUNE_KEYS[1]} ${NEPTUNE_KEYS[2]}

# Step 6: Assign the ABP 
$cleos1 push action eosio setprods '{"schedule":[{"producer_name":"'${ABP_KEYS[0]}'","block_signing_key":"'${ABP_KEYS[3]}'"}]}' -p eosio

# Step 7: Set token,msig,wrap contract
$cleos1 set contract eosio.token ${CONTRACTS_FOLDER}/eosio.token -p eosio.token 
$cleos1 set contract eosio.msig ${CONTRACTS_FOLDER}/eosio.msig -p eosio.msig
$cleos1 set contract eosio.wrap ${CONTRACTS_FOLDER}/eosio.wrap -x 1000 -p eosio.wrap

# Setp 8: Setting privileged account for eosio.msig
$cleos1 push action eosio setpriv '{"account": "eosio.msig", "is_priv": 1}' -p eosio

# Step 9: Create and issue token
$cleos1 push action eosio.token create '["eosio", "10000000000.0000 BOS"]' -p eosio.token 
$cleos1 push action eosio.token issue '["eosio", "1013000000.0000 BOS", "BOSCore"]' -p eosio

# Step 10: Set contract eosio.system
$cleos1 set contract eosio ${CONTRACTS_FOLDER}/eosio.system -x 1000 -p eosio
$cleos1 push action eosio init '{"version": 0, "core": "4,BOS"}' -p eosio

# Step 11: Change BOS accounts to multisig
MSIG_ACTIVE_RULE='{"threshold":5,"keys":[],"accounts":[{"permission":{"actor":"earth","permission":"active"},"weight":1},{"permission":{"actor":"jupiter","permission":"active"},"weight":1},{"permission":{"actor":"mars","permission":"active"},"weight":1},{"permission":{"actor":"mercury","permission":"active"},"weight":1},{"permission":{"actor":"neptune","permission":"active"},"weight":1},{"permission":{"actor":"uranus","permission":"active"},"weight":1},{"permission":{"actor":"veuns","permission":"active"},"weight":1}],"waits":[]}'

MSIG_OWNER_RULE='{"threshold":2,"keys":[],"accounts":[{"permission":{"actor":"mars","permission":"active"},"weight":1},{"permission":{"actor":"mercury","permission":"active"},"weight":1},{"permission":{"actor":"uranus","permission":"active"},"weight":1}],"waits":[]}'

MSIT_ACTS=("bos" "bos.op" "bos.angel" "bos.eco" "bos.bonus" "bos.dapp")
for account in ${MSIT_ACTS[*]}
do
    echo -e "\n multisig setup $account \n"; 
    $cleos1 set account permission ${account} active ${MSIG_ACTIVE_RULE} owner -p ${account}@owner
    sleep 1; 
    $cleos1 set account permission ${account} owner ${MSIG_OWNER_RULE}  -p ${account}@owner
    sleep 1; 
done

# Step 12: Allocate token 
$cleos1 transfer eosio bos        "200000000.0000 BOS" "Lock 4 years, unlock daily"
$cleos1 transfer eosio bos.op     "100000000.0000 BOS" "BOS operation"
$cleos1 transfer eosio bos.angel  "200000000.0000 BOS" "4 times of angel investment"
$cleos1 transfer eosio bos.eco    "400000000.0000 BOS" "BOS eco-cultivation"
$cleos1 transfer eosio bos.adrop  "10000000.0000 BOS"  "Airdrop for EOS directly"
$cleos1 transfer eosio bos.boost  "20000000.0000 BOS"  "For better BPs and DApp"
$cleos1 transfer eosio bos.dapp   "70000000.0000 BOS"  "Airdrop for EOS by DApp"

echo "resign all system accounts!!"
# Step 13: Resign all system accounts
for account in ${SYS_ACTS[*]}
do
    echo -e "\n creating $account \n"; 
    $cleos1 push action eosio updateauth '{"account": "'$account'", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@active
    $cleos1 push action eosio updateauth '{"account": "'$account'", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio", "permission": active}}]}}' -p ${account}@owner
    sleep 1; 
done

#resign eosio
$cleos1 push action eosio updateauth '{"account": "eosio", "permission": "active", "parent": "owner", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@active 
$cleos1 push action eosio updateauth '{"account": "eosio", "permission": "owner", "parent": "", "auth":{"threshold": 1, "keys": [], "waits": [], "accounts": [{"weight": 1, "permission": {"actor": "eosio.prods", "permission": active}}]}}' -p eosio@owner

#check again
for account in ${SYS_ACTS[*]} #eosio.bpay eosio.msig eosio.names eosio.ram eosio.ramfee eosio.saving eosio.stake eosio.token eosio.vpay  eosio.wrap bos.dev bos.gov  redpacket
do 
    echo --- ${account} --- && $cleos1 get account ${account} && sleep 1; 
done
for account in ${MSIT_ACTS[*]} #eosio.bpay eosio.msig eosio.names eosio.ram eosio.ramfee eosio.saving eosio.stake eosio.token eosio.vpay  eosio.wrap bos.dev bos.gov  redpacket
do 
    echo --- ${account} --- && $cleos1 get account ${account} && sleep 1; 
done

echo --- eosio  --- && $cleos1 get account eosio
```
# Step 14: Import the accounts from snapshot and airdrop 
The top 50 EOSMainnet BPs' accounts will be kept by replacing `eos` into `bos`.  
This account list be taken at 2019-01-10 14:30 UTC+13
eoshuobipool, starteosiobp, eoslaomaocom, zbeosbp11111, eosflytomars, eosliquideos, eosiosg11111, bitfinexeos1, atticlabeosb, jedaaaaaaaaa, cochainworld, eosnewyorkio, eoscannonchn, eosbixinboot, eos42freedom, eosbeijingbp, eoscanadacom, eoshenzhenio, eosauthority, eosriobrazil, eosnationftw, eosdacserver, eosswedenorg, teamgreymass, cypherglasss, eospaceioeos, games.eos(gamesbosssss), oraclegogogo, unlimitedeos, helloeoscnbp, eostitanprod, eoscafeblock, eosiomeetone, libertyblock, eospacificbp, cryptolions1, eosasia11111, argentinaeos, eosfishrocks, eoszhizhutop, eosyskoreabp, eosfengwocom, eosgenblockp, eosvolgabpru, alohaeosprod, eostribeprod, eos.fish(fishbossssss), eosstorebest, eosdotwikibp, moonxwitheos

# Step 15: Validate the airdrop by community

# Step 16: Hello BOS