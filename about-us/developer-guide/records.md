# Records

The following records are currently available on ANS Protocol:

| Name     | Value                |
| -------- | -------------------- |
| IPFS     | An IPFS CID          |
| ARWV     | An Arweave address   |
| SOL      | A address            |
| ETH      | An ETH address       |
| BTC      | A BTC address        |
| LTC      | A LTC address        |
| DOGE     | A DOGE address       |
| email    | An Email address     |
| url      | A website URL        |
| discord  | A discord username   |
| github   | A Github username    |
| reddit   | A Reddit username    |
| twitter  | A Twitter username   |
| telegram | A Telegram username  |
| pic      | A Profile picture    |
| SHDW     | A SHDW DRIVE address |



#### Resolving a record:

```typescript
import { NameRecordHeader, getDomainKey } from "@onsol/tldparser";
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
