# Structure/Architecture

NameRecord:

```rust
#[account]
pub struct NameRecordHeader {
    // Names are hierarchical.  `parent_name` contains the account address of the parent
    // name, or `Pubkey::default()` if no parent exists.
    pub parent_name: Pubkey,

    // The owner of this name
    pub owner: Pubkey,

    // The class of data this account represents
    // If `Pubkey::default()` the data is unspecified.
    pub nclass: Pubkey,

    // expires_at is an unix timestamp in UTC.
    // programs must respect the expiry_at. after by which a rent is void.
    // the data is invalid unless extended by the owner or
    // new owner comes with new data replacing the old one.
    // defaults to 0
    // if it is 0, the domain is forever
    pub expires_at: u64,

    // time in unix timestamp in utc.
    // when the name account has been created.
    pub created_at: u64,

    // immutable owner domains
    pub non_transferable: bool,
}
```
