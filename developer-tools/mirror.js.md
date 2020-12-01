# Mirror.js

{% hint style="info" %}
This section provides a brief guide on how to get set up with Mirror.js. For more information, be sure to check out source code and documentation on [GitHub](https://github.com/Mirror-Protocol/mirror.js).
{% endhint %}

Mirror.js is a client SDK for building applications that can interact with Mirror Protocol from within JavaScript runtimes, such as web browsers, server backends, and on mobile through React Native.

## Getting Mirror.js

Mirror.js is available as a package on NPM.

Add `@mirror-protocol/mirror.js` to your JavaScript project's `package.json` as a dependency using your preferred package manager:

```text
$ npm install -S @mirror-protocol/mirror.js
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

