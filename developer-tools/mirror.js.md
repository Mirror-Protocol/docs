# Mirror.js SDK

{% hint style="danger" %}
THIS SECTION IS UNDER CONSTRUCTION
{% endhint %}

Mirror.js is a client SDK for building applications that can interact with Mirror Protocol from within JavaScript runtimes, such as web browsers, server backends, and on mobile through React Native.

## Getting Mirror.js

Add the following dependency using your package manager:

```text
yarn add @mirror.js/mirror
```

## Broadcasting Transactions

Mirror.js is intended to be used in tandem with Terra.js to product and sign transactions.

```typescript
import { Mirror } from '@mirror.js/mirror';
import { MnemonicKey } from '@terra.js/mnemonic-key';
import { LCDClient, isTxError } from '@terra.js/lcd-client';

const mirror = Mirror();
const terra = new LCDClient({
  URL: 'https://lcd.terra.dev',
  chainID: 'columbus-4'
});

const mk = new MnemonicKey({
  mnemonic: '...'
});

const wallet = terra.wallet(mk);

const main = async () => {
  const tx = await wallet.createAndSignTx({
    msgs: [mirror.mint.openPosition(...)]
  });

  const result = await terra.tx.broadcast(tx);
  if (isTxError()) {
    throw new Error(`TX was not included in block. ${result.rawlog}`);
  }
};

main().catch(console.error);
```

## Command Line Interface

&lt;TO BE ADDED&gt;

