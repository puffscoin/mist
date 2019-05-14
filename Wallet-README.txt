PUFFScoin Wallet

The PUFFScoin wallet, which allows you to create simple and multisig wallets to manage your PUFFScoins.

The wallet contains its own node, but can also use an already running one, if the IPC path of that node is the standard path.
(See below)

## Paths

The paths which store your wallets database and node are different:

The wallet (Mist) stores its data at:

* Mac: ~/Library/Application Support/Mist
* Windows: %APPDATA%\Roaming\Mist
* Linux: ~/.config/Mist

The nodes data is stored at:

* Mac: ~/Library/Puffscoin
* Windows: %APPDATA%\Roaming\Puffscoin
* Linux: ~/.puffscoin

## Issues

If you find issues or have suggestion, please report them at  
https://github.com/puffscoin/meteor-dapp-wallet/issues

## Repository

The wallet code can be found at  
https://github.com/puffscoin/meteor-dapp-wallet

And the binary application code, which wraps the wallet app can be found at  
https://github.com/puffscoin/mist/tree/wallet

## Bundling the wallet

To bundle the binaries yourself follow the instructions on the mist#wallet readme  
https://github.com/puffscoin/mist/tree/wallet#deployment
