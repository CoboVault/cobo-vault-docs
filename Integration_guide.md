# Cobo Vault Integration Guide
Cobo Vault is an air-gapped, QR Code based hardware wallet. it would like to be the pure signer for your crypto transaction, Cobo Vault don't have other connections like BlueTooth, WIFI, etc. the only way it can transmit data is via QR codes.
this guide is for developer who would like to intregate their serivces with Cobo Vault.

## BTC Only Firmware
we provide BTC-only Firmware for Bitcoiner. you can get the latest firmware from [here](https://cobo.com/hardware-wallet/downloads), For Bitcoin, we follow the BIP174, aka PSBT to encode transaction. for ones who is not familar with BIP174 and PSBT here is some good reference guide.

- guide 1
- guide 2

### Animated QR Codes

### Single-Sig
currently we have integrated with a lot of well-known wallet, like electrum, BlueWallet, Wasabi Wallet, BTCPay, Spector etc. and also we provide and generic wallet model to other wallets or services who would like to integrate with us. 

#### Setup the watch-only wallet

#### Sign PSBT

#### export Signed PSBT

## Multi-Sig

#### Setup Multi-sig wallet

#### Sign PSBT

#### export Signed PSBT


### FAQ