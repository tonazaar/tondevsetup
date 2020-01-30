# tondevsetup
Setup ton dev environment in Ubuntu


# Install docker
Docker  >= 19.x installed

# Install nodejs
Node.js >= 10.x installed

# Install tondev
Set environment before install

export PATH=~/.npm-global/bin:$PATH
export NPM_CONFIG_PREFIX=~/.npm-global


tondev setup


# Sanity test with node.js application

https://docs.ton.dev/86757ecb2/p/665008

Create hello.sol file

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


Run command
tondev sol hello -l js -L deploy

npm install --save ton-client-node-js@latest

Create example1.js with

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



Create example2.js with

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

This example gives error. This is solved in example3.js

# Running the contract
runLocal
run

# Using helper function

Creating helper function

Using helper functions

  const hello = new HelloContract(client, helloAddress, helloKeys);
 await hello.deploy();
         console.log(hello.address, hello.keys);
 var response = await hello.sayHello();
 console.log(response);
// const response = await hello.runLocal('sayHello', {});
 response = await hello.run('sayHello', {});
 console.log(response);



# Sanity test with javsacript application


# Sanity test with React native application





