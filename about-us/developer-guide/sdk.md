# SDK

### How to integrate ANS protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Domain Resolution

The following code shows how to get the owner of a domain name

* it works with any ANS TLD

```typescript
import { TldParser } from "@onsol/tldparser";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// get the owner pubkey of a domain name
async function resolveDomain(domain){
    // initialize
    const connection = new Connection(RPC_URL);
    const parser = new TldParser(connection);
    
    return await parser.getOwnerFromDomainTld(domain);
}

//get the owner pubkey of "miester.abc";
resolveDomain("miester.abc");

//get the owner pubkey of "miester.bonk"
resolveDomain("miester.bonk");

//get the owner pubkey of "miester.poor"
resolveDomain("miester.poor");

```





