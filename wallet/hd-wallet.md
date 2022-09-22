---
title: Metamask - Hierarchical Deterministic (HD) Wallet
---

<head>
  <title>Metamask - Hierarchical Deterministic (HD) Wallet  </title>
  <meta
    name="description"
    content="Metamask - Hierarchical Deterministic (HD) Wallet"
  />
  <style>{`
    :root {
      --doc-item-container-width: 60rem;
    }
  `}</style>
</head>

## Overview

At a high level, a wallet is an application that serves as the primary user interface. The wallet controls access to a user’s money, managing keys and addresses, tracking the balance, and creating and signing transactions.

More narrowly, from a programmer’s perspective, the word "wallet" refers to the data structure used to store and manage a user’s keys.

There are two primary types of wallets: _nondeterministic wallets and deterministic wallets_

## Nondeterministic (Random) Wallets

-   In random wallets, the keys are randomly generated values. Such wallets are being replaced with deterministic wallets because they are cumbersome to manage, back up, and import.
-   The disadvantage of random keys is that if you generate many of them you must keep copies of all of them, meaning that the wallet must be backed up frequently.
-   Each key must be backed up, or the funds it controls are irrevocably lost if the wallet becomes inaccessible.

## Deterministic (Seeded) Wallets

-   Deterministic, or "seeded," wallets are wallets that contain private keys that are all derived from a common seed, through the use of a one-way hash function.
-   In a deterministic wallet, the seed is sufficient to recover all the derived keys, and therefore a single backup at creation time is sufficient.

## HD Wallets

-   Deterministic wallets were developed to make it easy to derive many keys from a single "seed".
-   The most advanced form of deterministic wallets is the HD wallet defined by the BIP-32 standard.
-   HD wallets contain keys derived in a tree structure, such that a parent key can derive a sequence of children keys, each of which can derive a sequence of grandchildren keys, and so on, to an infinite depth.
-   Now let us see how keys are generated in HD wallets. For that, we are taking MetaMask. [MetaMask](https://metamask.io/) is the commonly used HD wallet to interact with the Ethereum blockchain.

## Mnemonic Code Words (Secret Recovery Phrases)

-   Let’s start with the **Secret Recovery Phrase.** When you first install MetaMask, you were shown a list of 12 words known as the Secret Recovery Phrase. MetaMask uses a specification known as the **BIP39** to generate these 12 words. [https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc)
-   These 12 words in the **Secret Recovery Phrase** are generated from a list of 2048 words. The complete list of 2048 words can be found from here: [https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt](https://github.com/bitcoin/bips/blob/master/bip-0039/english.txt)
    ![](../../../resources/wallet/mnemonic-seed.png)

## Creating an HD Wallet from the Seed

-   HD wallets are created from a single root seed, which is a 128, 256, or 512-bit random number. Most commonly, this seed is generated from a mnemonic as detailed in the previous section.
-   Every key in the HD wallet is deterministically derived from this root seed, which makes it possible to re-create the entire HD wallet from that seed in any compatible HD wallet. This makes it easy to back up, restore, export, and import HD wallets containing thousands or even millions of keys by simply transferring only the mnemonic that the root seed is derived from.
    ![](../../../resources/wallet/master-key.png)

## How MetaMask Creates Accounts

-   The master key is used to generate a sequence of child keys. Each of these child keys can act as a parent key and generate a series of keys, calling grandchildren of the master key.
-   This can be repeated any number of times, expanding the tree structure by increasing the number of levels.
-   The child key derivation functions are based on a one-way hash function that combines:
    -   A parent public key (ECDSA compressed key)
    -   A seed called a chain code (256 bits)
    -   An index number (32 bits)
-   The corresponding public key for the private key is derived from the private key using the **ECDSA** (_Elliptic Curve Signature_) algorithm.
-   From the public key derived, compute a **keccak256** hash
-   From the **keccak256** hash, take the last 20 bytes (which is 40 hexadecimal), prefix it with“0x”and this is your Ethereum address
    ![](../../../resources/wallet/child-key.png)

**There are a number of important features to note here:**

-   The Secret Recovery Phrase is the secret that controls the wallet. If someone has this secret, they have complete access to the wallet. MetaMask does not keep your SRP.
-   Your SRP is used locally to derive private keys, one per account/address. Accounts are stored on the blockchain, and these private keys unlock those accounts.
-   If you uninstall the app or the extension, then the local version of the data is gone.
-   Within your wallet, you can have a very large number of separate accounts. When MetaMask creates or restores your wallet from the Secret Recovery Phrase, it initially produces only the first account. However, any additional accounts you create can be re-created in a future instance of MetaMask; as the wallet is deterministic, it will always re-create the same accounts, in the same order.

## How MetaMask store your wallet secret?

**Keyring**

-   The keyring is the core concept of the secret storing and account management system in MetaMask. [KeyringController](https://github.com/MetaMask/KeyringController) is the implementation of the keyring.
-   Here is the visual reference of the keyring structure:
    ![](../../../resources/wallet/keyring.png)

-   The circular ring represents the seed phrase that is used to generate public key-private key pairs. Each key hangs on the ring is an individual wallet account with its private key drives from the seed phase. The seed phrase and all accounts data get bundled together, encrypted with an encryption key generated from the user password, and stores in the extension.
    ![](../../../resources/wallet/local-storage.png)

# Reference Link

-   [https://www.wispwisp.com/index.php/2020/12/25/how-metamask-stores-your-wallet-secret/](https://www.wispwisp.com/index.php/2020/12/25/how-metamask-stores-your-wallet-secret/)
-   [https://metamask.zendesk.com/hc/en-us/articles/4404722782107](https://metamask.zendesk.com/hc/en-us/articles/4404722782107)
-   [https://metamask.zendesk.com/hc/en-us/articles/4405451730331](https://metamask.zendesk.com/hc/en-us/articles/4405451730331)
-   [https://ethex-smm.medium.com/all-what-you-need-to-know-about-metamask-6f0a5fff49fd](https://ethex-smm.medium.com/all-what-you-need-to-know-about-metamask-6f0a5fff49fd)
-   [https://levelup.gitconnected.com/blockchain-series-how-metamask-creates-accounts-a8971b21a74b](https://levelup.gitconnected.com/blockchain-series-how-metamask-creates-accounts-a8971b21a74b)
-   [https://kbaiiitmk.medium.com/hierarchical-deterministic-hd-wallet-4b883faab955](https://kbaiiitmk.medium.com/hierarchical-deterministic-hd-wallet-4b883faab955#:~:text=MetaMask%20is%20the%20commonly%20used,Wallet%20using%20its%20browser%20extension)
-   [https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc)
