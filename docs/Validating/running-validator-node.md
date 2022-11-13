---
sidebar_position: 4
---

# Run a validator node

## Using AWS Image

Please replace the port according to the network. Below is the table mentioning ports of each network.

| Network | PORT |
| ------ | ------ |
| devnet | 8420
| testnet| 8421
| mainnet | 8456

A Newrl node can be instantly spawned using the pre-built Newrl Image on AWS. 
Please note that for testnet image name is : `Newrl Testnet Node - Ubuntu`. 
At the launch time, the only configuration that needs to be specified is the security group to allow incoming connections on ports, refer table above on which port to open.

1. In your aws console instances section click `Launch Instance`. (Change region to N.Virginia or Ohio as image is currently available in those)
2. In the launch instance page, Under application and OS images section click `Browse more AMIs`
3. In search bar, type in `newrl testnet node`. 
4. Under community AMI section, there should be an image under the name 
Newrl Testnet Node - Ubuntu. Select the image
![AWSNewrl](/img/screenshot_aws_testnet.png)
5. Under network setting, make sure ssh, http, https traffic radio box is checked.
6. Launch the instance.
7. Once the instance is launched, Go to the instance page, under security tab click security group. Then add an inbound rule with port 8421 open from any source.  



### Add Node wallet to chain
Once the node is live, it's generated wallet key can be obtained by running the below command. 
```bash
cd newrl
python3 scripts/show_wallet.py
```
![ShowWallet](/img/show_wallet.png)

The node's wallet need to be added on the chain with the help of a custodian or as a linked wallet from [Newrl wallet app](https://wallet.newrl.net).You can log into the wallet with a password specific to your machine/mobile. Inside, you can load your wallet from the node as obtained above (use only the json mentioned after “wallet” in the .auth.json file).

You can get your node’s public info from the URL /get-node-info. Example: If node’s public IP is 54.91.131.211, the url will be http://54.91.131.211:8421/get-node-info. 


## From source code
### Prerequisites 
1. A computer accessible outside with port open for incoming.
    1. To start a node on AWS, launch an EC2 instance from instructions [here](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html). Make sure to make port 8456 open to public when configuring the security group. 
    2. To start a node on Digital ocean droplet, instructions [here](https://docs.digitalocean.com/products/droplets/quickstart/)
3. Git installed. Steps [here](https://git-scm.com/downloads)
4. Python3.7+ installed. This is preinstalled on most common linux distributions. On ubuntu the steps will be `sudo apt update && sudo apt install python3.10-venv`
6. Pip and python3 venv installed. Installation steps available [here](https://pip.pypa.io/en/stable/installation/)

### Installation
Note: Below are sample commands for testnet. Replace the keyword 'testnet' with desired network name if needed.
```bash
git clone https://github.com/newrlfoundation/newrl.git
cd newrl
scripts/install.sh testnet
```

### Start the node
```bash
screen -S newrl
scripts/start.sh testnet
```
Screen session is used to let the node run in background when terminal is closed. 

### Import existing wallet
```
python3 scripts/import_wallet.py 
Enter environment[mainnet/testnet/devnet]: mainnet
```
Paste the existing wallet.

Note: The port 8456 for mainnet and 8421 if testnet should be publicly accessible on the instance. On AWS, this can be done by opening inbound port in the security group. For local installations, the router need to be configured to bypass symmetric NAT for the port 8456 for mainnet and 8421 for testnet.

### Update the Node software

#### Nodes created using AWS image

Nodes created using AWS image can be update by mearely rebooting the instance from AWS console.


### Nodes created from source

Follow the steps to update nodes created from source


1. Login to your VPS
2. Check Your NEWRL screen number and screen name
```screen -list ```
3. Kill your NEWRL screen
```screen -S (screen_number.screen_name) -X kill```
4. Create new screen
```screen -S newrl```
5. Pull & Update NEWRL nodes
```
cd newrl-venv
source newrl-venv/bin/activate
cd newrl
git pull
scripts/start.sh mainnet
```
