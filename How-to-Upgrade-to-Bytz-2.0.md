# Important Changes to BYTZ 2.0

Upgrading BYTZ 2.0 requires a _**FULL RESYNC**_ of the blockchain. 

This version also updates and changes the location of user specific files including wallet.dat.  

_It is imperative that you back up your wallet.dat and private keys BEFORE upgrading to BYTZ 2.0_

** New Default datadir for BYTZ**

`Linux: ~/.bytzcoin/   `  
`Windows: %APPDATA%\bytzcoin  `  
`Mac OS: ~/Library/Application Support/bytzcoin/ `   

# Steps to Update

## 1. **Back up wallet.dat and private keys** 

## 2. **Shut down old BYTZ QT/Daemon (Bytz 1.0)**

## 3. **Launch new BYTZ QT/Daemon**
* Wait for full resync (this process may take some time)

## 4. **Shut down new BYTZ QT/Daemon**

## 5. **Copy wallet.dat from BYTZ to new bytzcoin Directory**
_** For Mac OS and Windows the scripts below will accomplish the move for you. For linux simply run the provided copy command **_  
Mac and Windows : https://github.com/bytzcurrency/BYTZ/releases   
Linux: ` cp ~/.bytz/wallet.dat ~/.bytzcoin/wallets/wallet.dat ` 

## 7. **Relaunch BYTZ 2.0 Daemon/QT**
## 8. **After rescanning, upgrade is complete**


