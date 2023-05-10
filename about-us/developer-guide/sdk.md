# SDK

### How to integrate ANS protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Direct Lookup

The following code shows how to get the owner of a domain name

```typescript
import { TldParser } from "@onsol/tldparser";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// the ANS domain you want to resolve the public key for
const domain = "miester.abc";

// initialize
const connection = new Connection(RPC_URL);
const parser = new TldParser(connection);

// get the owner of a domain name
const owner = await parser.getOwnerFromDomainTld(domain);

```





