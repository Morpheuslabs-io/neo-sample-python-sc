# Python Smart Contract sample

This is a simple Python smart contract sample that is mainly to show the steps of compile. deploy and test a Python smart contract on a Neo private network on Morpheus Labs Blockchain Platform as a Service.

## Setup Env
---------------------------------
1. Create private net on "Blockchain Ops"
2. Create a new workspace using a NEO Stack for developing, compiling and tesing the Python smart contracts.
3. Open the workspace
4. Get code from github `git clone https://github.com/Morpheuslabs-io/neo-sample-python-sc`
5. Next we need to config `neo-python` wallet to point to private net 
Open information tab of blockchain ops and get the two links:
- `Internal RPC URL: http://bops-t.morpheuslabs.io:33362 (example)`
- `Internal P2P URL: http://bops-t.morpheuslabs.io:21660 (example)`
6. Using editor like vim open and change file: `/home/user/.local/lib/python3.6/site-packages/neo/data/protocol.privnet.json`
Update 2 parameters SeedList and RPCList (example): 
```
"SeedList": [
    "bops-t.morpheuslabs.io:21660" //using Internal P2P URL without http
],
"RPCList": [
    "http://bops-t.morpheuslabs.io:33362" //using Internal RPC URL with full link
]
```
7. Open terminal and enter command `np-prompt -p -v` to connnect to private net
You should get something like this:

```
Privatenet useragent '/Neo:2.10.1/', nonce: 977855951
[I 190616 15:25:54 LevelDBBlockchain:112] Created Blockchain DB at /home/user/.neopython/Chains/privnet
[I 190616 15:25:55 NotificationDB:73] Created Notification DB At /home/user/.neopython/Chains/privnet_notif
NEO cli. Type 'help' to get started


neo>
```

It meant we connected to private net, wait little bit for neo-python node to sync, you can check syncing status by using command `show state`.

8. Enter `neo> wallet create ./mywallet` and fill in password to create new wallet. Or open wallet if you already have wallet `neo> wallet open {path to wallet file}`

9. Import the default account of the private network to the wallet `neo> wallet import wif KxDgvEKzgSBPPfuVfw67oPQBSjidEiqTHURKSDL1R7yGaGYAeYnr`

This default account has NEO and GAS tokens for you to perform various transactions on the private network.

10. Rebuild wallet after import `neo> wallet rebuild`
11. Check NEO and GAS in wallet `neo> wallet` focus to check `percent_synced` and `synced_balances`
```
    "percent_synced": 100,
    "synced_balances": [
        "[NEO]: 100000000.0 ",
        "[NEOGas]: 30525.98755 "
    ],
```
Wallet need to have NEO and GAS to able deploy contract. Next we start to compile and deploy NEP contract to private network.

# Compile & Deploy smart contract to private network
---------------
1. To build contract use command `neo> sc build {path to python file}` example:
`neo> sc build /projects/neo-sample-python-sc/smart-contract/sample.py`
2. To deploy contract use command `neo>sc deploy {path to .avm file} True False False 0701 05 --fee={fee}`
And Enter Contract Information detail to deploy.
example: `neo> sc deploy /projects/neo-sample-python-sc/smart-contract/sample.avm False False False 070202 02`
3. When finish to deploy wait little and view detail contract to get contract address to config on FrontEnd
Example:
```
Creating smart contract....
                 Name: Sample
              Version: V1
               Author: Neo
                Email: neo@test.com
          Description: Sample App
        Needs Storage: False
 Needs Dynamic Invoke: False
           Is Payable: False
{
    "hash": "0xb6730fd741b632401f89020409c6c0415d97dcee", //script hash of contract 
    "script": "011ac56b6a00527ac46a51527ac4586a52527ac4074e4744205045546a53527ac4034e50546a54527ac46a00c30b746f74616c537570706c79876406006c7566616a00c3046e616d65876409006a53c36c7566616a00c30673796d626f6c8764...
```
4. Test invoke smart contract
```
neo> sc invoke {script_hash} {paramters}
```
Example:
```
neo> sc invoke 0x1d36641faca64ddd0f49af488e543a0f89860690 add 02 03
```
Test invoke successful
Total operations: 39
Results [{'type': 'Integer', 'value': '5'}]
Invoke TX GAS cost: 0.0
Invoke TX fee: 0.0001

You can see result in Results field, Enter incorrect password to prevent to submit transaction network
