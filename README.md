# Keyhole Virus Tracker Blockchain

This project implements a HyperLedger Fabric blockchain network with chaincode that manages a ledger of COVID-19 and Influenza virus lab results. The chaincode implements functions to create and retrieve test results.  

It also references a React client project and supporting API Gateway project that provides a user interface to interact with a deployed blockchain.

> This blockchain implementation (and why it's an excellent solution for the dissemination and tracking of lab data) is described in detail by this recent free white paper: [Tracking Lab Results Better With Blockchain Technology](https://keyholesoftware.com/company/creations/white-papers/blockchain-virus-tracker/).

The instructions will start a Hyperledger network locally on a Linux, Unix, MacOS, or Windows operating system, and then invoke and access the blockchain chaincode in the following ways:

##### * Start the HyperLedger Orderer, Certificate Authority (CA), and Peer Nodes; create channel "mychannel"; and install chaincode
##### * Interact with Chaincode from a ReactJS/Node Web Application or CLI and Node JavaScript Commands.
##### * Execute Chaincode and Unit tests from CLI 

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Keyhole Virus Tracker Stack Setup](#keyhole-virus-tracker-full-stack-setup)
- [Installing and Running](#installing-and-running)
- [Start the Network](#start-the-network)
- [Execute Chaincode on Network Using Network CLI](#execute-chaincode-on-network-using-network-cli)
- [NodeJS Scripts](#nodejs-scripts)

## Requirements
* [Node](https://nodejs.org/en/download/) 8.9.x to 10.x - **Note: We have seen an issue with node-gyp when using > 10.x**
* [Python](https://www.python.org/downloads/) 2.7+ (v3+ not supported) - **Note: for Windows OS Only!**
* [XCode](https://apps.apple.com/us/app/xcode/id497799835?mt=12) or type `xcode-select --install` - **Note: for OSX Only!**

----
## Keyhole Virus Tracker Full Stack Setup

Follow these steps to get a React UI and API Gateway for the blockchain installed and running locally.

#### Setup Steps
1. **-> (You are here)** Set up and run Keyhole Virus Tracker

2. Set up and run the Keyhole Fabric API Gateway:  https://github.com/hyperledger-labs/keyhole-fabric-api-gateway

    - The communication gateway to the Keyhole Virus Tracker runtime

3. Set up and run the React UI:  https://github.com/in-the-keyhole/keyhole-virus-tracker-ui

    - A website containing a map displaying the locations and concentrations of reported virus samples


#### Optional Steps:
4. Byzantine Browser:  https://github.com/in-the-keyhole/byzantine-browser

    - A open source analytics tool showing real-time visibility into transactions, metadata, and blocks as they are added to a Hyperledger Fabric network
-----

# Installing and Running 

Prerequisite: Docker must be installed.

* Clone repo 

* Open terminal and execute shell script below to install Hyperledger Docker images. 

```
$ ./fabric-preload.sh
```
* Verify Docker image installation(s). 

```sh
| => docker images | grep hyperledger
hyperledger/fabric-ca           1.2.0           66cc132bd09c    2 months ago    252MB
hyperledger/fabric-ca           latest          66cc132bd09c    2 months ago    252MB
hyperledger/fabric-tools        1.2.0           379602873003    2 months ago    1.51GB
hyperledger/fabric-tools        latest          379602873003    2 monthsago     1.51GB
hyperledger/fabric-ccenv        1.2.0           6acf31e2d9a4    2 months ago    1.43GB
hyperledger/fabric-ccenv        latest          6acf31e2d9a4    2 months ago    1.43GB
hyperledger/fabric-orderer      1.2.0           4baf7789a8ec    2 months ago    152MB
hyperledger/fabric-orderer      latest          4baf7789a8ec    2 months ago    152MB
hyperledger/fabric-peer         1.2.0           82c262e65984    2 months ago    159MB
hyperledger/fabric-peer         latest          82c262e65984    2 months ago    159MB
hyperledger/fabric-couchdb      latest          3092eca241fc    2 months ago    1.61GB
```
* Install NPM modules - execute the following command in this directory to download and install all of the required npm modules (per the package.json file):

```
$ npm install
```

# Start the network
```
$ ./start.sh

```
This starts the Hyperledger Fabric network, creates a channel, installs chaincode, and executes the chaincode to verify the network is up and running.

Verify that that output ends with something similar to:
```
...
...
2019-10-28 20:56:37.490 UTC [chaincodeCmd] chaincodeInvokeOrQuery -> INFO 04f Chaincode invoke successful. result: status:200
```

If the script fails due to firewall blocking the ports: 


> Add a firewall rule to allow incoming connection on ports 139 and 445 from IP 10.0.75.0 Subnet 255.255.255.0 (or whatever your Docker config says)


# Execute Chaincode on Network Using Network CLI

* Execute the `queryAllLabs` chaincode function with the following shell script:

```
    $./sh_scripts/executeQueryLabs.sh 
```
This is a canned query. Verify the output ends with something similar to 
```
[chaincodeCmd] chaincodeInvokeOrQuery -> DEBU 0a8 ESCC invoke result: version:1 response:<status:200 payload:"
```

* Execute the `createLab` chaincode function with the following shell script:

```
    $./sh_scripts/executeCreateLab.sh 
```
verify the output ends  with something similar to 

[chaincodeCmd] chaincodeInvokeOrQuery -> DEBU 04f ESCC invoke result: version:1 response:<status:200 payload:"Lab Created"

If the network is not up, start it

```
    $ ./start.sh 
```

To run the UI, follow the setup steps in https://github.com/in-the-keyhole/keyhole-virus-tracker-ui

# NodeJS Scripts 

* There are also Node.js scripts defined in the `nodejs_scripts` folder. These scripts will use the fabric-node-sdk to invoke chaincode. You can create, retrieve, and change states of labs with the scripts. 

Here's how to create a lab in the Influenza or COVID-19 lab Channels script can be run 

```
    $ node createLab.js
```

Querying all labs for a channel can be done with this script 

```
    $ node queryAllLabs.js
```

A single lab can be queried by specifying a lab UUID with this script 

```
    $ node queryById 4634250f-e5ac-d7f0-10d0-813ef6282792
```

Change status for a lab to recovered 
```
    $ node recovered 4634250f-e5ac-d7f0-10d0-813ef6282792
```


# Compiling and Unit Testing Go Chaincode with the Development CLI 

Chaincode is implemented using the Go language, which is what Hyperledger is built with. Here's how a Docker-based Go development environment can be started with chaincode that was developed and tested outside of Hyperledger. 

Chaincode (i.e. Go implementation) can be found at this location: `chaincodes/lab/lab.go`.

* Stop and remove running Hyperledger Docker instances with following commands:

```
    $ docker stop $(docker ps -a -q) 
    $ docker rm $(docker ps -a -q) 
```

* Change to the `dev` directory and run the following Docker command:

```
$  docker-compose -f docker-compose-go.yaml up
```

* Open new terminal window, navigate to khs-lab-results-blockchain, and issue the following command:

```
$ docker exec -it chaincode bash
```

* Execute unit tests for `labs.go` chaincode bash with the following commands:

```
root@baf0e90e82a6:/opt/gopath/src/chaincode# cd lab
root@baf0e90e82a6:/opt/gopath/src/chaincode# go test 
```

* To compile and build the `labs.go` chaincode, issue the following commands:

```
root@baf0e90e82a6:/opt/gopath/src/chaincode# go build
```
