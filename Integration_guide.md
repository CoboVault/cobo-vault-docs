# Cobo Vault Integration Guide
Cobo Vault is an air-gapped, QR Code based hardware wallet. it would like to be the pure signer for your crypto transaction, Cobo Vault don't have other connections like BlueTooth, WIFI, etc. the only way it can transmit data is via QR codes.
this guide is for developer who would like to integrate their services with Cobo Vault.

### Animated QR Codes
In Cobo Vault, we use QR Codes to transmit data, since each qr code image can only contain limit size of data. in order to send big chunk of data, we use animated QR codes to transmit big chunk of data. we are using the bc-ur to encode the data, for bc-ur please refer this [doc](https://github.com/CoboVault/Research/blob/master/papers/bcr-0005-ur.md)
we have implemented different libraries of bc-ur:
- javascript version: https://github.com/CoboVault/cobo-vault-blockchain-base/tree/master/packages/sdk
```
// here is an sample:
yarn add @cvbb/sdk
const { CoboVaultSDK } = require("@cvbb/sdk")
const sdk = new CoboVaultSDK()
const data = sdk.encodeDataForQR('12334')
console.log(data)
// [ 'ur:bytes/1248938493496dfhjdh34' ]
```
each item in the array should be put into the qr code image.

- java version: https://github.com/CoboVault/bc32-java

#### libraries
##### web
there are a lot of existing libraries which currently can be used to scan qr codes and here is some of it used by our friends :)
- [qr-scaner](https://github.com/nimiq/qr-scanner)
- [vue-qr-code](https://github.com/gruhn/vue-qrcode-reader/) 

##### React Native
 - [react-native-camera](https://github.com/react-native-camera/react-native-camera)
 
##### Native App
for IOS and Android App we can use libraries to scan QR Codes.
 
 
## BTC Only Firmware
we provide BTC-only Firmware for Bitcoiner. you can get the latest firmware from [here](https://cobo.com/hardware-wallet/downloads), For Bitcoin, we follow the BIP174, aka PSBT to encode transaction. for ones who is not familar with BIP174 and PSBT here is some good reference guide.

- guide 1
- guide 2


### Single-Sig
currently we have integrated with a lot of well-known wallet, like electrum, BlueWallet, Wasabi Wallet, BTCPay, Spector etc. and also we provide and generic wallet model to other wallets or services who would like to integrate with us. 

#### Setup the watch-only wallet

watch-only wallet import the extended public key info from cobo vault, cobo vault support both file and qrcode, the xpub info defines as follow, here is an sample file for this info:

- `ExPubKey`: this is the extended public key of the master seed
- `MasterFingerprint`: this is the master finger print of master seed which is only used to identify the master seed
- `AccountKeyPath`: this is the derivation path of the extended public key
- `CoboVaultFirmwareVersion`: this is the firmware version in current hardware device. 

file: `p2wpkh-pubkey.txt`
```
{
    "ExtPubKey":"vpub5Z3SXQwuvQWt5vBQiRYrqhbou6BB7u1TFcA4DTQxPirU4oqMwnWW5DcSmM31h7SzofmUM3xHHn8rEht38jyuX8tfXS2D1desPVRsvnD5Dtr",
    "MasterFingerprint":"5271C071",
    "AccountKeyPath":"84'/1'/0'",
    "CoboVaultFirmwareVersion":"1.8.2(BTC-Only)"
}
```

qr code (json string):
![qrcode]("./pics/export_single_sig_expub.png)

#### Sign PSBT

file:
The unsigned PSBT file should be encoded as binary and use the .psbt file extension. reference [bip174](https://github.com/bitcoin/bips/blob/master/bip-0174.mediawiki)

see the example unsigned psbt file [example](./unsigned_single_sig_psbt.psbt)

qrcode:

The unsigned psbt qrcode encoded in bc-ur

<img src="./pics/export_single_sig_expub.png" width="200" height="200" />

#### export Signed PSBT

signed psbt file:

see the example signed psbt file [example](./signed_41262fb9.psbt)

qrcode:

<img src="./pics/signed_single_sig_psbt_qr.png" width="200" height="200" />

## Multi-Sig

#### Setup Multi-sig wallet

Users can create a Multi-sig wallet by collect all co-signers extended public key or import a multi-sig wallet file export from another cobo vault co-signer.

1. Create Multi-sig wallet

    file: `5271C071_P2WSH.json` (export from cobo vault)
   ```
   {
    "xfp":"5271C071",
    "xpub":"Vpub5mpRVCzdkDTtCwH9LrfiiPonePjP4CZSakA4wynC4zVBVAooaykiCzjUniYbLpWxoRotGiXwoKGcHC5kSxiJGX1Ybjf2ioNommVmCJg7AV2",
    "path":"m/48'/1'/0'/2'"
   }
   ```

   or `ccxp-5271C071.json` (export from cold card)

   ```
    {
    "p2wsh_deriv": "m/48'/1'/0'/2'",
    "p2wsh": "Vpub5mpRVCzdkDTtCwH9LrfiiPonePjP4CZSakA4wynC4zVBVAooaykiCzjUniYbLpWxoRotGiXwoKGcHC5kSxiJGX1Ybjf2ioNommVmCJg7AV2",
    "p2wsh_p2sh_deriv": "m/48'/1'/0'/1'",
    "p2wsh_p2sh": "Upub5SzABYKibXvQLzV7QjmTwEASKFT5kYQd6rC1khZRsTDXakFN7npFgrdoiStpBd9tt3cWbEUX1JrPZ2TzgibzzGgvfqLHkJ5TyCCAsft4XRG",
    "p2sh_deriv": "m/45'",
    "p2sh": "tpubD8sGsX8cpVReqMzjTH9QxbSrQd5sTodLWpNf7HWNaJgUFH31LZQfgqxYoj2infUf49NPtao85o9a2x3PNhQ2SmU8xGmDKZaBdiyck711mBk",
    "xfp": "5271C071"
    }
   ```
   qrcode(json string):

    <img src="./pics/multisig_xpub_qrcode.png" width="200" height="200" />
   

2. Import Multi-sig wallet
    
    File: `export_CV_85C39000_2-3.txt`

    ```
    # CoboVault Multisig setup file (created on C2202A77)
    #
    Name: CV_85C39000_2-3
    Policy: 2 of 3
    Derivation: m/48'/1'/0'/2'
    Format: P2WSH

    C2202A77: Vpub5nbpJQxCxQu9Nv5Effa1F8gdQsijrgk7KrMkioLs5DoRwb7MCjC3t1P2y9mXbnBgu29yL8EYexZqzniFdX7Xo3q8TuwkVAqbQpgxfAfrRiW
    5271C071: Vpub5mpRVCzdkDTtCwH9LrfiiPonePjP4CZSakA4wynC4zVBVAooaykiCzjUniYbLpWxoRotGiXwoKGcHC5kSxiJGX1Ybjf2ioNommVmCJg7AV2
    748CC6AA: Vpub5mcrJpVp9X8ZKsjyxwNu36SLRAWTMbqUtbmtcapahAtqVa66JtXhT4Uc9SVLN1nF782sPRRT2jbUbe7XzT8eue6vXsyDJKBvexGJHewyPxQ
    ```

    QrCode (bc-ur): `text -> getByte(text, "utf8") -> hex -> Ur encode`

    ```
    UR:BYTES/TYQL2GEQGDHKYM6KV96KCAPQF46KCARFWD5KWGRNV4682UPQVE5KCEFQ9P3HYETPW3JKGGR0DCSR2V3HX9PNQDE39Y9ZXZJWV9KK2W3QGDT97WP4GVENJVPSXP0NYTFNPFGX7MRFVDUN5GPJYPHKVGPNPFZX2UNFWESHG6T0DCAZQMF0XSUZWTE3YUHNQFE0XGNS53N0WFKKZAP6YPGRY46NFQ9Q5SEJXGCRYSFHXUAZQ4NSW43R2MNZWP99Z7ZR0PGH2W2WWC652ENXVYC5VWR8V3GHX6T2WFNKKD6TWFXKK6T0F3EN23R02FMKYD6DGD4YXVM5X9GRY7FED4VXYMJZVA6NYWTEFSUY2KT90PD8Z7NWD9RXGKPHTPHNXUFC236HW66KG9CKY5TSVAUXVSTXWFFXJ4C2X5ERWV2RXQMNZW3Q2EC82C34D4C9Y4JR0FJXK3Z5W3PHWJPEF3EXV6TF2PHKUE2SDFGRGS662DSKKSF5WAUKUSE50FTYY4JPDAHKZ7TTD9PH56J4DE54JCJVWPTHSM6JDA6YW62CWAH5K3MRFPPN266N0P55536CX9VKY6NXXF5K7NN0D4K4VM2RFFNNWS2KXG9RWDPCGDPNVS2P8GS9VUR4VG6K6CMJFFC9VUPETQU95JMNDFUHSA6WW5ENV56V2FQ4W4ZDVFC42ARZD46XXCTSV95YZAR32ESNVDJ2W3VXS4P5243NJ56KF38RZMJXXUURYU6S2FF9GVN2VF2KYEFHTPA9GWR9W4JNVAJCWDU5GJJTGFMX27Z8FFYX2AME2PU9ZZSDGUVWA
    ```

    <img src="./pics/import_multisig_qr.png" width="200" height="200" />

#### Sign PSBT


unsigned psbt file:

see the example unsigned multisig psbt file [example](./unsigned_multisig_psbt.psbt)

unsigned psbt Qrcode(encoded in bc-ur):

<img src="./pics/unsigned_multisig_psbt.gif" width="200" height="200" />


#### export Signed PSBT

signed multisig psbt file:

see the example partially signed multisig psbt file [example](./part_f6a35290_5271C071.psbt)

signed multisig psbt qrcode(encoded in bc-ur):

<img src="./pics/signed_multisig_psbt.gif" width="200" height="200" />

### FAQ
