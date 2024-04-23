# AD SDK

### How to integrate AllDomains Protocol:

#### 1. Installation

```
npm install @onsol/tldparser
```

#### 2. Domain Resolution

The following code shows how to get the Public Key owner of a domain name

* works with **any AllDomains TLD**

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



#### 3. Get domains owned by a Public Key

**a. Get all owned domains**

```typescript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get all the domains owned by a public key
async function getOwnedDomains(owner: PublicKey){

    // initialize a Tld Parser
    const parser = new TldParser(connection);

    // get all owned domains
    let allUserDomains = await parser.getParsedAllUserDomains(owner);

    return allUserDomains;
}

// get all owned domains
getOwnedDomains(new PublicKey(""));
```

**b. Get only unwrapped owned domains**

```typescript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get only the unwrapped domains owned by a public key
async function getOwnedUnwrappedDomains(owner: PublicKey){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get only unwrapped domains
    let unwrappedUserDomains = await parser.getParsedAllUserDomainsUnwrapped(owner);

    return unwrappedUserDomains;
}

// get only unwrapped owned domains
getOwnedUnwrappedDomains(new PublicKey(""));
```



#### 4. Get domains owned by a Public Key from a specific TLD

**a. Get all owned domains from a specific TLD**

<pre class="language-typescript"><code class="lang-typescript">import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get the all the domains owned by a public key in a TLD
<strong>async function getOwnedDomainsFromTld(owner, tld){
</strong>
    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get all owned domains from a TLD
    let ownedDomainsFromTld = parser.getParsedAllUserDomainsFromTld(owner, tld);

    return ownedDomainsFromTld;
}

//get all owned domains in the ".abc" Tld, without the "."
getOwnedDomainsFromTld(new PublicKey(""), "abc");

//get all owned domains in the ".bonk" Tld, without the "."
getOwnedDomainsFromTld(new PublicKey(""), "bonk");

//get all owned domains in the ".poor" Tld, without the "."
getOwnedDomainsFromTld(new PublicKey(""), "poor");
</code></pre>

**b. Get all only unwrapped owned domains from a specific TLD**

```typescript
import { TldParser, NameRecordHeader } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

// get only the unwrapped domains owned by a public key from a TLD
async function getOwnedUnwrappedDomainsFromTld(owner, tld){

    // initialize a Tld Parser
    const parser = new TldParser(connection);
    
    // get only unwrapped domains from a TLD
    let ownedUnwrappedDomainsFromTld = parser.getParsedAllUserDomainsFromTldUnwrapped(owner, tld);

    return ownedUnwrappedDomainsFromTld;
}

//get owned unwraped domains in the ".abc" Tld, without the "."
getOwnedUnwrappedDomainsFromTld(new PublicKey(""), "abc");

//get owned unwraped domains in the ".bonk" Tld
getOwnedUnwrappedDomainsFromTld(new PublicKey(""), "bonk");

//get owned unwraped domains in the ".poor" Tld
getOwnedUnwrappedDomainsFromTld(new PublicKey(""), "poor");
```

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

#### 6.  Get all domains registered in All TLDs

```typescript
import { NameRecordHeader, findAllDomainsForTld, getAllTld } from "@onsol/tldparser";
import { Connection } from "@solana/web3.js";

const RPC_URL = 'https://api.mainnet-beta.solana.com';

// initialize a Solana Connection
const connection = new Connection(RPC_URL);

//this code doesn't check if a domain is expired or not
async function getAllRegisteredDomains(connection: Connection){
    //get all TLDs
    const allTlds = await getAllTld(connection);

    let domains = [];
    for (let tld of allTlds) {

        //get the parent name record for a TLD
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(connection, tld.parentAccount);
            
        //get all name accounts in a specific TLD
        const allNameAccountsForTld = await findAllDomainsForTld(connection, tld.parentAccount);

        //parse all name accounts in a specific TLD
        for (let nameAccount of allNameAccountsForTld) {

            //get domain as string without the tld
            const domain = await parser.reverseLookupNameAccount(nameAccount, parentNameRecord?.owner);
            domains.push(                {
                nameAccount: nameAccount,
                domain: `${domain}${tld.tld}`
            });
        }
    }
    return domains;
}

//get all domains registered on AllDomains
console.log(await getAllRegisteredDomains(connection))
```

7. #### &#x20;Get User Main Domain&#x20;

```typescript
import { MainDomain, findMainDomain } from "@onsol/tldparser";
import { Connection, PublicKey } from "@solana/web3.js";


// returns MainDomain struct if it is available. it will return undefined if it is not.
// MainDomains are usually shown as mainDomain.domain + mainDomain.tld 
// the mainDomain.nameAccount is proof that the user has set it.
const fetchMainDomain = async (connection: Connection, pubkey: string | PublicKey): Promise<MainDomain | undefined> => {
    if (typeof pubkey === "string") {
        pubkey = new PublicKey(pubkey)
    }
    const [mainDomainPubkey] = findMainDomain(pubkey);
    let mainDomain = undefined;
    try {
        mainDomain = await MainDomain.fromAccountAddress(
            connection,
            mainDomainPubkey,
        );
        return mainDomain
    } catch (e) {
        console.log("No main domain found")
    }
    return mainDomain
};


const CONNECTION = new Connection("");
console.log(await fetchMainDomain(CONNECTION, "2EGGxj2qbNAJNgLCPKca8sxZYetyTjnoRspTPjzN2D67"))

// MainDomain {
//     nameAccount: PublicKey [PublicKey(9YzfCEHb62bQ47snUyjkxhC9Eb6y7CSodK3m8CKWstjV)] {
//       _bn: <BN: 7f0fb1f72ae0af9c5e7f5e4190d02ed2a720e88fb5787425157b9a9ec3fc39ec>
//     },
//     tld: '.abc',
//     domain: 'miester'
//   }
```

