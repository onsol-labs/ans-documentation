# AD SDK

### How to integrate AllDomains Protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Domain Resolution

The following code shows how to get the Public Key owner of a domain name

* works with **any AllDomomains TLD**

```typescript
import { TldParser } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the owner pubkey of a domain name
async function resolveDomain(domain){

    // initialize a Tld Parser
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



#### 3. Get all domains owned by a Public Key

```typescript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the all the domains of owned by a user public key
async function getOwnerDomains(owner){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // list of name record header public keys owned by a user
    const domainRecordPks = await parser.getAllUserDomains(owner);
    let domains = [];
    for (var recordPubkey of domainRecordPks) {
        //get the name record of a domain pk
        const nameRecord = await NameRecordHeader.fromAccountAddress(connection, new PublicKey(recordPubkey));
        
        //get the parent name record of a domain pk
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(connection, nameRecord.parentName);
        
        //get the tld
        const tld = await parser.getTldFromParentAccount(nameRecord.parentName);
        
        //get the domain in string form
        const domain = await parser.reverseLookupNameAccount(recordPubkey, parentNameRecord?.owner);
        
        domains.push(`${domain}${tld}`);
    }
      
    return domains;
}

//get the all owner domains
getOwnerDomains(new PublicKey(""));
```



#### 4. Get all domains owned by a Public Key in a specific TLD

```typescript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the all the domains of owned by a user public key
async function getOwnerDomains(owner, tld){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // list of name record header publickeys owned by user in a tld
    const ownedTldDomains = await parser.getAllUserDomainsFromTld(owner, tld);
    let domains = [];
    for (var recordPubkey of domainRecordPks) {
        //get the name record of a domain pk
        const nameRecord = await NameRecordHeader.fromAccountAddress(connection, new PublicKey(recordPubkey));
        
        //get the parent name record of a domain pk
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(connection, nameRecord.parentName);
        
        //get the domain in string form
        const domain = await parser.reverseLookupNameAccount(recordPubkey, parentNameRecord?.owner);
        
        domains.push(`${domain}.${tld}`);
    }
      
    return domains;
}

//get all owned domains in the ".abc" Tld, without the "."
getOwnerDomains(new PublicKey(""), "abc");

//get all owned domains in the ".bonk" Tld
getOwnerDomains(new PublicKey(""), "bonk");

//get all owned domains in the ".poor" Tld
getOwnerDomains(new PublicKey(""), "poor");
```

####

#### 5.  Get all active AllDomains TLDs

```typescript
import { getAllTld } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

//get all active AllDomains TLDs
const allTlds = await getAllTld(connection);
```
