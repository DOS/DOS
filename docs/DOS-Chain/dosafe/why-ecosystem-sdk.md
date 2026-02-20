# Why Ecosystem SDK

### Why DOSafe SDK

Developing a user-friendly wallet poses a major challenge. To address this, we've introduced the DOSafe SDK—a toolkit that streamlines the creation, management, and customization of wallet systems. Ideal for games, marketplaces, or social platforms, DOSafe SDK supports

Explore the live demo of our ecosystem wallet!

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Why we build this

Developing a smart wallet system from scratch is a complex challenge. Key tasks include:

* **Infrastructure Preparation**: Set up for transaction sponsorship and bundling.
* **Signer Implementation**: Incorporate signers to manage ownership.
* **Smart Contract Selection**: Choose appropriate smart contracts for accounts.
* **Authentication Setup**: Establish secure authentication processes.

Choosing a third-party provider to manage your system can be a practical decision. However, it is essential for the provider to offer flexibility, allowing adjustments to meet your specific needs and reduce the risk of vendor lock-in. Additionally, prioritize a smooth developer experience to avoid wasting time on implementing complex interactions, like a wallet system.

### **What's in the box?**

Here's the cool stuff you get with the DOSafe SDK:

* **Universal Compatibility**: Our solution supports development across browsers, mobile platforms, and Unity.
* **Effortless Wallet Setup**: Users can swiftly create, fund, and start using their wallets.
* **Efficient User Management**: Acts as a gatekeeper, ensuring only authorized users gain access.

### **How it all fits together**

The DOSafe SDK is structured like a well-organized kitchen featuring three main stations:&#x20;

1\.     Developer Integration Layer: Where you set up your integration&#x20;

2\.     Client-Wallet Communication: The service window where orders (transactions) are handled&#x20;

3\.     Blockchain Interaction: Where the magic occurs blockchainGetting started

### DOSafe SDK Architecture

The DOSafe SDK is organized into three main components, each serving a specific purpose, similar to distinct stations in a well-organized kitchen:

1. **Developer Integration Layer**: This is where you configure your integration, setting up the necessary components for seamless functionality.
2. **Client-Wallet Communication**: This acts as the service window where transactions are initiated and processed.
3. **Blockchain Interaction**: This is the core area where blockchain magic happens, managing the intricate operations of

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### The main ingredients

#### Custom Wallet SDK Integration

This serves as the main coordinator for your application, managing everything from designing a custom wallet SDK to overseeing pop-ups and transactions. The best part? It operates directly within your app's front end and can be customized with your brand's colors. It also includes built-in security features and integrates seamlessly with

{% hint style="info" %}
The Client SDK supports EIP-1193, ensuring seamless integration with popular
{% endhint %}

**Communication Module**

Meet your dedicated maître d', designed to facilitate seamless interactions between clients and their wallets. It assists newcomers with setting up non-custodial wallets and operates smoothly in any browser environment. For more detailed information, please refer to our comprehensive

**Framework Prefabs**

Similar to ready-to-use recipe cards, our framework prefabs include special helpers for various frameworks, featuring an excellent setup for React. All components are customizable to suit your style.

### "But why not just use embedded wallets?"

#### **The Embedded Wallet setup**

Imagine this: each app requires a unique setup—installing packages, generating keys in iframes, managing authentication... And then, how do you make it discoverable across different applications?

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

#### The Not-So-Fun parts of embedded wallets[#](https://www.openfort.io/docs/guides/ecosystem/intro#the-not-so-fun-parts-of-embedded-wallets) <a href="#the-not-so-fun-parts-of-embedded-wallets" id="the-not-so-fun-parts-of-embedded-wallets"></a>

1. **Authentication gets messy**
   * Every app needs its own bouncer (authentication system)
   * OAuth2 providers get confused when trying to share wallet access
   * There's no good way to keep track of who's logged in where
2. **Less control than you'd like**
   * Your partners need access to the master keys (Admin credentials)
   * It's harder to keep track of who's spending what
   * No central control room for permissions

### Why the Ecosystem SDK is better <a href="#why-the-ecosystem-sdk-is-better" id="why-the-ecosystem-sdk-is-better"></a>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

What you get:

* One login system that works everywhere (no more authentication headaches!)
* Control all permissions from one place
* A slick dashboard with your brand to onboard projects
* Paymasters and smart accounts by default
* One solid security setup for everything
