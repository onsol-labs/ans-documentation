# SDK

### How to integrate ANS protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Direct Lookup

In order to get the information of a domain name you need to:

1. Get the domain name public key
2. Retrieve the account info

```typescript
import { getDomainKey, NameRegistryState } from "@bonfida/spl-name-service";

const domainName = "bonfida"; // With or without the .sol at the end

// Step 1
const { pubkey } = await getDomainKey(domainName);

// Step 2
// The registry object contains all the info about the domain name
// The NFT owner is of type PublicKey | undefined
const { registry, nftOwner } = await NameRegistryState.retrieve(
  connection,
  pubkey
);

// Subdomain derivation
const subDomain = "dex.bonfida"; // With or without the .sol at the end
const { pubkey: subKey } = await getDomainKey(subDomain);

// Record derivation (e.g IPFS record)
const record = "IPFS.bonfida"; // With or without the .sol at the end
const { pubkey: recordKey } = await getDomainKey(record, true);

```





