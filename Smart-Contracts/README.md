# Smart Contracts Cheat Sheet

## General
This is a short cheat sheet about smart contract development, intended for people who are already familiar with solidity. I hope it helps to prevent mistakes. In the last sections you can find code snippets for basic functionality (like the withdraw function etc.), so that you do not have to type these again and again for every new contract.  
All the information is presented in this ReadMe. Just hit CTRL + F to quickly find what you are looking for. In the sub-directories I will try to add examples about what not to do and best practices.  

This cheat sheet is meant to be constantly improving.  
In case you find any mistakes or have additional information that might be helpful write me an email or create a pull request.  

**None of the presented code and concepts here take the responsibility to be free of mistakes and complete. Always check your smart contracts in detail and ask other experienced developers for help. This is only a short summary of what I found to be useful. Use at your own risk!**

Smart Contracts security best practices: https://consensys.github.io/smart-contract-best-practices/  
Smart Contract weakness classcification and test cases: https://swcregistry.io

---

## Geth Basics
Never forget the "await" before each call.
- Get balance:            `eth.getBalance(publicAddress);`
- Get storage value:      `eth.getStorageAt(contractAddress, index);` //for more details see chapter about storage variables
- Get abi:                `var abi = ....;` //get from your IDE after compiling the contracts
- Get contract instance:  `var instance = new.eth.Contract(abi, await addresses.getPublic(address).call);`
- call methods:           `instance.methods.methodName(parameters).send({from: myAddress, value: web3.utils.toWei('0.0025')});`
- call view functions:    `instance.methods.balanceOf(address).call();`

---

## web3 Basics
- Convert eth to toWei: `web3.utils.toWei('0.25')`
- Convert wei to eth: `web3.utils.fromWei('5000000')`
- Create new instance of smart contract: `var contractInstance = new web3.eth.Contract(abi, contractAddress)`

---

## Sending transactions

### Low level calls
- CALL: "normal" call, like a call from an external address.
- STATICCALL: like CALL, but fails if the contract tries to modify the state. Used by Solidity when calling external/public functions declared as pure/view.
- DELEGATECALL: execute someone else’s contract code in the context of the current contract instance. Used by Solidity when calling external/public functions of libraries. This call modifies the state of the contract calling it and thus msg.sender and msg.value stay the same.

```javascript
address to;
bool status;
bytes memory input;
bytes memory output;
// if no value and/or gas are specified, there will only be sent the remaining gas.
( status , output ) = to.call         {value: 1 ether, gas: 2300}(input) ;
( status , output ) = to.staticcall   {value: 1 ether, gas: 2300}(input) ;
( status , output ) = to.delegatecall {value: 1 ether, gas: 2300}(input) ;
// Check if transaction succeeds otherwise revert.
if(!status) revert();
```

How to construct the call data:
```javascript
input = "" ; // empty data for pure Ether transfers
// call the function test(address _addr, uint_i)
// with the arguments 0xD0a53F4f71C2188D10bd31efd689dfb0e7B5D526 and 420
string memory signature = "test(address, uint256)";
address receiver = address (0xD0a53F4f71C2188D10bd31efd689dfb0e7B5D526);
input = abi.encodeWithSignature(signature, receiver, 420);

bytes4 selector = bytes4(keccak256(bytes(signature)));
bytes4 selector = "\xe6\x9d\x84\x9d"; // for known selector
input = abi.encodeWithSelector(selector, receiver ,420);
```

### High level calls
- Use transfer method because of built-in error handling.
- Transfer and send protect against reentrancy attacks because of fixed 2300 gas.
- Use low level call instead if you need to adjust gas.

```javascript
address payable to;
to.transfer(1 ether);           // fixed 2,3kGas, revert on failure
bool status = to.send(1 ether); // fixed 2,3kGas, returns false
require(to.send(1 ether));      // throws exception if failure
```

### Call a function with ether
```javascript
contract A {
  function f() external payable {...};
}

contract B {
  A a = new A();
  function g() public {
    a.f{value: 1 ether}();
  }
}
```

### Let a contract receive ether
Create a **receive** or **fallback** function that is **external payable**. If there is no such function, the ether will be refused and the transaction reverted.  
Note: The default 2300 gas is only sufficient for logging an event. If you want to do more in your receive function, you need to specify gas by using call instead of transfer.  

A contract cannot refuse ether if it is:
- a mining reward.
- a destination of a selfdestruct.
- sent to the address before contract deployment.

**=> Your internal bookkeeping may vary from the actual balance. Be cautions with using address(this).balance!**

```javascript
contract A {
  receive() external payable {...}
  // fallback() external payable {...}
}

contract B {
  A a = new A();
  function g() public {
    payable(a).transfer(1 ether);
  }
}
```

---

## Storage variables / private variables and mappings
Variables declared as private can be read from storage if one knows at which position they are.

