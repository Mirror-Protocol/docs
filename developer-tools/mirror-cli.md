---
description: Command-line interface for Mirror Protocol
---

# mirrorcli

{% hint style="info" %}
This section provides a brief guide on how to use Mirror Protocol via `mirrorcli`. For more information, please check its source code and documentation on [GitHub](https://github.com/Mirror-Protocol/mirrorcli).
{% endhint %}

`mirrorcli` is a command-line interface for Mirror Protocol on Terra and allows more advanced users to perform operations directly from their shell or terminal without having to interact with a graphical interface. `mirrorcli` is built on top of `terracli` and allows you to use keys saved in its keychain.

## Installation

### Requirements

* Make sure your have `terracli` installed. `terracli` is a binary that is shipped with [Terra Core](https://github.com/terra-project/core) and installed in your GOPATH.
* Have Node.js v10+ installed with NPM

You can install `mirrorcli` through NPM:

```bash
$ npm install -g mirrorcli
```

## Configuration

On first launch, `mirrorcli` will generate `~/.mirrorclirc.json` in your `$HOME` directory.

### Adding assets



### Specifying the Network

`mirrorcli` will work out-of-the-box with no configuration to access Mirror on `columbus-4`. However, you can edit the configuration to use a different network by changing the value of the `MIRRORCLI_NETWORK` environment variable.

#### Example

```
MIRRORCLI_NETWORK=tequila-0004 mirrorcli x mint [deposit ...]
```

OR

```bash
export MIRRORCLI_NETWORK=tequila-0004
mirrorcli x mint [deposit ...]
```

## Usage



