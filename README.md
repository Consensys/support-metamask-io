# MetaMask Help Center Content

The MetaMask Help Center lives at https://www.support.metamask.io.

The code for the site is private, but this repository exists to allow anyone to contribute, correct, or suggest changes to the content, or the functionality of the site as a whole. Changes to this repository will be routinely reviewed and synced with the live site content.

## How to contribute

See something missing? Is there an error in an article, a broken link? Is something really frustrating about the site? We love this feedback! 

To report something, create an issue [here](https://github.com/Consensys/support-metamask-io/issues).

Alternatively, help us improve our documentation! [Fork our repo](https://github.com/Consensys/support-metamask-io/fork), create a pull request, and tag us for review! (for help on this, see below)

### How to submit a suggestion or change

The best way to suggest a change to these docs is through a process known as a **pull request**. If you're not familiar with how that works, check out [GitHub's guide here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request).

If that process is too involved for you, you can always open a thread on the [MetaMask Community page](https://community.metamask.io).

## Resources

For help on formatting your submission correctly, check out these guidelines:

- [Format your Markdown](https://docs-template.consensys.net/contribute/format-markdown) correctly.
import Onboard from '@web3-onboard/core'
import injectedModule from '@web3-onboard/injected-wallets'
import { ethers } from 'ethers'

const MAINNET_RPC_URL = 'https://mainnet.infura.io/v3/<INFURA_KEY>'

const injected = injectedModule()

const onboard = Onboard({
  wallets: [injected],
  chains: [
    {
      id: '0x1',
      token: 'ETH',
      label: 'Ethereum Mainnet',
      rpcUrl: MAINNET_RPC_URL
    },
    {
      id: '0x2105',
      token: 'ETH',
      label: 'Base',
      rpcUrl: 'https://mainnet.base.org'
    }
  ]
})

const wallets = await onboard.connectWallet()

console.log(wallets)

if (wallets[0]) {
  // create an ethers provider with the last connected wallet provider
  const ethersProvider = new ethers.providers.Web3Provider(wallets[0].provider, 'any')
  // if using ethers v6 this is:
  // ethersProvider = new ethers.BrowserProvider(wallet.provider, 'any')

  const signer = ethersProvider.getSigner()

  // send a transaction with the ethers provider
  const txn = await signer.sendTransaction({
    to: '0x',
    value: 100000000000000
  })

  const receipt = await txn.wait()
  console.log(receipt)
}
