# Contract Verification via Sourcify

Along with contract verification through a flattened source file \(the default option in UCHAIN.INFO\), we provide a verification option through the [Sourcify](https://sourcify.dev/) API. The _Verification with Sourcify_ premium feature is enabled in the [Gnosis instance of UCHAIN.INFO](https://gnosis.uchain.info/).

## Usage Example

Verify your contract using Sourcify:

   1. Open the address page for the contract you want to verify, switch to _Code tab,_ and click _Verify & Publish_ button.

![](../../.gitbook/assets/screenshot-2020-12-28-at-08.38.15_2.png)

    2. Choose S_ources and metadata JSON file_ option and click the _Next_ button.

![](../../.gitbook/assets/screenshot-2020-12-28-at-08.43.13_2.png)

On the next screen, you will see a drop field where you will add files.

![](../../.gitbook/assets/screenshot-2020-12-28-at-08.46.06.png)

3. Drag and drop \(or click the button to add files from your computer\) all _.sol_ files used by the target contract you want to verify and the _.json_ file containing the contract's metadata. For example, this _.json_ is created by Truffle in _./build/contracts_ folder after `truffle compile`. If your contract has linked libraries you should also drag & drop _.json_ files __for those libraries. Once all files are added, start verification by clicking the _Verify & Publish_ button.

![](../../.gitbook/assets/screenshot-2020-12-28-at-08.57.42_2.png)

After several seconds your contract should be verified through Sourcify's API \(If verification fails, you will see the reason in the dropzone\). Verification metadata will be saved in the UCHAIN.INFO DB and you will see the verified contract page with the link to the same metadata in the [Sourcify contract repository](https://contractrepo.komputing.org/contracts/full_match/100/) \(chain ID is 100 for the xDai chain\).

![](../../.gitbook/assets/screenshot-2020-12-28-at-08.59.50.png)

Example Contract:

* Contract [verified via Sourcify contract in UCHAIN.INFO](https://gnosis.uchain.info/address/0x4f15a6e74CFC2F80D5967a8aB75F3c83D8043cF4/contracts). 
* The same contract in the [Sourcify contract repository](https://contractrepo.komputing.org/contracts/full_match/100/0x4f15a6e74CFC2F80D5967a8aB75F3c83D8043cF4/).

