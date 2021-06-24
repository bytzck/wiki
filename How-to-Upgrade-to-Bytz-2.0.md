# Important Changes to BYTZ 2.0

Upgrading BYTZ 2.0 requires a _**FULL RESYNC**_ of the blockchain. 

This version also updates and changes the location of user specific files including wallet.dat.  

_It is imperative that you back up your wallet.dat and private keys BEFORE upgrading to BYTZ 2.0_


# Steps to Update
## 1. **Back up wallet.dat and private keys** 
 
**Default datadir for bytz**

`Linux: ~/.BYTZ/   `  
`Windows: %APPDATA%\BYTZ  `  
`Mac OS: ~/Library/Application Support/BYTZ/ `   

## 2. **Rename old data directory to BYTZ.old**

`Linux: mv .BYTZ .BYTZ.old`  
`Windows: Right click > rename > BYTZ.old`  
`Mac OS: Right Click > rename > BYTZ.old`  

## 3. **Launch new BYTZ QT/Daemon**
* Wait for full resync (this process may take some time)

## 4. **Shut down new BYTZ QT/Daemon**

## 5. **Copy wallet.dat from BYTZ.old to new BYTZ Directory**
* Within the **_NEW_** BYTZ directory is a folder `wallets`  
* Copy your `wallet.dat` from `BYTZ.old` to `BYTZ/wallets`

## 6. **Relaunch BYTZ 2.0 Daemon/QT**
## 7. **After rescanning, upgrade is complete**


