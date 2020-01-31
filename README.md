### Tondev setup
Setup ton dev environment in Ubuntu


#### Install docker
Docker  >= 19.x installed

> https://docs.docker.com/install/linux/docker-ce/ubuntu/
  



#### Install nodejs
Node.js >= 10.x installed

> https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04

#### Install tondev
Set environment before install

```javascript
export PATH=~/.npm-global/bin:$PATH
export NPM_CONFIG_PREFIX=~/.npm-global

```
Run commands
```
npm install -g ton-dev-cli


tondev setup

```

#### Sanity test with node.js application

https://docs.ton.dev/86757ecb2/p/665008

##### Create hello.sol file

```javascript

pragma solidity ^0.5.4;

contract HelloTON {
    uint32 timestamp;
  

 // Modifier that allows public function to accept all external calls. 
    modifier alwaysAccept {
      tvm_accept();
      _;
    }
// Runtime function that allows contract to process inbound messages spending 
// it's own resources (it's necessary if contract should process all inbound messages,
// not only those that carry value with them).
    function tvm_accept() private pure {}

    constructor() public {
        timestamp = uint32(now);
    }
//Function setting set value to state variable timestamp
    function touch() external alwaysAccept {
        timestamp = uint32(now);
    }
//Function returns value of state variable timestamp
    function sayHello() external view returns (uint32) {
        return timestamp;
    }
}

```

##### Run command

```javascript

tondev sol hello -l js -L deploy

npm install --save ton-client-node-js@latest

```

##### Create example1.js with

```javascript

const { TONClient } = require('ton-client-node-js');

async function main(client) {
}

(async () => {
    try {
        const client = new TONClient();
        client.config.setData({
            servers: ['http://0.0.0.0']
        });
        await client.setup();
        await main(client);
        console.log('Hello TON Done');
    process.exit(0);
    } catch (error) {
        console.error(error);
    }
})();


```

##### Create example2.js with

```javascript
const { TONClient } = require('ton-client-node-js');
HelloContract = require('./helloContract');

async function main(client) {
 const helloKeys = await client.crypto.ed25519Keypair();
    const helloAddress = (await client.contracts.deploy({
        package: HelloContract.package,
        constructorParams: {},
        keyPair: helloKeys,
    })).address;
    console.log(`Hello contract was deployed at address: ${helloAddress}`);
}

(async () => {
    try {
        const client = new TONClient();
        client.config.setData({
            servers: ['http://0.0.0.0']
        });
        await client.setup();
        await main(client);
        console.log('Hello TON Done');
    process.exit(0);
    } catch (error) {
        console.error(error);
    }
})();
```

This example gives error. This is solved in later examples

#### Link to examples     

- [Example1](https://github.com/tonazaar/tonbasic#example1)
- [Example2](https://github.com/tonazaar/tonbasic#example2)
- [Example3](https://github.com/tonazaar/tonbasic#example3)
- [Example4](https://github.com/tonazaar/tonbasic#example4)
- [Example5](https://github.com/tonazaar/tonbasic#example5)
- [Example6](https://github.com/tonazaar/tonbasic#example6)




