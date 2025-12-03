# Monad SDK

## AllDomains Monad EVM SDK Documentation

### Overview

The AllDomains Monad EVM SDK is a library designed to interact with top-level domain (TLD) names on EVM-compatible chains (e.g., Monad). This SDK provides functionality to manage domain names, retrieve ownership details, and handle domain records across supported blockchains.

### Supported Chains

* Monad (EVM)

### Supported TLDs

* .mon
* .nad
* .chog
* .mokadel
* .molandak
* .mouch
* .moyaki
* .salmonad

### Installation

To install the AllDomains Monad EVM SDK, add the following line to your `package.json` or run the command:

```bash
npm install @onsol/tldparser
```

### Usage Examples

#### 1. Initialize EVM Connection

```javascript
import { TldParser, NetworkWithRpc } from '@onsol/tldparser';
import { getAddress } from 'ethers';

// Set up RPC URL and initialize a network instance
const RPC_URL = 'https://rpc.monad.xyz';
const PUBLIC_KEY = getAddress('0x94Bfb92da83B27B39370550CA038Af96d182462f');
const settings = new NetworkWithRpc('monad', 143, RPC_URL);

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

#### 6. Get Main Domain (Favourite Domain)

<pre class="language-javascript"><code class="lang-javascript">const ownedDomainsReceived = await parser.getMainDomain(PUBLIC_KEY) ;
<strong>// Output: &#x3C;NameRecord> {
</strong>//    created_at: '1795616523',
//    domain_name: 'miester',
//    expires_at: '1795616823',
//    main_domain_address: PUBLIC_KEY,
//    tld: '.mon',
//    transferrable: true,
// };
</code></pre>

### Core Methods (Available on Both Chains)

* `getAllUserDomains(userAccount)`: Retrieves all domains owned by a user.
* `getAllUserDomainsFromTld(userAccount, tld)`: Retrieves domains for a specific TLD.
* `getOwnerFromDomainTld(domainTld)`: Retrieves the owner of a domain.
* `getNameRecordFromDomainTld(domainTld)`: Retrieves detailed record data for a domain.
* `getMainDomain(userAddress)`: Retrieves the user's main domain.
