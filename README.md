# Proof of Authority Development Chain

### Blockchain - Unit 18 

![Blockchain](https://specials-images.forbesimg.com/imageserve/5f2a32ee3b52675a453e2881/960x0.jpg?fit=scale)

**Created by: Richard Ripley**

---

**Before we begin, please download these:**
- [My Crypto](https://download.mycrypto.com)
- [Geth](https://geth.ethereum.org/downloads/) Please choose Geth & Tools version 1.9.7 for your machine.
Once downloaded, go to your downloads and create a folder to store Geth within (I chose: "Blockchain Tools") and feel free to rename Geth as well. 

## Instructions for launching a blockchain

**Step 1: The first step to launch is to create a Genesis Block**
 
 - Open a terminal window, navigate to the Blockchain-Tools folder and type the following command: ./puppeth
 - Type in a name for your network, like "puppernet" and hit enter to move forward in the wizard.
 - Type "2" to pick the Configure new genesis option, then "1" to Create new genesis from scratch
 
**Now you have the option to pick a consensus engine (algorithm) to use.**
 - Type "1" to choose Proof of Work and continue.
 - You will be asked to enter a pre-fund account: Copy and paste an address from your Ethereum wallet in MyCrypto, without the 0x prefix.
 - Once you paste an address and hit enter, hit enter again on the blank 0x address to continue the prompt.
 - Continue with the default option for the prompt that asks Should the precompile-addresses (0x1 .. 0xff) be pre-funded with 1 wei? by hitting enter again, until you reach the Chain ID prompt.
 - Come up with a number to use as a chain ID (e.g. 333) type it, then hit enter.
 
**You should see the following success message and redirected to the original prompt:"Configured New Genesis Block"**
 
**Step 2:Create two nodes with accounts**

- In the puppeth prompt, navigate to the Manage existing genesis by typing 2 and hitting enter.
- Then, type 2 again to choose the Export genesis configurations option, and continue with the default (current) directory by hitting enter. 
- This will export several yournetworkname.json files - you only need the first one without aleth, parity, or harmony suffixes.

**Now, we need to create at least two nodes to build the chain from the genesis block onward**
- Exit puppeth by using the Ctrl+C keys combination.
- Create the first node's data directory using the geth command and a couple of command line flags by running the following line in your terminal window (Git Bash in Windows): ./geth account new --datadir node1
- Create a new text file for notes, and copy the node's address into the file and label it Node 1 Key.
- Repeat the same process for the second node by replacing the datadir parameter with the node2 folder: ./geth account new --datadir node2

**Now, it's time to initialize and tell the nodes to use your genesis block**
- Initialize the first node, replacing yournetworkname.json with your own: ./geth init yournetworkname.json --datadir node1
- Run the same command for node2: ./geth init yournetworkname.json --datadir node2

**Step 3: Start both Nodes** 
- In your notes text file, make sure to keep track of every command you run in this activity for later. You can use these notes as a cheat-sheet later to easily start your chain again.

- Launch the first node into mining mode with the following command: ./geth --datadir node1 --mine --minerthreads 1
- You should see the node Committing new mining work: Copy this command into your notes and label it Start Node 1.

**Now you will launch the second node and configure it to let us talk to the chain via RPC**
- Scroll up in the terminal window where node1 is running, and copy the entire enode:// address (including the last @address:port segment) of the first node located in the Started P2P Networking line 8 (*We will need this address to tell the second node where to find the first node.*)
- Open another terminal window and navigate to the same directory as before.
- Launch the second node, enable RPC, change the sync port, and pass the enode:// address of the first node in quotes by running the following command (it will differ in Windows and OS X):
     - Running in OS X:

        ./geth --datadir node2 --port 30304 --rpc --bootnodes "enode://<replace with node1 enode address>"
     - Running in Microsoft Windows:

        ./geth --datadir node2 --port 30304 --rpc --bootnodes "enode://<replace with node1 enode address>" --ipcdisable
        
             - The output of the second node should show information about Importing block segments and synchronization - Copy this command into your notes and call it Start Node 2

**Step 4:Transacting on your chain**

- Open up MyCrypto to get the private key of the ETH address you use to pre-fund your chain. Be sure the Kovan network is selected.
- Unlock your wallet using your mnemonic phrase and choose the address you want to inspect.
- Select the ETH address you use to pre-fund your chain, and in the "Select" dropdown list, choose Wallet Info.
- Click on the eye icon next to the Private Key field, and copy and paste the private key of the wallet. Keep this handy, as you will use it in a bit.

**Now you are going to connect MyCrypto with the blockchain you created. Follow the next steps.**
- Open up MyCrypto, then click Change Network at the bottom left
- Click "Add Custom Node", then add the custom network information that you set in the genesis.
- Make sure that you scroll down to choose Custom in the "Network" column to reveal more options like Chain ID: The chain ID must match what you came up with earlier (333)
- The URL is pointing to the default RPC port on your local machine. Use http://127.0.0.1:8545.
- Once you save and use the network, double-check that it is selected and is connected.

**Now that you are connected to your blockchain, you will need to load a private key that you created and funded on the network.**
- If you are logged in to another wallet, you'll need to click Change Wallet on the top right, but make sure you are connected to your custom network.
- On the left pane menu, click on "View & Send".
- Next, click on the "Private Key" option to continue.
- A new window will pop-up, paste the private key of the pre-fund wallet and click on the "Unlock" button to continue.
- Look to the right, This is the account balance that was pre-funded for this account in the genesis configuration; however, these millions of ETH tokens are just for testing purposes.

**Now we're going to send a transaction to ourselves to test it out. Follow the next steps.**
- Copy the pre-fund address into the "To Address" field, then fill in an arbitrary amount of ETH.
- Confirm the transaction by clicking "Send Transaction", and the "Send" button in the pop-up window.
- Click the Check TX Status when the green message pops up, confirm the logout.
- You should see the transaction go from Pending to Successful in around the same block time you set in the genesis.
- You can click the Check TX Status button to update the status.
---
## Congrats, you are finished!