This guide contains instructions to set up a single BYTZ mainnet masternode manually using a combination of a local desktop wallet (BYTZ QT) and a remote wallet (BYTZ daemon) on a virtual private server (VPS).

**For moderate to advanced users**

Setting up an BTYZ masternode this way requires some basic understanding of command line operations and careful inputting of commands in a console or terminal window. Masternodes require some ongoing maintenance, including installing services updates and upgrades for network improvements. Masternode operators must also carefully consider the security of their funds, particularly their masternode collateral by securely backing up crucial information and encrypting wallets.

# Requirements

**To operate a masternode, you will need:**
* 10,000,000.01 BYTZ coin (10,000,000 for collateral/.01 for transaction fees).
* 1 GVT.credit token (Issued by the BYTZ team)
* A local QT wallet available from https://bytz.gg for the BYTZ collateral.
* A local LINUX computer instance or remote Virtual Private Server (VPS) to run the BYTZ masternode daemon wallet and access the collateral coins remotely.


# VPS requirements

The VPS or remote server should have at least 2GB of free memory. Ubuntu 18.04 x64 is a stable platform for running masternodes. A VPS normally has a static IPs and an always-on internet connection with stable uptime which provides the constant network connection that the masternode needs.

**There are many providers of VPS services:**

* Vultr
* Hetzner
* Digital Ocean
* Google
* Amazon EC2
* OVH

The VPS provider you choose should offer good hardware and guarantee high uptime. Continuous connectivity is important because if the masternode loses connection with the BYTZ network, it can take time to be registered for rewards again. Read reviews of services to determine which is best for you.

# Setting up the Remote Masternode Server

Once you have installed Ubuntu 18.04, you’ll connect to the server through Secure Shell (SSH).

**Mac and Linux**

On Mac and Linux it is possible to SSH directly from the Terminal.app window by typing:

`ssh root@<server IP address>`

The formatted command should look something like this:

`ssh root@111.222.333.444`

Since this is the first time your computer is connecting to the server, you’ll be prompted with a warning: 
```
<The authenticity of host 'xxx.xxx.xxx.xxx (xxx.xxx.xxx.xxx)' can't be established.
ECDSA key fingerprint is SHA256:3SDF4LKfhsJOWhsIrEUTruynMNjdCXjLdKJOTH.
Are you sure you want to continue connecting (yes/no)?>.” If prompted, reply “Yes.
```

Enter your password at the prompt.

**Windows**

To connect to the new VPS via SSH on Windows, download the PuTTy app:

https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

Open PuTTy.
Under “Host Name (or IP address),” replace the default text, “111.222.333.444,” with your new VPS IP.
Click “Open.”

There _may_ be a certificate warning because this is the first time you are connecting to this server.

