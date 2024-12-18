# Records

The following records are currently available on AllDomains Protocol:

| Name     | Value                |
| -------- | -------------------- |
| IPFS     | An IPFS CID          |
| ARWV     | An Arweave address   |
| SOL      | An address           |
| ETH      | An ETH address       |
| BTC      | A BTC address        |
| LTC      | An LTC address       |
| DOGE     | An DOGE address      |
| email    | An Email address     |
| url      | A website URL        |
| discord  | A Discord username   |
| github   | A Github username    |
| reddit   | A Reddit username    |
| twitter  | A Twitter username   |
| telegram | A Telegram username  |
| pic      | A Profile picture    |
| SHDW     | A SHDW DRIVE address |



#### Resolving a record:

```typescript
import { NameRecordHeader, getDomainKey, Record } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';
const connection = new Connection(RPC_URL);

//domain
const domain = "vlad.abc";

/**
we are getting the IPFS record value here but any from the list can be used
**/
const recordPubkey = (await getDomainKey(Record.IPFS + "." + domain, true)).pubkey
const nameRecord = await NameRecordHeader.fromAccountAddress(connection, recordPubkey);
const idx = nameRecord?.data?.indexOf(0x00);

//get the record value
const recordValue = nameRecord?.data?.subarray(0, idx).toString();
```
