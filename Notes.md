### Ethereum

ETH => DAO's Decentralized Autonomous Organisations

SmartContracts => Piece of code on Blockchain
	
`Web3js` used to communicate to the ETH Network, 
Metamask is used by consumers

Types1 of Network
- Mainet -> The main ETH chain
- Test Network -> Used to test our contracts
- Local Network -> Locally running networks used also for testing
- RPC -> Chains running in some remote location

#### Metamask 

Provides us with a Account number (kinda like your email id that idenfies you but can be used across networks), Public key,  private key.

12 word Mnemonics -> instead of remembering the private, public keys of your accounts. We can remember a 12 unique words that would inturn get us our accont details.

This is used BTS to generate accounts for use. This is the BIP39 Algo.

--- 

Ethereum Facuet => These are services that give you free ETH to test your Dapps on the test network, Each test network has its own Faucet.

Eth scan -> Can enter the transaction hash here to check the transaction in realtime while its being mined.

#### Transactions
They make up the whole story of the block chain.

They consist of 
- `nounce` -> increasing count of the number of transaction sent by the user
- `to` -> address of the user that this is being sent to
- `value` -> ETH to be sent along
- `gasPrice` -> gas to process the transaction
- `gasLimit` -> Max ETH that can be used as gas to process the transaction
- `v,r,s` -> They are derived from the sender's private key and can be used to check the authenticity fo the transaction

`SHA256` -> Hashing function used.
SHA256( Block# + Nounce + data) -> This is the expresssion that is solved to obtain the nounce of some specific type like say starts with 4 zeros. 

The distributed nature of the chain, and its hash depending on the previous block's hash make the chain resist change.

`data` in the block can be used to store programs, or transactions of money etc. Using this we can also see our programs on the block after its being mined.

According to the mining time (around 15s for a block in ETH) the target is reduced or increased to add dificulty.

#### Smart Contracts
This is just another account controlled solely by the code written for it. Therefore these are also known as external Accounts.

`balance`, `storage`, `code` are the extra things that this has from a normal transaction. 
`to` is empty.
`value` is amount in Wei. ((10^18) Wei is 1 ether)

Think of `contract-accounts` like classes that can be deployed(instantiated) across different networks as `contract-instances`.

#### Solidity
- strongly typed language
- similar to JS syntax
- `.sol` extension

![[Solidity Contract Compilation Overview.png]]

[Online Editor to get started](https://remix.ethereum.org/)
This comes with an solidity compiler, inmemory blockchain to deploy to and other extensions. 
Select `env` as `JS VM` to access the same.
Select `web3` to interact with already deployed contracts by providing address of the same.

```js
pragma solidity ^0.8.0;
contract Inbox{
	// properties of the contract that will be stored with it
	string public msg;
	
	constructor(strin init){
		msg = init;
	}
	
	// function signature taking args 
	fucntion setMesage(string newMessage) public{
		msg = newMessage;
	}
	
	// fucntion returning a value
	fucntion getMessage() public view returns(string){
		return msg;
	}
}
```

`pragma` defines what version of the compiler should be used to compile this contract

`contract` defines the struture of the contract. More like a class that will define the structure.

`constructor` runs during the instantiation of the contract. 

Function Types
- `public` => This can be accessed by any contract from anyhwere. These variables get a getter by default without us providing one.
- `private` => Can be accessed by only within the contract and nowhere else. Not even inheriting  contracts
- `view` => Fucntion returns variables. Does not modify it.
- `pure` => Function does not modify nor interact with variables. Kinda like an helper if you think?
- `payable` => This function accepts payments to run.

`Calling` a function is 
- not modifying any data.
- Runs in sync
- Free 

`Sending` a transaction
- will cost ETH
- takes time to process as has to be mined.
- returns a Transaction Hash.

#### Denominations 
`Wei` `gwei` `fenny` are also other denomination that `ether` can be represented as

#### Gas Prices
`gasPrice` => Amount of wei per unit gas to get transaction processed
`gasLimit` => Max gas that can be used for the transaction. If it does cross this then the transaction would revert.

ex. Say `+` operation causes 3 units and our `gasPrice` is set to 10. Then this operation would cost us total of `10*3 = 30` gas.

Storing data on the chain would also cost us gas as its transaction too that is being sent to the chain.

---
#### Infrastucture
- Truffle -> Contract creation, local testing, deployment,
- Solc compiler -> Compiles contract
- Mocha, Chai -> Test runner and asserstions

---
#### Solc compiler

`npm i solc`
[Docs on how to use](https://www.npmjs.com/package/solc)

This takes our solidity code and compiles it down to ABI  as `interface` and byte code as `bytecode` that is used to interact with the deployed contract on the chain and code that is to be deployed onto the chain respectively.

---
#### Ganache cli
This deploys a local blockcahin for dev purposes. 
Comes from Truffle suite. 
Has a GUI too.
Gives unlocked accounts out of the box.

---
#### Web3.js
 This takes the ABI that explains what the contract looks like in a human readable format and then interacts with the blockchain.
 
 This also get a `provider` from the blockcahin node this is interacting with.
 
 ```js
 // require all things like
 // - mocha, chai
 // - ganahce-cli
 // - web3
const web3 = new Web3(ganache.provider());

web3.eth.getAccounts()
	.then(acc => {
		acc; // First 10 accounts is returned
	});

// Deploy a new contract instance
inbox = await new web3.eth.Contract(JSON.parse(interface))
					.deploy({data: bytecode, args: })
					.send({from: acc[0], gas: });
 inbox.options.address // gives the addres of the contract
 inbox.methods.message().call() // calls the message function
 ```
 
 -> `args` takes function arguments
 
 To test these now
 
 ```js
 // beforeEach wehre all the init config is done
 it("Contract should be deployed", () => {
 	assert(inbox.options.address);
 })
 ```
 
 #### Deployment
 
 We can either run a fully local blockchain node or use services like Infura, that offer a blockchain node as a service. They further connect to the rest of the blockchain network.
 
 [Infura](https://infura.io/)
 
 `@truffle/hd-wallet-provider` is used to connect to this infura from our web3. Provide this to our web3 that will interact with the contract
 
 ```js
 const provider = new HDWalletProvider({
 	"MNEMONIC STRING",
	"INFURA ENDPOINT"
 })
 web3 = new Web3(provider);
 ```
 
 