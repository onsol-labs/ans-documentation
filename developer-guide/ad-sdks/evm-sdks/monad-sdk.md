# Monad SDK

## AllDomains Monad EVM SDK Documentation

### Overview

The AllDomains Monad EVM SDK is a library designed to interact with top-level domain (TLD) names on EVM-compatible chains (e.g., Monad). This SDK provides functionality to manage domain names, retrieve ownership details, and handle domain records across supported blockchains.

### Supported Chains

* Monad Testnet (EVM)

### Installation

To install the AllDomains Monad EVM SDK, add the following line to your `package.json` or run the command:

```bash
npm install @onsol/tldparser
```

### Usage Examples

#### 1. Initialize EVM Connection

```javascript
import { TldParser, NetworkWithRpc } from 'tld-parser';
import { getAddress } from 'ethers';

// Set up RPC URL and initialize a network instance
const RPC_URL = 'https://testnet-rpc.monad.xyz';
const PUBLIC_KEY = getAddress('0x94Bfb92da83B27B39370550CA038Af96d182462f');
const settings = new NetworkWithRpc('monad', 10143, RPC_URL);

// Create an instance of TldParser
const parser = new TldParser(settings, 'monad');
```

#### 2. Get All User Domains

```javascript
const allDomains = await parser.getAllUserDomains(PUBLIC_KEY);
// Output: [NameRecord { domain_name: "miester", tld: ".mon", ... }]
```

#### 3. Get Domains for a Specific TLD

```javascript
const tldDomains = await parser.getAllUserDomainsFromTld(PUBLIC_KEY, '.mon');
// Output: [NameRecord { domain_name: "miester", tld: ".mon", ... }]
```

#### 4. Get Domain Owner

```javascript
const owner = await parser.getOwnerFromDomainTld('miester.mon');
// Output: "0x94Bfb92da83B27B39370550CA038Af96d182462f"
```

#### 5. Get Name Record

```javascript
const nameRecord = await parser.getNameRecordFromDomainTld('miester.mon');
// Output: NameRecord { created_at, domain_name, expires_at, main_domain_address, tld, transferrable }
```

### Core Methods (Available on Both Chains)

* `getAllUserDomains(userAccount)`: Retrieves all domains owned by a user.
* `getAllUserDomainsFromTld(userAccount, tld)`: Retrieves domains for a specific TLD.
* `getOwnerFromDomainTld(domainTld)`: Retrieves the owner of a domain.
* `getNameRecordFromDomainTld(domainTld)`: Retrieves detailed record data for a domain.
* `getMainDomain(userAddress)`: Retrieves the user's main domain.

### Additional Concepts and Example Implementations

#### Domain Resolution

Use the following code to get the public key owner of a domain name:

```javascript
import { TldParser } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';
const connection = new Connection(RPC_URL);

async function resolveDomain(domain){
    const parser = new TldParser(connection);
    return await parser.getOwnerFromDomainTld(domain);
}

// Example of usage
resolveDomain("miester.abc");
```

#### Get All Domains Owned by a Public Key

```javascript
async function getOwnedDomains(owner) {
    const parser = new TldParser(connection);
    let allUserDomains = await parser.getParsedAllUserDomains(owner);
    return allUserDomains;
}

// Example usage:
getOwnedDomains(new PublicKey("YOUR_PUBLIC_KEY"));
```

#### Get All Active AllDomains TLDs

```javascript
import { getAllTld } from "@onsol/tldparser";

const allTlds = await getAllTld(connection);
```

#### Get All Registered Domains

```javascript
async function getAllRegisteredDomains(connection) {
    // Similar functionality as previously detailed.
}

// Example usage:
console.log(await getAllRegisteredDomains(connection));
```

#### Fetch User Main Domain

You can retrieve a user's main domain using the following code:

```javascript
const fetchMainDomain = async (connection, pubkey) => {
    const [mainDomainPubkey] = findMainDomain(pubkey);
    let mainDomain = undefined;
    try {
        mainDomain = await MainDomain.fromAccountAddress(
            connection,
            mainDomainPubkey,
        );
        return mainDomain;
    } catch (e) {
        console.error("No main domain found");
    }
    return mainDomain;
};

// Example usage:
const mainDomain = await fetchMainDomain
```
