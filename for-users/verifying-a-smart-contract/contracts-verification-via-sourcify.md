# Contract Verification via Sourcify

Along with contract verification through a flattened source file (the default option in UCHAIN.INFO), a [Sourcify](https://sourcify.dev/) API verification option is also available. Projects who want to use this feature need to set the following [ENV variables](../../for-developers/information-and-settings/env-variables.md#sourcify).

```
SOURCIFY_INTEGRATION_ENABLED=true
SOURCIFY_SERVER_URL=https://sourcify.dev/server
SOURCIFY_REPO_URL=https://repo.sourcify.dev/select-contract/
```

## Usage Example

Verify your contract using Sourcify:

1\) Go to the Verify contract page _(Other -> Verify contract)_

<figure><img src="../../.gitbook/assets/sourcify-uchaininfo-1.png" alt=""><figcaption><p>Go to Other -> verify contract</p></figcaption></figure>

2\) Enter the deployed contract address, select Solidity (Sourcify) from the Verification method dropdown.

<figure><img src="../../.gitbook/assets/sourcify-uchaininfo-2.png" alt=""><figcaption></figcaption></figure>

3\) Drag and drop (or click the button to add files from your computer) all _.sol_ files used by the target contract you want to verify and the _.json_ file containing the contract's metadata. For example, hardhat stores the outputs of the compilations under the `artifacts/build-info/` folder inside the project. [More info is available here](https://docs.sourcify.dev/docs/metadata/).

If your contract has linked libraries you should also drag & drop _.json_ files _\_for those libraries. Once all files are added, start verification by clicking the \_Verify & Publish_ button.

<figure><img src="../../.gitbook/assets/solidity-uchaininfo-3.png" alt=""><figcaption></figcaption></figure>

5\) After several seconds your contract should be verified through Sourcify's API (If verification fails, you will see the reason in the dropzone). Verification metadata will be saved in the UCHAIN.INFO DB and you will see the verified contract page with the link to the same metadata in the [Sourcify contract repository](https://repo.sourcify.dev/contracts/full\_match/100/).

### Example Contract:

* Contract [verified in UCHAIN.INFO](https://gnosis.uchain.info/address/0x4f15a6e74CFC2F80D5967a8aB75F3c83D8043cF4?tab=contract)
* The same contract in the [Sourcify contract repository](https://repo.sourcify.dev/contracts/full\_match/100/0x4f15a6e74CFC2F80D5967a8aB75F3c83D8043cF4/).
