## What is Tacoin?
Tacoin is an unstructured, spaghetti token created on the JeChain network using Jelscript. It doesn't follow any standards whatsoever, and exist just to be a fun example.

## The code
You can check out the file `tacoin.jel`, but here it is basically:
```
pull contract_balance, insert_contract_address_here
equ contract_balance, 0
jump $contract_balance, release_token
jump 1, transfer

label release_token
	store insert_contract_address_here, 297297297

label transfer
	address sender_address
	pull sender_balance, $sender_address
	lss sender_balance, %0
	jump $sender_balance, eof

	pull receiver_balance, %1
	pull sender_balance, $sender_address
	add receiver_balance, %0
	sub sender_balance, %0

	store $sender_address, $sender_balance
	store %1, $receiver_balance

label eof
```

### Explanation
It will get the token balance of the contract, if it's equal to 0, the contract will jump to the `release_token` label and proceeds the execution, if not, it will jump straight to `transfer`.

With `release_token`, you can see that I have stored `297297297` into the contract address, which means the initial total supply is 297297297 Tacoins held by the contract.

With `transfer`, you can see that the contract checked if sender's balance is less than the amount they send or not. If yes, it will quit executing by simply jumping to `eof` which  is the end of the program. Or else, it will get balance of sender and receiver and change them.

## Deploy
Setup a JeChain node, and then paste this code at the bottom of `jenode.js`:
```js
const contract = `SC
pull contract_balance, insert_contract_address_here
equ contract_balance, 0
jump $contract_balance, release_token
jump 1, transfer

label release_token
	store insert_contract_address_here, 297297297

label transfer
	address sender_address
	pull sender_balance, $sender_address
	lss sender_balance, %0
	jump $sender_balance, eof

	pull receiver_balance, %1
	pull sender_balance, $sender_address
	add receiver_balance, %0
	sub sender_balance, %0

	store $sender_address, $sender_balance
	store %1, $receiver_balance

label eof
`;

const deployContract = new Transaction(publicKey, contract, 0, 0);

deployContract.sign(keyPair);

sendTransaction(deployContract);
```

### Deploy in mainnet/testnet
JeChain currently doesn't have a public community to have a mainnet/testnet, but if you happen to find one, just simply connect to their nodes and deploy the contract.

### Deploy in testing environment
Just don't connect to any nodes and run the node in localhost :D.
