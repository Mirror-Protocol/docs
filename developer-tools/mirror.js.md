# Mirror.js

{% hint style="info" %}
This section provides a brief guide on how to get set up with Mirror.js. For more information, be sure to check out source code and documentation on [GitHub](https://github.com/Mirror-Protocol/mirror.js).
{% endhint %}

Mirror.js is a client SDK for building applications that can interact with Mirror Protocol from within JavaScript runtimes, such as web browsers, server backends, and on mobile through React Native.

You can find a reference of the Mirror.js API [here](https://mirror-protocol.github.io/mirror.js/).

## Getting Mirror.js

Mirror.js is available as a package on [NPM](https://www.npmjs.com/package/@mirror-project/mirror.js) and is intended to be used alongside [Terra.js](https://www.npmjs.com/package/@terra-money/terra.js). 

Add both:

* `@terra-money/terra.js`
* `@mirror-protocol/mirror.js`

To your JavaScript project's `package.json` as dependencies using your preferred package manager:

```text
$ npm install -S @terra-money/terra.js @mirror-protocol/mirror.js
```

## Usage

Mirror.js provides facilities for 2 main use cases:

* query: runs smart contract queries through LCD
* execute: creates proper `MsgExecuteContract` objects to be used in transactions

### Querying



### Executing

Mirror.js is intended to be used in tandem with Terra.js to produce transactions.

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

