# DOS Bridge

## LayerZero Addresses

#### DOS Testnet <a href="#id-0u7_u" id="id-0u7_u"></a>

* chainId: 10162
* endpoint: 0x45841dd1ca50265Da7614fC43A361e526c0e6160

#### DOS Mainnet <a href="#tomnd" id="tomnd"></a>

* chainId: 149
* Endpoint: 0x9740FF91F1985D8d2B71494aE1A2f723bb3Ed9E4



Bridging tokens from and to DOS is powered by the [LayerZero protocol](https://layerzero.network/). To learn more about it and how to use it in your own project, visit our [Omnichain token](https://docs.doschain.com/contract-addresses) guide.

## LayerZero Bridge Guide

{% hint style="info" %}
Ensure you understand the risks and technical details of the LayerZero Bridge process before proceeding. It is crucial to double-check all transaction details and addresses.
{% endhint %}

{% stepper %}
{% step %}
#### Access Bridge

To access the LayerZero Bridge, navigate to [bridge.doschain.com](https://bridge.doschain.com).
{% endstep %}

{% step %}
Connect your Wallet

1. Once you’re on the bridge page, locate and click on the “Connect Wallet” button.
2. Choose the appropriate wallet type from the list.
3. Follow the prompts to authorize the bridge to interact with your wallet.
{% endstep %}

{% step %}
#### Choose the Source and Destination

1. Once your wallet is connected, select the asset you wish to bridge from the drop-down list.
2. Choose the destination network (DOS mainnet or testnet) to which you want to bridge the asset.
3. Input the amount you wish to send and provide the destination address.
{% endstep %}

{% step %}
Confirm and Send

1. Review the transaction details thoroughly.
2. If everything is correct, click on the “Bridge” button to initiate the transaction.
3. Wait for the transaction to be confirmed on the source network and then on the DOS Chain.
{% endstep %}
{% endstepper %}

## FAQ

<details>

<summary>What are OFTs?</summary>

OFTs refer to Omnichain Fungible Tokens. These tokens are designed to seamlessly traverse various blockchain networks, making them highly versatile. With OFTs, you can send a token across existing chains and even future ones without the need for liquidity or incurring additional fees.



</details>

<details>

<summary>How do I add my token to the DOS Bridge?</summary>

If you’re a token owner or developer and want your token to be available for bridging on the DOS Chain, please reach out by contacting [the DOS team](mailto:hi@doschain.com).

</details>

<details>

<summary>Are there any fees associated with the bridge?</summary>

While DOS Chain sponsors all the fees of the destination chain when you bridge your tokens to DOS, LayerZero also applies a fee depending on the source network, to cover the transaction costs for bridging. Make sure to always double-check the displayed fee when using [bridge.doschain.com](https://bridge.onbeam.com/). Users should also be aware of standard network transaction fees which might be applicable depending on the source or destination network.

</details>

<details>

<summary>Is the DOS Bridge safe?</summary>

The DOS Bridge is built on top of the robust and secure [LayerZero framework](https://layerzero.network/). However, as with all cryptocurrency transactions, users are advised to be cautious, double-check all details, and understand the risks involved.

</details>

<details>

<summary>How to test the DOS Bridge?</summary>

Test token contracts have been deployed to DOS Testnet and Fuji for bridging purposes.

On the _DOS_ testnet, you can obtain DOS tokens from our test faucet tool [https://faucet.doschain.com](https://faucet.doschain.com/)

On _Fuji_, you can bridge the native AVAX token. Obtain these from the [official faucet](https://core.app/tools/testnet-faucet). Once you’ve acquired the tokens, utilize [bridge.doschain.com](https://bridge.doschain.com) as you normally would to transfer them to the DOS Testnet.

</details>

