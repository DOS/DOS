# Get Started

{% hint style="info" %}
Developers can follow this guide and configuration sections to launch an ecosystem wallet. Alternatively, if you prefer to use an existing ecosystem wallet SDK, refer to the usage section to install
{% endhint %}

The **DOSwap SDK** provides a straightforward method to create your own ecosystem wallet. It offers a code-splitting environment and essential tools to bring your wallet SDK to life, making the process both easy and efficient.

Video

### Requirements

To ensure optimal functionality, your code editor and project must use the same TypeScript version.

1. **TypeScript Version**: Ensure you have TypeScript version 5.0.2 or higher installed.
2. **TS Config Setup**:
   * Set the `compilerOptions.moduleResolution` to `bundler`.
3. **Enable Code Splitting**: Configure your project to support code splitting for enhanced performance.

This setup will help maintain consistency and leverage TypeScript's latest features efficiently.

### Install the DOSafe SDK

In your wallet SDK directory, use your preferred package manager to install the latest Ecosystem SDK version:

{% tabs %}
{% tab title="NPM" %}
`npm install @dos.me/wallet-sdk`
{% endtab %}

{% tab title="Yarn" %}
`yarn add @dos.me/wallet-sdk`
{% endtab %}
{% endtabs %}

### Creating your wallet SDK

{% stepper %}
{% step %}
**Import the SDK**

`import DOSafeSDK from "@dos.me/wallet-sdk";`
{% endstep %}

{% step %}
**Initialize the SDK**

Create a new instance of the DOSafe SDK with your configuration parameters:

```
const walletSDK = new DOSafeSDK({
  appName: "Your App Name",
  appLogoUrl: "Your App Logo URL",
  backendUrl: " Our Authentication Endpoint", // Backend URL
  walletUrl: " Our Wallet Endpoint", // Production URL
  ecosystemId: "Your Ecosystem ID",
  tokens: tokens,
  contractNFTs: contractNFTs,
  chainsSupported: [43113, 3939],
  policies: policyConfigurations,
  webPublicKey: "Your_Web_Public_Key",
  typeSDK: "popup", // 'sdk' or 'popup'
  explorerUrl: "https://testnet.snowtrace.io",
  chainDefault: 43113,
});
```
{% endstep %}

{% step %}
**Configuration parameters**

The following table describes the configuration parameters for the DOSafe SDK:

<table data-header-hidden><thead><tr><th width="209.00347900390625"></th><th width="138.2528076171875"></th><th></th></tr></thead><tbody><tr><td><kbd>Parameter</kbd></td><td>Required</td><td>Description</td></tr><tr><td><kbd>appName</kbd></td><td>Yes</td><td>Your application name</td></tr><tr><td><kbd>appLogoUrl</kbd></td><td>Yes</td><td>URL of your application logo</td></tr><tr><td><kbd>backendUrl</kbd></td><td>Yes</td><td>DOSafe backend URL for authentication</td></tr><tr><td><kbd>walletUrl</kbd></td><td>Yes</td><td>DOSafe wallet URL for transactions</td></tr><tr><td><kbd>ecosystemId</kbd></td><td>Yes</td><td>Your ecosystem ID provided by DOSafe</td></tr><tr><td><kbd>tokens</kbd></td><td>Yes</td><td>Token configuration by chain</td></tr><tr><td><kbd>contractNFTs</kbd></td><td>Yes</td><td>List of NFT contracts</td></tr><tr><td><kbd>chainsSupported</kbd></td><td>Yes</td><td>Array of supported chainIds</td></tr><tr><td><kbd>policies</kbd></td><td>Yes</td><td>Object containing policy IDs by chainId</td></tr><tr><td><kbd>webPublicKey</kbd></td><td>Yes</td><td>Your web public key for verification</td></tr><tr><td><kbd>typeSDK</kbd></td><td>Yes</td><td>SDK type ('sdk' or 'popup')</td></tr><tr><td><kbd>explorerUrl</kbd></td><td>No</td><td>Blockchain explorer URL</td></tr><tr><td><kbd>chainDefault</kbd></td><td>Yes</td><td>Default chainId to display first</td></tr></tbody></table>
{% endstep %}

{% step %}
**Environment URLs**

**Backend URL (backendUrl)**

<kbd>Development: https://beta.dos.me</kbd>

<kbd>Production: https://api.dos.me</kbd>

**Wallet URLs (walletUrl)**

<kbd>Development: https://test-of-wallet.doschain.com</kbd>

<kbd>Production: https://wallet.doschain.com</kbd>

**Token configuration (tokens)**

The `tokens` parameter requires a specific JSON structure:

