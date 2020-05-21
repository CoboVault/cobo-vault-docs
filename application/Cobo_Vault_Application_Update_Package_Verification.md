
# Cobo Vault Application Upgrade Package Verification

## Introduction
Cobo Vault offers a method for verifying official release upgrade packages. You can compare a version package you compiled from Github source code with the official release update package to accomplish this.

Refer to the below sections for instructions.

## Download official release update package
- [Firmware upgrades on the official Cobo Vault website](https://cobo.com/hardware-wallet/firmware)

## Unzip official release update package
  Use [update public key](https://github.com/CoboVault/cobo-vault-cold/blob/master/app/build.gradle#L112) to unzip upgrade package.
  An upgrade package consists of the following parts:
- `app_[version_code]_[version_name]_[git_commit_id]_[apk_sha1_checksum].apk` : Cobo Vault cold upgrade version package
- `manifest.json` : Upgrade package digest information
- `serial_*.bin` : Cobo Vault Secure Element upgrade version package
- `signed.rsa` : Signature for upgrade package

## Download source code
- Download the code of the branch corresponding to the official upgrade package version from Github. For example, if the official upgrade package version is `V1.1.0`, the corresponding code branch is `V1.1.0-release`.
Run the following command to download the code:

`git clone -b [code branch] git@github.com:CoboVault/cobo-vault-cold.git --recursive`

Replace `[code branch]` with a specific branch name.

- When the code is downloaded you can run `git log` to find the latest commit ID.
  This commit ID should be the same as the commit ID obtained in the previous step.

## Build APK

`./gradlew assembleVault_v2Release`

You can find the APK in:

 `cobo-vault-cold/releases/[version_code]/app_***.apk`

## Verify APK
We now have two APK files, one extracted from official upgrade package, another built from the open source code.

Because the keystore used to sign the APK cannot be made public, a test keystore is used in the open source code.
Therefore, the digest of the APK file cannot be directly compared. Instead, we can compare whether the information before signing is consistent.

Change the suffix of the APK file to .zip, and then unzip the .zip file.
You can find the `META-INF/CERT.SF` file.

Then compare the two `META-INF/CERT.SF` files. If they are the same，you will have verified that the official upgrade package matches the open source code.







