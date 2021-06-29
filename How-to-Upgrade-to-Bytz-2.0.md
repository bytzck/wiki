# Important Changes to BYTZ 2.0

Upgrading BYTZ 2.0 requires a _**FULL RESYNC**_ of the blockchain. 

This version also updates and changes the location of user specific files including wallet.dat.  

_It is imperative that you back up your wallet.dat and private keys BEFORE upgrading to BYTZ 2.0_


# Steps to Update
## 1. **Shut down old BYTZ QT/Daemon (Bytz 2.0)**
## 2. **Back up wallet.dat and private keys** 
 
**Default datadir for bytz**

`Linux: ~/.bytz/   `  
`Windows: %APPDATA%\BYTZ  `  
`Mac OS: ~/Library/Application Support/BYTZ/ `   

## 3. **Rename old data directory to BYTZ.old**

`Linux: mv .bytz .bytz.old`  
`Windows: Right click > rename > BYTZ.old`  
`Mac OS: Right Click > rename > BYTZ.old`  

## 4. **Launch new BYTZ QT/Daemon**
* Wait for full resync (this process may take some time)

## 5. **Shut down new BYTZ QT/Daemon**

## 6. **Copy wallet.dat from BYTZ.old to new BYTZ Directory**
* Within the **_NEW_** BYTZ directory is a folder `wallets`  
* Copy your `wallet.dat` from `BYTZ.old` to `BYTZ/wallets`

## 7. **Relaunch BYTZ 2.0 Daemon/QT**
## 8. **After rescanning, upgrade is complete**