![PuTTy](https://i.imgur.com/3wgTuCg.png)

When you see the prompt “Login as,” enter:

`root`

Follow prompts to enter the password assigned by your provider and press “Return.”

If your provider does not require a password change upon initial login, it is the best security practice to set a root password yourself. A strong password should be randomly generated and should contain both upper and lowercase characters as well as numbers and special characters.

To change root password, enter:

`passwd root`

After you press return, confirm the password.

For security, it is best practice not to run the system as the root user and so a new user should be added. Enter:

`adduser --gecos "" <username>`

**Example adding user “MN1”:** 

`adduser --gecos "" mn1`

Press return, then add the new user to the sudo users file:

`usermod -aG sudo <username>`

The new user has now been added with sudo permissions.

Save the new username and password in a secure place. If you lose these details, you will also lose access to the user account.

Switch user from the root user to the newly created user:

`su <username>`

For example:
`su mn1`

# Update the server:

Run the following two commands. 

`sudo apt update`

`sudo apt upgrade`

**Install build dependencies**

Enter the following commands to install and setup the BYTZ masternode remote wallet on your VPS. These compile instructions have been tested on Ubuntu 18.04. They will also work on other compatible Linux flavors.

`sudo apt-get install curl build-essential libtool autotools-dev automake pkg-config python3 bsdmainutils cmake git`

Change the directory to your masternode user’s home folder so you can download and compile the BYTZ daemon wallet.

`cd ~`

**Clone the BYTZ repository**

“Cloning” copies the wallet software from the Github developer repository to your VPS.

`git clone https://github.com/bytzcurrency/BYTZ.git`

**Build additional dependencies**

“Runtime dependencies” are tools or packages required to install, compile, or run software.

`cd bytz/depends`

`make HOST=x86_64-linux-gnu`

**Return to the BYTZ directory**

`cd`

`cd bytz`

# Build the wallet

`./autogen.sh`

```./configure --prefix=`pwd`/depends/x86_64-linux-gnu```

or

```./configure --prefix=`pwd`/depends/x86_64-linux-gnu --without-miniupnpc --without-gui --disable-tests```

**NOTE:** If these commands return an error message, replace with full path, eg: 

`/home/Username/bytz/depends/x86_64-linux-gnu`

`make`

**NOTE:** Compiling will take some time. Make some tea.

After compiling, move the newly created binaries to the /usr/bin/ folder. This allows you to run and interact with the BYTZ daemon (bytzd) and the command line interface (bytz-cli) from anywhere on the system:

`cp ~/bytz/src/bytzd ~/bytz/src/bytz-cli /usr/local/bin`

Next, you’ll configure the **local** “controller” wallet (on desktop). You will complete the **remote** wallet setup (on VPS) later.

# Setting up the local desktop wallet

First, configure your preferences on the BYTZ QT for easy masternode management.

**Windows:** Under Settings > Options choose Preferences
**Mac:** BYTZ > Preferences

Select the Wallet tab and check “Show Masternodes tab” from the File menu > Settings > Options > Wallet tab then ‘✔’ the box.

The BYTZ local desktop wallet will hold the masternode collateral. The wallet must be fully synced and must hold a single 10,000,000 BYTZ unspent transaction which serves as the masternode collateral required to operate on the BYTZ network. Ideally this is held on a newly created and backed up address.

If you don’t yet have 10,000,000 BYTZ in a single transaction, here’s how to create a new address so you can get the 10,000,000 BYTZ.

After the wallet has synced, click on **Tools**, then click **Debug Console** and type the following commands.

`getnewaddress mn1`

Paste this newly created address into a text editor then label it “mn1” or “masternode collateral.” Adding a recognizable label (like mn1) will make it easy to recognize it when you need it later.

**Security note:** Backup your private key securely!

It is best practice to back up the private keys for this address before filling it with the required masternode collateral. To backup the private key enter the following:

`dumpprivkey <newaddress>`

Where <newaddress> is the output from getnewaddress command. NOTE: do not add the brackets.

Now, **SEND EXACTLY 10,000,000 BTYZ to the new address in a single transaction.** Once the network has confirmed this transaction, open the debug window and enter:

`masternode outputs`

Example of resulting output (may be multiple strings):

```
{
  "37124f4da7ffe26b509263e70ac417fe5d862e8905c5abc9cdbf827f2658a6c3": "0"
}
```

**Generate the masternode key pair**

To run a BYTZ masternode, you must generate a public/private key pair.
 
By the end of this document, you will register your public key on the network through an initial registration transaction. Then, once the public key is registered, you can add the “secret” or private key to the remote VPS server, and register it on the network so that your masternode gets included in the masternode list.

# Background on BYTZ’s new masternode system

On BYTZ's new masternode system, the masternode list is derived entirely from information found on-chain. New masternodes are added by new special transactions called “Provider Registration Transactions” (abbreviated as ProRegTx). Masternodes are only removed by spending the collateral. A ProRegTx includes either a 10,000,000 BYTZ collateral payment in a single transaction AND 1 GVT.credit token which is burned on creation. 

To generate the masternode key pair, go to the debug console and enter:

`bls generate`

[**Note:** BLS stands for Boneh-Lynn-Sacham and refers to a type of cryptographic signature.]

**Sample output:**

```
{
  "secret": "395555d67d884364f9e37e7e1b29536519b74af2e5ff7b62122e62c2fffab35e",
  "public": "99f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670e"
}
```

The keys generated by this command are NOT stored by the wallet, so you must securely back them up in case you need them later. The `bls generate` is similar to the masternode `genkey` or `createmasternode` commands used in previous versions of BYTZ.

# Configure the bytz.conf file on the remote server (VPS)

Back on the VPS, you’ll add the BLS public/private key pair to the masternode’s bytz.conf file. This allows the masternode to actively listen on the blockchain for relevant ProRegTx transactions. When the ProRegTx transaction is broadcast by the operator’s wallet and confirmed by the wider network of miners, the node will start serving as a masternode on the BYTZ network.

NOTE: In order to run commands on the remote server (on VPS), you must specify the application that will run the commands. To issue commands for the BYTZ daemon, you must use the bytz-cli (CLI stands for command line interface). So you will type “bytz-cli” before each command for the BYTZ daemon. 

Example:

`bytz-cli help`

**Note:** You will replace help with the relevant commands.

On the **remote** masternode server that you setup in the first step, edit the bytz.conf file with the command as follows:

Make a new directory called “.bytz”

`mkdir ~/.bytz`

Configure the daemon by writing a preferences file called “bytz.conf”

`nano ~/.bytz/bytz.conf`

The editor will open a blank document called, “bytz.conf”. Copy the text below and paste it into the editor:

```# NOTE: Lines preceded by the hashtag or pound sign are “commented out” and ignored by the application.
# If you want to run testnet, uncomment the line below for testnet by removing the hashtag.

# testnet=1

rpcuser=<REPLACE ME>
rpcpassword=<REPLACE ME>
rpcallowip=127.0.0.1
listen=1
server=1
daemon=1

externalip=XXX.XXX.XXX.XXX:37415
# **NOTE:** Replace “XXX.XXX.XXX.XXX” with the VPS IP address
# **NOTE:** testnet port 47415

masternodeblsprivkey=<“secret”>

# NOTE: Replace <”secret”> with the “secret” output from the `bls generate` command

masternode=1
```

**Sample bytz.conf with all comments removed and including output from BLS generate:**
```
rpcuser=STRONG_USERNAME
rpcpassword=STRONG_PASSWORD
rpcallowip=127.0.0.1
listen=1
server=1
daemon=1
externalip=123.456.78.90
masternodeblsprivkey=395555d67d884364f9e37e7e1b29536519b74af2e5ff7b62122e62c2fffab35e
```

Once you confirm the details, press **ctrl+X** to exit the editor, then **Y** to confirm the new edits, then the **Return key** to save the file under the same name.

Adding the `masternodeblsprivkey` enables masternode mode. When the daemon wallet next starts, the “bytz.conf” file now forces the following settings needed to provide the masternode service:

`txindex=1`

`peerbloomfilters=1`

`prune=0`

# Start the wallet on the remote masternode:

`bytzd &`

Press Enter again to get back to the command prompt.

# Generate owner/voting key

Return to your local desktop wallet’s Debug Console.

With the keys that were generated above by the ``bls generate`` command, we must prepare a transaction that registers the masternode on the network. This is called the “ProRegTx” transaction.

The private key was already entered on the bytz.conf configuration file. Now, the public key is used in this transaction as the `operatorPubKey`.

You’ll need an “`OwnerKeyAddr`” for voting (later) and for masternode registration (now). This needs to be a new and unused address. Generate it with the following command:

`getnewaddress OwnerKeyAddr`

A new address will be generated within the wallet and will be displayed on screen in the console window.

Example output:
	
`sLQ72uinxF95yxiTrpq9QM4uh1GgBGPSoa`

Paste the address generated on your screen into your text editor and label it as “`OwnerKeyAddr`”.

**Generate payout address (PayoutAddress)**

Next, you’ll  be generating a `payoutAddress` that receives the masternode rewards. By default, the `PayoutAddress` will also be the `FeeSourceAddress`, but you may choose to separate them. You may even choose a payout address that is not on your local wallet. If you do so, however, note that in order to submit the transaction to the network successfully, the `FeeSourceAddress` must exist in the wallet that is submitting the transaction to the network. The `PayoutAddress` or `FeeSourceAddress` must have enough balance to cover the transaction fee. If there is insufficient balance,  the final `register_submit` transaction will fail.

**To generate a new PayoutAddress:**

`getnewaddress PayoutAddress`

**Example output:**

`sT5k3AZPvQ7Ky13LfVP6E8bjfurk2AtHre`

It is also possible to generate and fund another address as the “transaction fee source” (labelled as `FeeSourceAddress`):

`getnewaddress FeeSourceAddress`

`Registering the masternode with the network`

If the wallet is encrypted and locked, it must now be unlocked to perform the next commands:

`walletpassphrase y0urSecretP455w0rd 600`

Replace `y0urSecretP455w0rd` with your own password (the one that you used to encrypt the wallet).

Note: “`600`” in the command example is the amount of time in seconds that the wallet remains unlocked.

“ProRegTx” is a special transaction that must now be prepared with the command: 

`protx register_prepare`

The command has the following syntax:

```
protx register_prepare collateralHash collateralIndex ipAndPort ownerKeyAddr operatorPubKey votingKeyAddr operatorReward payoutAddress (OPTIONAL:feeSourceAddress)
```

**NOTE:** The “`feeSourceAddress`“ at the end of the command is in parentheses to indicate that it is optional. If you don’t use a separate `feeSourceAddress`, you can leave this out.

Open a text editor to prepare this command, replacing each syntax argument as follows:

`collateralHash`: The txid of the 10,000,000 BYTZ collateral funding transaction<br><br>
`collateralIndex`: The output index of the 10,000,000 BYTZ funding transaction<br><br>
`ipAndPort`: Masternode IP address and port, in the format x.x.x.x:yyyy, where the IP address is the IP address of your remote masternode server (VPS), and the standard BYTZ Mainnet port is 37415. The standard BYTZ Testnet port is 47415.<br>
`ownerKeyAddr`: The new BYTZ address generated above for the owner/voting address. <br><br>
If you own the private key connected to the `ownerKeyAddr` (which is a public key), you can change the masternode's configuration, e.g. through the protx update_registrar command.<br><br>
`operatorPubKey`: The BLS public key generated above (or provided by your hosting service).<br><br>
The `operatorPrivKey` is added to the configuration file of the remote masternode node, and is used to demonstrate to the network that the remote masternode node acts on behalf of you, the owner of the `operatorPubKey`.<br><br>
`votingKeyAddr`: The new BYTZ address generated above for owner/voting, or the address of a delegate, used for proposal voting and other functions.<br><br>
The person owning the private key belonging to the `votingKeyAddr` can vote on behalf of the masternode that is registered with this `votingKeyAddr`.<br><br>
`operatorReward`: The percentage of the block reward allocated to the operator as payment. [Most users will choose 0, indicating that 100% of the reward will go to the `payoutAddress`.]<br><br>
`payoutAddress`: A new or existing normal BYTZ address to receive the owner’s masternode rewards. Payouts are sent to the `payoutAddress`. (If there is an operator reward, these payouts are deducted from the amount sent here, and sent to the operator instead.)<br><br>
`feeSourceAddress`: An (optional) address used to fund ProTx fee. When a feeSource Address is specified, the `payoutAddress` will not be used for fees.<br><br>
When sending this registration transaction to the BYTZ network (which you will do a bit later), you must pay a fee. If `feeSourceAddress` is specified, the fee will be paid from this address.<br>

**Notes about the above keys and addresses:**

`ownerKeyAddr` must be unique (cannot be used for another masternode)<br><br>
`operatorPubKey` must be unique (cannot be used for another masternode)<br><br>
`ipAndPort` must be unique (cannot be used for another masternode)<br><br>
Initially, the `ownerKeyAddr` and `votingKeyAddr` must be the same. Masternodes that are created later can specify a `votingKeyAddr` that is different from the `ownerKeyAddr`.<br><br>

**NOTE:** This “`protx register_prepare`” transaction, does not allow you to specify an `operatorReward` payout address.  If you enter a non-zero `operatorReward`, you’ll need to follow up with another separate transaction called the “update_service transaction” that specifies a different operator payment address.

**ProTx Register example:**

```
protx register_prepare f292da56253b983d89c377460d46cacdffc2fa95b6c085463c99e6d19c12d851 1 72.80.492.390:27170 i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip 10390edad24d130602f22072a0cd58d7bc662d3667696e331f08308a29c45eb112d5eefff87b7866d1 5b387c01e659e i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip 0 i84AChqrdED2FHEFBCV7dWgt0fEaaGAEao
```

Example of transaction ID output from the `protx register_prepare command` above:

```
{
  "tx": "030001000175c9d23c2710798ef0788e6a4d609460586a20e91a15f2097f56fc6e007c4f8e0000000000feffffff01a1949800000000001976a91434b09363474b14d02739a327fe76e6ea12deecad88ac00000000d1010000000000e379580dcfea3eb6944bfbe1de5fa2317932e260acce4d9ff3ede5f4287a34160100000000000000000000000000ffff2d4ce6ef4e1fd47babdb9092489c82426623299dde76b9c72d9799f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670ed1935246865dce1accce6c8691c8466bd67ebf1200001976a914fef33f56f709ba6b08d073932f925afedaa3700488acfdb281e134504145b5f8c7bd7b47fd241f3b7ea1f97ebf382249f601a0187f5300",
  "collateralAddress": "i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip",
  "signMessage": "i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip|1|i84AChqrdED2FHEFBCV7dWgt0fEaaGAEao|cd58d7bc662d3667696e331f08308a29c45eb112d5eefff87b7866d1f5b387"
}
```

Next, to complete the composition of the transaction so that you can submit it, you will sign the transaction using the `collateralAddress` and `signMessage` fields with the output of the tx field.

**Sign the ProRegTx transaction**

The contents of the ProRegTx operation must now be signed to authorize the masternode instance with the `signMessage` field for the collateral address detailed in the `collateralAddress` entry.

The BYTZ wallet or another daemon instance is usually used to sign the transaction but signing can also be carried out from a hardware wallet. To sign from a hardware wallet, use the following command syntax:

`signmessage collateralAddress signMessage`

Or

`“signmessage "collateralAddress" "signMessage"”`

**Note:** In the syntax above, the first “`signmessage`” is a command. “`collateralAddress`” and the second instance of “`signmessage`“  are both placeholders that you’ll need to replace with the collateral address and the output from the ProRegTx above.

**Example:**

```
signmessage i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip i84AChqrdED2FHEFBCV7dWgt0fEaaGAEao|0|i0nFXhqrdDG1GZWKJAN6dQba6dZdgBGAip|ad5f82257bd00a5a1cb5da1a44a6eb8899cf096d3748d68b8ea6d6b10046a28e
```

**Sample console output:**

```
II8JvEBMj6I3Ws8wqxh0bXVds6Ny+7h5HAQhqmd5r/0lWBCpsxMJHJT3KBcZ23oUZtsa6gjgISf+a8GzJg1BfEg=
```

**Submit the transaction:**

The special masternode transaction we created now needs to be submitted to the BYTZ blockchain to register the masternode on the network. The command needs to be sent from an BYTZ wallet that holds the balance on either the `feeSourceAddress` or `payoutAddress` entries. A standard transaction fee is required (.0001 BYTZ).

This command uses the following syntax:

`protx register_submit tx sig`

**Where:**

`tx`: The serialized transaction previously returned in the tx output field from the protx register_prepare command
`sig`: The message signed with the collateral key from the signmessage command

**Input example:**

```
protx register_submit 030001000175c9d23c2710798ef0788e6a4d609460586a20e91a15f2097f56fc6e007c4f8e0000000000feffffff01a1949800000000001976a91434b09363474b14d02739a327fe76e6ea12deecad88ac00000000d1010000000000e379580dcfea3eb6944bfbe1de5fa2317932e260acce4d9ff3ede5f4287a3416010000000000000000000001100ffeef2d4ce6ef4e1fd47babdb9092489c82426623299dde76b9c72d9799f20ed1538e28259ff80044982372519a2e6e4cdedb01c96f8f22e755b2b3124fbeebdf6de3587189cf44b3c6e7670ed1935246865dce1accce6c8691c8466bd67ebf1200001976a914fef33f56f709ba6b08dd073932f925afedaa3700488acfdb281e134504145b5f8c7bd7b47fd241f3b7ea1f97ebf382249f601a0187f5300 II8JvEBMj6I3Ws8wqxh0bXVds6Ny+7h5HAQhqmd5r/0lWBCpsxMJHJT3KBcZ23oUZtsa6gjgISf+a8GzJg1BfEg=
```

**Output example:**

The following output example is the transaction ID for the command ``protx register_submit tx sig``

`dba8c22f8992d78fd4ff0c94cb19a5c30e62e7587ee43d5285296a4e6e5af062`

After this operation has been performed, your masternode will be successfully registered on the network after the transaction has been mined and confirmed in a block.

You can view the list of masternodes in the command console using the command

`protx list valid`

The final transaction ID details will be your newly registered masternode.

You can check the status of the masternode with the following command:

`masternode status`

This command returns the status of your masternode. If your masternode is correctly configured and actively serving the BYTZ network, Congratulations!


***

**BYTZ Masternode Components Worksheet**

Copy this form to a text editor. Paste the content you will need as you generate it.

New masternodes are added by new special transactions called Provider Registration Transactions (abbreviated as ProRegTx). This guide gives you step-by-step instructions to create the ProRegTx, constructing the command in the following format.

```
protx register_prepare collateralHash collateralIndex ipAndPort ownerKeyAddr operatorPubKey votingKeyAddr operatorReward payoutAddress (feeSourceAddress)
```

This worksheet will help you gather the data you need to construct the ProRegTx.

IPandPort: Write down the IP address of your remote masternode server (VPS). The standard BYTZ port is 37415.

Collateral address: This is the address that holds exactly 10,000,000 BYTZ in a single unspent transaction.

Collateral address private key: 

**XXXXXXXXXXXXXXX**<br>
**Do NOT paste the private key in the document editor.**<br>
**Store it somewhere that no one else can access.**<br>
**XXXXXXXXXXXXXXX**

"transaction hash" for 10,000,000 BYTZ collateral (output from `masternode outputs` command)

"output index" for the collateral transaction above

"secret" output from ``bls generate`` command

operatorPubKey: This is the "public" output from `bls generate` command.

ownerKeyAddr: This is the output from `getnewaddress ownerKeyAddr`

votingKeyAddr: This is the same as the `ownerKeyAddress` as above)

feeSourceAddress: This is the output from `feeSourceAddress`. This address must have BYTZ in it for the fee as well as the 1 GVT.credit token to be burned during the Masternode registration.


