# SDK

### How to integrate ANS protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Direct Lookup

The following code shows how to get the owner of a domain name

* it works with any ANS domain

```typescript
import { TldParser } from "@onsol/tldparser";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// the ANS domain name you want to resolve the owner public key for
const domain = "miester.abc";

// initialize
const connection = new Connection(RPC_URL);
const parser = new TldParser(connection);

// get the owner pubkey of a domain name
const owner = await parser.getOwnerFromDomainTld(domain);

```





