# Architecture

This document serves as a starting point for new users to the DOS Chain ecosystem. General knowledge of cryptocurrency is assumed, and in particular familiarity with the Ethereum or Avalanche ecosystem. If you don't understand something right away, that's OK. Search for an answer online, and if you don't find it, ask on our [Discord](http://discord.gg/DOS).

### Architecture[​](https://docs.bnbchain.org/docs/getting-started#architecture)﻿ <a href="#id-5qh2j" id="id-5qh2j"></a>

DOS Chain utilizes the L1s technology of Avalanche. Before you start developing DApps on the DOS Chain, it is important to learn about its underlying architecture.

An Avalanche L1 is a sovereign network which defines its own rules regarding its membership and token economics. It is composed of a dynamic subset of Avalanche validators working together to achieve consensus on the state of one or more blockchains. Each blockchain is validated by exactly one Avalanche L1, while an Avalanche L1 can validate many blockchains.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

### Ecosystem Account <a href="#fxqvi" id="fxqvi"></a>

DOS Chain has partnered with Openfort to provide the end-users and developers a better experience on Web3. DOS Chain tools are built on public goods infrastructure like the [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) or [ERC-6551](https://eips.ethereum.org/EIPS/eip-6551) standards, which are scalable and make them simple to use.

DOSafe, our ecosystem wallet, is not a 1-to-1 mapping of any wallet solution you've seen. While we offer many of the features that other solutions have, we go much further than anyone else because: (1) We're based on smart accounts not EOAs, and (2) we build the server stack so you don't have to worry about the reliability of the infrastructure.&#x20;

DOSafe consists of modules that allow you to plug-and-play the tools to achieve your success.

Our infrastructure orchestrates everything under the hood to make the development and maintenance easy. The 4 key components we take care of are:

* **Smart accounts**: We offer optimized and diverse set of smart accounts to fit your needs. [Types of smart accounts](https://github.com/openfort-xyz/openfort-contracts).
* **Embedded Signers**: Signers create non-custodial wallets for your users. [Embedded wallet](https://www.openfort.xyz/docs/guides/javascript/wallets).
* **Bundlers/RPCs**: We built a Meta Bundler Network to ensure reliable access to the blockchain. If one fails, another takes over.
* **Paymasters**: We built the most customizable paymasters to configure granular rate limits and policies for gas sponsoring, through either our API or dashboard.
* **Backend wallets**: These are externally-owned accounts to be used internally to manage the projects flows and experience. They can execute logic like escrow for competition or sending a minted asset to a user after winning a competition.

<figure><img src="https://blog-cms.openfort.xyz/uploads/Group_295_min_3ec4bfb35f.png" alt=""><figcaption></figcaption></figure>

### Advantages <a href="#advantages" id="advantages"></a>

#### Independent Networks <a href="#independent-networks" id="independent-networks"></a>

* DOS Chain uses virtual machines to specify its own execution logic, determine its own fee regime, maintain its own state, facilitate its own networking, and provide its own security.
* DOS's performance is isolated from other Avalanche L1s in the ecosystem, so increased usage on other Avalanche L1s won't affect DOS.
* DOS has its own token economics with its own native tokens, gasless support, and incentives.
* DOS can host multiple blockchains with customized [virtual machines](https://build.avax.network/docs/quick-start/virtual-machines).

#### Native Interoperability <a href="#native-interoperability" id="native-interoperability"></a>

Avalanche Warp Messaging enables native cross-Avalanche L1 communication and allows Virtual Machine (VM) developers to implement arbitrary communication protocols between DOS and any other Avalanche L1.

#### Accommodate App-Specific Requirements <a href="#accommodate-app-specific-requirements" id="accommodate-app-specific-requirements"></a>

Different blockchain-based applications may require validators to have certain properties such as large amounts of RAM or CPU power.

DOS Chain could require that validators meet certain hardware requirements so that the application doesn't suffer from low performance due to slow validators.

#### Networks Designed With Compliance <a href="#launch-networks-designed-with-compliance" id="launch-networks-designed-with-compliance"></a>

DOS's architecture makes regulatory compliance manageable. Some examples of requirements the DOS may choose include:

* Validators must be located in a given country.
* Validators must pass KYC/AML checks.
* Validators must hold a certain license.

#### Control Privacy of On-Chain Data <a href="#control-privacy-of-on-chain-data" id="control-privacy-of-on-chain-data"></a>

DOS is ideal for organizations interested in keeping their information private.

Institutions conscious of their stakeholders' privacy can create private blockchains of DOS where the contents of the blockchains would be visible only to a set of pre-approved validators.



﻿﻿
