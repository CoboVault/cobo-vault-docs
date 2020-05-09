# Cobo Vault Developer Documents

Cobo Vault is an air-gapped, open source hardware wallet that uses completely transparent QR code data transmissions. Visit the [Cobo Vault official website]( https://cobo.com/hardware-wallet/cobo-vault)  to learn more about Cobo Vault.

## High Level Architecture
Cobo Vault is build on self designed and safelly enhancend hardware platform with a secure element, currently it runs on highly safelly enhanced Android Go platform. 

![Cobo Vault Hight Level Architecture](./vault.jpg)

The Secure elememt is an secure component saving user's master seed. the secure element is commnucating with the application based on serial communication protocol. meanwhile user should set password before using Cobo Vault. all the senstive action should be authorized by user password.

since the Cobo Vault is compeletly air gaped, we use qr code to send and receive data. for the detail data protocol please check [here](https://github.com/CoboVault/crypto-coin-message-protocol)

since one QR code image can only contain limit size of data, we use animated QR Codes to handle bigger size of data. here is an sample:

```
{
    "total": 2,
    "index": 1,
    "checkSum": "807271c36d6e275b0e89b023ccf8e3b6",
    "value": "H4sIAAAAAAAAAyVPu0oDURQ0UZYllWy51RKESGDdc88995VCJCaLjaIYbeW+tgoshAj+gxZ+gP6Dlb9h5f94F4eBmWaGmXxclJe966tH+7zdV3e7yz7E6nbX73vfb8uPcT4uLkghC+gUi2iRGx6EDjqQ4x2TTnsK1kt0gBC0AgHEQURhrTLo0QkTp7+jyeF6c1WcOB+pSx21IIM1yWhqG5mpSShuu4jSMCyPrxuiWSNh1gxssPr6/vl8PTstFm+jyRxe1sSApQQJF4WUWrfE2xZWJtISl1yvrW6VRl1kDAaUk38dMM0YlxpgfgDZ4jzPiqOH+9WmzNO8p8FNU3/6w1Rn0kuMUSC3iCATjSFhlGc8aM6iV9X7zR9wPHYkQAEAAA==",
    "compress": true,
    "valueType": "protobuf"
}
```

The Total field is for `total` number of the animated QRCodes and `index` field is for the index number of the QRCodes. `value` field is for the chunck of data. `compress` is the indicator of whether the data is compressed.currently we use gzip + base64 to compressed the data and `checkSum` is the checksum of the whole data.

we are working an demo of the animated QRCodes. once finished we will open it on our Github.


## Hardware docs
[hardware](): check the hardware folders to checkout our hardware documents. currently we open our schematic and BOM files

## Application docs
[applications](): check our applications folders to see our applications documents



## Don't Trust, Verify!
In blockchain we belevie `Don't Trust Verify`. please checkout our documents how to verify our firmware.

## License
[![GPLv3 License](https://img.shields.io/badge/License-GPL%20v3-green.svg)](https://opensource.org/licenses/)
This project is licensed under the GPL License. See the [LICENSE](LICENSE) file for details.
