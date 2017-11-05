# solidity-tutorial
Initial setup to write your first ReactJS D-apps

## Required installation before using
- NodeJS >v8.0

## To run this D-app
```
yarn install
yarn start
```

The app will run on http://localhost:3000


## Interact with Ethereum Solidity
```
  npm install --save web3@0.15.3 (yarn add web3@0.15.3)
  npm install --save ethers-wallet (yarn add ethers-wallet)
  yarn start
```

- Write your smart contract at https://remix.ethereum.org/

## How Smart Contract looks like
```
contract SimpleCounter {
  // declare variables (must be less than 16 in total)
  uint256 counter;
  address owner;
  uint256 etherBalance; (Must record ether balance in uint256)
  
  // modifier
  modifier isOwner() {
    require(owner === msg.sender);
    _;
  }
  
  // event (the same as console.log in Javascript or System.println in Java)
  event Increment(uint256 _newCounter);
 
  // create instance of SimpleCounter
  function SimpleCounter(uint256 arg1, string arg2) public {
    counter = 0;
    owner = msg.sender // address of an Ethereum Wallet or a contract address
  }
  
  // modify data based on certain condition (without using modifier)
  function increment() public {
    if (msg.sender === owner) {
      counter += 1;
      etherBalance += msg.value; // msg.value is the amount of ether specified by sender
      Increment(counter);
    }
  }
  
  // modify data based on certain condition (using modifier)
  function specIncrement() public isOwner {
    counter += 1;
    etherBalance += msg.value; // msg.value is the amount of ether specified by sender
    Increment(counter);
  }
  
  // getter function
  function getCounter() public constant returns (uint256) {
    return counter;
  }
}
```

## Interact with Smart Contract via ReactJS/Client-side web application
Create new smart contract (Assuming you are using Metamask)
```
import Wallet from 'ethers-wallet/wallet';
const web3 = window.web3 (Metamask inject web3 inside the browser, alternative: import web3 from 'web3')
const scAbi = []; // In live lecture, Viet will show how to get this scAbi from remix.ethereum.org
const scBytecode = ''; // In live lecture, Viet will show how to get this scAbi from remix.ethereum.org

const contract = web3.eth.contract(scAbi);
const createScData = contract.new.getData(
 arg1,
 arg2,
 {
  data: scBytecode,
  gas: 4700000
 }
);

web3.eth.getTransactionCount(<your-wallet-address>, 'pending', (err1, nonce) => {
 web3.eth.getGasPrice((err2, gasPrice => {
  const txObj = {
   nonce: web3.toHex(nonce),
   gasLimit: web3.toHex(4700000),
   gasPrice: web3.toHex(gasPrice),
   data: createScData
  }
  
  const signedTransaction = wallet.sign(txObj);
  
  web3.eth.sendRawTransaction(signedTransaction, (err3, txnHash) => {
   console.log('txnHash to paste on Ropsten Etherscan', txnHash);
  })
 })
})

```
## Q&A (Will constantly be updated)
