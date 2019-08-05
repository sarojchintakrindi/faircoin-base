

#Faircoin base cryptocurrency

## Requirements
- `golang`	>1.12.1 installed
- Appropriate `$GOPATH$`
Follow the [instructions here](https://golang.org/doc/install). Make sure that this repository is in your `$GOPATH`! 

In addition, add the following parameters to your environment (just run these commands). 

```
mkdir -p $HOME/go/bin
echo "export GOPATH=$HOME/go" >> ~/.bash_profile
echo "export GOBIN=\$GOPATH/bin" >> ~/.bash_profile
echo "export PATH=\$PATH:\$GOBIN" >> ~/.bash_profile
echo "export GO111MODULE=on" >> ~/.bash_profile
source ~/.bash_profile
```

To install the application, run 

```
make install
```

## Initializing the chain
```
// initialize node
nsd init <node_name_here> --chain-id fairchain

//generate keys
//Save the addresses!
nscli keys add <Alice>

nscli keys add <Bob>

// Add accounts to genesis file with coins

nsd add-genesis-account $(nscli keys show alice -a) 1000faircoin,100000000stake
nsd add-genesis-account $(nscli keys show bob -a) 1000faircoin,100000000stake

// Configuration for ease and clarity
nscli config chain-id fairchain
nscli config output json
nscli config indent true
nscli config trust-node true

//generate genesis transaction
nsd gentx --name alice

//input genesis transaction into genesis file
nsd collect-gentxs

//validate genesis file
nsd validate-genesis

//start chain
nsd start
```

## Do things
```
//Query account information
nscli query account $(nscli keys show Alice -a)

//Send coins
nscli tx send [Sender's address] [Recipient's address] [amount|token]

//ex. nscli tx send cosmos16pknf5p5ytzzy5hafj6hemvygrapfjz8wd6tu0 cosmos1uhk26zw3ldxxrg6wrxw65cevrqe36fws8e8fks 500faircoin

//Query again to verify 

```
## Initialize as a second node
I've configured this repo to connect to my (saroj's) IP address, so an error is possible if I'm offline. 
You'll still have to replace the genesis file yourself, as it's stored outside of the repo. 
- Make install as usual, and initialize second node with different moniker and same chain-id
- Replace ~/.nsd/config/genesis.json with genesis file from first node 
- Change persistent peers 
