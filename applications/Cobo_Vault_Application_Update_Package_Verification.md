
# Cobo Vault Application Update Package Verification

## Instruction
The Cobo Vault offer a method to verifying official release update package. You can compare the version package compiled from GITHUB source code with the official release update package.

Refer the following steps below:

## Download official release update package
- [Cobo Value official firmware website](https://cobo.com/hardware-wallet/firmware)

## Unzip official release update package
  Use the public key [] to unzip update package.
  Update package consists of the following parts:
- `app_[version_code]_[version_name]_[git_commit_id]_[apk_sha1_checksum].apk` : Cobo Vault cold update version package
- `manifest.json` : Update package digest information
- `serial_*.bin` : Cobo Vault secure element update version package
- `signed.rsa` : Signature for update package

## Download source code
- Download the code of the branch corresponding to the official upgrade package version from github. For example, the official upgrade package version is `V1.0.6`, and the corresponding code branch is `1.0.6-public-release`.
Run the following command to download the code:
`git clone -b [code branch] git@github.com:CoboVault/cobo-vault-cold.git --recursive`
Replace `[code branch]` with a specific branch name

- When the code is downloaded you can run `git log` to find the latest commit id,
This commit id should be the same as the commit id obtained in the previous step

## Build apk
`./gradlew assembleVault_v2Release`
you can find apk in `cobo-vault-cold/releases/[version_code]/app_***.apk`

## Verify apk
Because the keystore used to sign the apk cannot be made public, a test keystore is used in the open source code. Therefore, the digest of the apk file cannot be directly compared. Instead, we can compare whether the information before signing is consistent.

now we have 2 apk files, one extract from official update package, another one is build from open source code.

Change the suffix of the apk file to .zip, and then unzip the .zip file
you can find the `META-INF/CERT.SF` file,

then compare the 2 `META-INF/CERT.SF`s, if they are the same，It can be proved that the official upgrade package and the open source code match








