# Color API
Color API is a library for interacting with applications built on the Color SDK.

## Install

```
yarn add @rnssolution/color-api
```

## Use

Simple example of how to send tokens.

```
import Color from "@rnssolution/color-api"

const STARGATE_URL = "https://proxy.testnet.color-platform.org:9061"
const ADDRESS = "colors1abcd1234"
const color = Color(STARGATE_URL, ADDRESS)

// create the transaction object
const msg = color
  .MsgSend({toAddress: 'colors1abcd09876', amounts: [{ denom: 'CLR', amount: 10 }})

// estimate the needed gas amount
const gasEstimate = await msg.simulate()

// create a signer
const ledgerSigner = ... // async (signMessage: string) => { signature: Buffer, publicKey: Buffer }

// send the transaction
const { included }= await msg.send({ gas: gasEstimate }, ledgerSigner)

// await tx to be included in a block
await included()
```

## API

If you want to query data only, you don't need to specify an address.

```
import { API } from "@rnssolution/color-api"

const STARGATE_URL = "https://proxy.testnet.color-platform.org:9061"

const api = API(STARGATE_URL)

const validators = await api.validators()
```

### Create a sign message to sign with on a Ledger or with any other signer

```
const { signWithPrivateKey } = require('@rnssolution/color-keys');
const { createSignMessage } = require('@rnssolution/color-api');

const stdTx = {
  msg: [
    {
      type: `color/Send`,
      value: {
        inputs: [
          {
            address: `colors1qperwt9wrnkg5k9e5gzfgjppzpqhyav5j24d66`,
            coins: [{ denom: `CLR`, amount: `1` }]
          }
        ],
        outputs: [
          {
            address: `colors1yeckxz7tapz34kjwnjxvmxzurerquhtrmxmuxt`,
            coins: [{ denom: `CLR`, amount: `1` }]
          }
        ]
      }
    }
  ],
  fee: { amount: [{ denom: ``, amount: `0` }], gas: `21906` },
  signatures: null,
  memo: ``
}

const signMessage = createSignMessage(stdTx, { sequence, accountNumber, chainId });
const signature = signWithPrivateKey(signMessage, Buffer.from(wallet.privateKey, 'hex'));
```

### Create and sign a transaction from a message which then is ready to be broadcast

```
const { signWithPrivateKey } = require('@rnssolution/color-keys');
const { createSignedTransaction } = require('@rnssolution/color-api');

const sendMsg = {
  type: `color/Send`,
  value: {
    inputs: [
      {
        address: `colors1qperwt9wrnkg5k9e5gzfgjppzpqhyav5j24d66`,
        coins: [{ denom: `CLR`, amount: `1` }]
      }
    ],
    outputs: [
      {
        address: `colors1yeckxz7tapz34kjwnjxvmxzurerquhtrmxmuxt`,
        coins: [{ denom: `CLR`, amount: `1` }]
      }
    ]
  }
}

const signer = signMessage = > signWithPrivateKey(signMessage, Buffer.from(wallet.privateKey, 'hex'))

const signMessage = createSignedTransaction({ gas: 1000, gasPrices = [{ amount: "10", denom: "uatom" }], memo = `Hi from Color Platform` }, [sendMsg], signer, chainId: "test-chain", accountNumber: 0, sequence: 12);
```