#### 8. Batch get AllDomains Domains

```typescript
import {
    ANS_PROGRAM_ID,
    findNameHouse,
    findNftRecord,
    getAllTld,
    getHashedName,
    getNameAccountKeyWithBump,
    NameRecordHeader,
    NftRecord,
    TldParser,
} from "@onsol/tldparser";
import {
    Connection,
    GetProgramAccountsResponse,
    PublicKey,
} from "@solana/web3.js";
import { setTimeout } from "timers/promises";

const connection = new Connection("");

async function findAllDomainsForTld(
    connection: Connection,
    parentAccount: PublicKey,
): Promise<{ pubkey: PublicKey; nameRecordHeader: NameRecordHeader }[]> {
    const filters: any = [
        {
            memcmp: {
                offset: 8,
                bytes: parentAccount.toBase58(),
            },
        },
    ];

    const accounts: GetProgramAccountsResponse =
        await connection.getProgramAccounts(ANS_PROGRAM_ID, {
            filters: filters,
        });
    return accounts.map((a) => {
        return {
            pubkey: a.pubkey,
            nameRecordHeader: NameRecordHeader.fromAccountInfo(a.account),
        };
    });
}

export async function performReverseLookupBatched(
    connection: Connection,
    nameAccounts: PublicKey[],
    tldHouse: PublicKey,
): Promise<(string | undefined)[]> {
    let reverseLookupDomains: (string | undefined)[] = [];

    while (nameAccounts.length > 0) {
        const currentBatch = nameAccounts.splice(0, 100);

        const promises = currentBatch.map(async (nameAccount) => {
            const reverseLookupHashedName = await getHashedName(
                nameAccount.toBase58(),
            );
            const [reverseLookUpAccount] = getNameAccountKeyWithBump(
                reverseLookupHashedName,
                tldHouse,
                undefined,
            );
            return reverseLookUpAccount;
        });

        const reverseLookUpAccounts: PublicKey[] = await Promise.all(promises);
        const reverseLookupAccountInfos =
            await connection.getMultipleAccountsInfo(reverseLookUpAccounts);

        const batchDomains = reverseLookupAccountInfos.map(
            (reverseLookupAccountInfo) => {
                const domain = reverseLookupAccountInfo?.data
                    .subarray(200, reverseLookupAccountInfo?.data.length)
                    .toString();
                return domain;
            },
        );

        reverseLookupDomains = reverseLookupDomains.concat(batchDomains);
    }

    return reverseLookupDomains;
}

// onlyDomains will grab only domains and no nfts
async function getAllRegisteredDomains(
    tldExpected?: string,
    onlyDomains: boolean = false,
) {
    const parser = new TldParser(connection);
    //get all TLDs
    const allTlds = await getAllTld(connection);

    const domains = [];
    for (const tld of allTlds) {
        if (tldExpected) {
            if (tld.tld != tldExpected) continue;
        } //get the parent name record for a TLD
        const parentNameRecord = await NameRecordHeader.fromAccountAddress(
            connection,
            tld.parentAccount,
        );
        if (!parentNameRecord) continue;
        if (!parentNameRecord.owner) continue;

        //get all name accounts in a specific TLD
        const allNameAccountsForTld = await findAllDomainsForTld(
            connection,
            tld.parentAccount,
        );
        await setTimeout(50);
        const [nameHouseAccount] = findNameHouse(tld.parentAccount);

        const nameAccountPubkeys = allNameAccountsForTld.map((a) => a.pubkey);

        const domainsReverse = await performReverseLookupBatched(
            connection,
            nameAccountPubkeys,
            parentNameRecord.owner,
        );

        let index = 0;
        for (const domain of domainsReverse) {
            const [nftRecord] = findNftRecord(
                allNameAccountsForTld[index].pubkey,
                nameHouseAccount,
            );
            let finalOwner =
                allNameAccountsForTld[index].nameRecordHeader.owner?.toString();
            if (finalOwner == nftRecord.toString() && !onlyDomains) {
                const nftRecordData = await NftRecord.fromAccountAddress(
                    connection,
                    nftRecord,
                );
                const largestAccounts =
                    await connection.getTokenLargestAccounts(
                        nftRecordData.nftMintAccount,
                    );
                if (largestAccounts.value.length > 0) {
                    const largestAccountInfo =
                        await connection.getParsedAccountInfo(
                            largestAccounts.value[0].address,
                        );
                    if (largestAccountInfo?.value?.data) {
                        finalOwner = new PublicKey(
                            // @ts-ignore
                            largestAccountInfo.value.data.parsed.info.owner,
                        ).toString();
                    }
                }
                await setTimeout(50);
            }
            domains.push({
                nameAccount: allNameAccountsForTld[index].pubkey,
                domain: `${domain}${tld.tld}`,
                owner: finalOwner,
                expiresAt:
                    allNameAccountsForTld[index].nameRecordHeader.expiresAt,
                createdAt:
                    allNameAccountsForTld[index].nameRecordHeader.createdAt,
            });
            index += 1;
        }
    }
    return domains;
}

async function main() {
    // or ".eyekon" or ".superteam" or ".monke"
    // if tldExpected is undefined it will grab all domains
    const tldExpected: string | undefined = ".zk";
    // if set true it will grab only domains and no nfts
    const onlyDomains = false;
    const domains = await getAllRegisteredDomains(tldExpected, onlyDomains);
    console.log(JSON.stringify(domains));
    // console.log(domains?.length)
}

// get all domains registered on AllDomains
main();
```