```
const tokenConfigurations = {
  "43113": { // Avalanche Fuji Testnet
    "AVAX": {
      symbol: "AVAX",
      decimals: 18,
      contract: "", // Empty for native tokens
      provider: "https://avalanche-fuji-c-chain-rpc.publicnode.com",
      type: "native",
      abi: [],
    },
    "SECOND": {
      symbol: "SECOND",
      decimals: 18,
      contract: "0x10627F7D8117c4BdB9409813b47da5B21CAB5F9b",
      provider: "https://avalanche-fuji-c-chain-rpc.publicnode.com",
      type: "erc20",
      abi: [],
    },
  },
  "3939": { // DOS Chain Testnet
    "DOS": {
      symbol: "DOS",
      decimals: 18,
      contract: "",
      provider: "https://test.doschain.com",
      type: "native",
      abi: [],
    },
    "SECOND": {
      symbol: "SECOND",
      decimals: 18,
      contract: "0x17a11Dd7095555E26275F2DE38Ba4548229f5bbc",
      provider: "https://test.doschain.com",
      type: "erc20",
      abi: [],
    },
  }
};
NFT configuration (contractNFTs)
The contractNFTs parameter accepts an array of objects:
const nftConfigurations = [
  {
    chainId: 43113,
    contract: "0x7979c2815CD58184Bd91082CDe5E001f18b22368",
    abi: [],
  },
  {
    chainId: 3939,
    contract: "0x5f33e2db1448933f8e3B5a630E01D8E357bFe62F",
    abi: [],
  }
];
Policy configuration (policies)
The policies parameter requires an object mapping chain IDs to policy identifiers:
const policyConfigurations = {
  "43113": "pol_avalanche_policy_id",
  "3939": "pol_dos_chain_policy_id",
};
```

<mark style="color:red;">\[!IMPORTANT]</mark> To obtain policy IDs and register token/NFT contracts in the DOSafe system, contact DOSafe support at [hepl@doschain.com](mailto:hepl@doschain.com).
{% endstep %}
{% endstepper %}

### **SDK methods**

The following methods are available on the SDK instance:

{% stepper %}
{% step %}
#### **Check wallet connection**

_Check if the wallet is currently connected._

```
const connectionStatus = await walletSDK.checkWalletDOS();
```
{% endstep %}

{% step %}
#### **Authenticate wallet**

_Initiates the wallet authentication process._

```
const authResult = await walletSDK.authenticateWalletDOS();
```
{% endstep %}

{% step %}
#### **Connect wallet**

Establishes a connection to the DOSafe wallet.

```
const connectResult = await walletSDK.connectWalletDOS();
```
{% endstep %}

{% step %}
**Open wallet popup**

Manually opens the wallet popup interface.

```
await walletSDK.openPopupWallet();
```
{% endstep %}

{% step %}
**Logout**

Disconnects from the wallet and clears the session.

```
await walletSDK.logout();
```
{% endstep %}

{% step %}
**Read from contract**

Reads data from a smart contract.

```
const data = await walletSDK.readContract(
  "0xContractAddress",  // Contract address
  contractABI,          // Contract ABI as array
  "functionName",       // Function to call
  [param1, param2]      // Function arguments
);
```
{% endstep %}

{% step %}
**Write to contract**

Executes a transaction on a smart contract.

```
const txResult = await walletSDK.writeContract(
  contractAddress,  // Contract address
  contractABI,          // Contract ABI as array
  functionName,       // Function to call
  [param1, param2]      // Function arguments
);
```
{% endstep %}

{% step %}
**TypeScript support**

For TypeScript projects, create a declaration file named dosafe-sdk.d.ts with the following type definitions:

```
declare module "@dos/wallet-sdk" {
  class DOSafeSDK {
    constructor(config: {
      appName: string;
      appLogoUrl: string;
      backendUrl: string;
      walletUrl: string;
      ecosystemId: string;
      tokens: any;
      contractNFTs: any;
      policies: any;
      webPublicKey: string;
      chainsSupported: number[];
      typeSDK: string;
      chainDefault: number;
    });
    checkWalletDOS(): Promise<any>;
    authenticateWalletDOS(): Promise<any>;
    connectWalletDOS(): Promise<any>;
    openPopupWallet(): Promise<any>;
    logout(): Promise<any>;
    readContract(
      contractAddress: string,
      abiContract: any,
      functionName: string,
      args: any[]
    ): Promise<any>;
    writeContract(
      contractAddress: string,
      abiContract: any,
      functionName: string,
      args: any[]
    ): Promise<any>;
  }
  export default DOSafeSDK;
}
```
{% endstep %}

{% step %}
**Troubleshooting**

> **Tip**: Ensure the `chainDefault` value is part of the `chainsSupported` array.

> **Note**: Register ERC-20 token contracts and NFT contracts in the DOSafe system before
{% endstep %}
{% endstepper %}

