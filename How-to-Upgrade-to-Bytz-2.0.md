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
### Mac OS
1.  [Download Migration App](https://github.com/bytzcurrency/BYTZ/releases/download/v0.2.0.0/BYTZ.Wallet.Migration.v2.app.zip)
2. Extract Zip
3. Double Click "Bytz Wallet Migration" (some users may receive a prompt to allow the script to run, select "OK")
4. Follow on screen prompts

### Windows
1. [Download Migration Script](https://github.com/bytzcurrency/BYTZ/releases/download/v0.2.0.0/bytz_windows_migration.ps1)
2. Right click bytz._windows_migration.ps1
3. Select "run with powershell" (some users may receive a prompt to allow the script to run, enter "Y" to allow one time exception)
4. Hit "enter" to close the window when prompted


### Linux:
Copy wallet.dat to the new directory.   
` cp ~/.bytz/wallet.dat ~/.bytzcoin/wallets/wallet.dat `   

## 7. **Relaunch BYTZ 2.0 Daemon/QT**
## 8. **After rescanning, upgrade is complete**