### Reading private variables
`await eth.getStorageAt(address, key)`  
!!! Each storage element in Ethereum has 256 bits. So if the first two variables have e.g. only 128 bits, they both get saved at the location with the key == 0. The 256 bits get filled up from right to left. This only applies to variables not mappings. For mappings each element always takes up the full 256 bits. For more information on how to access mappings read on.

### Reading private mappings
```javascript
var mIdx = '0'.repeat(63) + '1'; // 256 bit == 64 hex chars
var mKey = '13EAB5D3ebc09C013709ED7DAF0120125a5cc486'.padStart(64,'0');
var sKey = web3.utils.sha3('0x' + mKey + mIdx);
//mIdx.length and mKey.length are equal to 64 characters = 256 bit
web3.utils.toDecimal(await eth.getStorageAt(contractAddress, sKey)); // 20
```
---

## Reentrancy attacks
Whenever a contract calls a function of another contract, make sure that the other contract can not harm your contract by calling the method again during that function call. (somewhat similar to recursive programming)  
In the following example ContractY can exploit ContractX because accounts[msg.sender] -= amount is processed after the function call to ContractY. So the withdraw function inside of ContractYs receive() function gets called again and again until there is no ether in ContractX left. To prevent this, subtract the amount in ContractX directly after the require statement and before the function call to ContractY.  

You can use the OpenZeppling [ReentrancyGuard](https://docs.openzeppelin.com/contracts/4.x/api/security#ReentrancyGuard) to prevent reentrancy attacks.

**ContractX:**
```javascript
function withdraw (address payable target, uint256 amount) public {
    require(accounts[msg.sender] >= amount);
    (bool success, bytes memory data) = target.call{value: amount}("");
    if (!success) revert();
    accounts[msg.sender] -= amount;
}
```

**ContractY**
```javascript
receive() external payable {
    ContractX(msg.sender).withdraw(payable(address(this)), 20);
}
```
---

## Access management
OpenZeppelins [AccessControl](https://docs.openzeppelin.com/contracts/access-control) and Ownership provide a good starting point to enable Ownable contracts and as well grant granular control by using AccessControl.

**Make contract ownable:**
```javascript
import "@openzeppelin/contracts/access/Ownable.sol";

contract ERC223Token is Ownable {
    constructor(address newOwner) {
        _transferOwnership(address newOwner) //if called externally: transferOwnership(address)
    }

    function specialThing() public onlyOwner {
        // only the owner can call specialThing()!
    }
}
```

**Setting up and using roles:**  
At the beginning a DEFAULT_ADMIN_ROLE needs to be set up.  
Roles can be granted by the admin of the role by calling grantRole(role, account).
```javascript
import "@openzeppelin/contracts/access/AccessControl.sol";

contract ERC223Token is AccessControl {

    bytes32 public constant MINTER_ROLE = keccak256("MINTER_ROLE");
    constructor() {
        _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
    }

    function mint(address to, uint256 amount) public onlyRole(MINTER_ROLE) {
        _mint(to, amount);
    }
}
```

---

## Deployment

### Extcodesize
When extcodesize(msg.sender) is called from within a constructor of another contract during deployment, it always returns 0. Therefore this is not a good method to prove whether msg.sender is a contract or an address. A simple way is require(msg.sender == tx.origin). However, preventing a contract is an anti-pattern with security and interoperability considerations. This will need revisiting when account abstraction is implemented.

### Constructor inheriting another contract
```javascript
contract MyToken is ERC223Token {
    constructor(string tokenName, string tokenSymbol, uint256 decimals) ERC223Token(tokenName, tokenSymbol, decimals) {...}
}
```

### Truffle deploy with data from other contracts
```javascript
deployer.deploy(DAOToken).then(() => {
    return deployer.deploy(ICO_DAOToken, DAOToken.address).then(() => {
        var daoTokenInstance = await DAOToken.deployed();
        await daoTokenInstance.grantRole(keccak256("OWNER"), ICO_DAOToken.address);
    })
})
```

### Deploy contract with ether
Make the constructor **payable** if it is deployed with ether.
```javascript
contract A {
  constructor () payable {...}
}

contract B {
  A a = new A{value: 1 ether}();
}
```

---

## Basic code snippets to implement

### Call contract from other contract with interface
```javascript
interface exampleInterface {
    function interfaceFunction() external;
}

contract exampleContract {

    exampleInterface a = exampleInterface(0x6E522A1Df4D0AB756169B905933184e4b80Ed9e1);  // Address of the contract you want to call

    function exampleFunction() public {
        a.interfaceFunction();
    }
}
```

### Basic withdraw function
```javascript
address payable owner;

constructor() payable {
    owner = payable(msg.sender);
}

modifier isOwner(){
    if (msg.sender != owner) revert();
    _;
}

function withdraw() public isOwner {
    (bool success, ) = msg.sender.call{value:address(this).balance}("");
    if (success == false) revert();
}
```

### Basic receive function
```javascript
event Received(address, uint);
receive() external payable {
    emit Received(msg.sender, msg.value);
}
```
