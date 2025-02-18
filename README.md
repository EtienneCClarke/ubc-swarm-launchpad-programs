# UBC/COMPUTE Swarm Launchpad Solana Programs

This repository contains the Solana programs used by Universal Basic Compute (UBC).

## Key Features

### Pool Management
- Create and manage share pools
- Configure pool settings including:
    - Total share supply
    - Fee ratios
    - Custodial accounts
- Pool admin controls for freezing/unfreezing trading
- Custom pool names and identifiers

### Share Management
- Track individual shareholder accounts
- Manage available and locked share balances
- Support for multiple share pools per user
- PDA-based shareholder account creation

### P2P Trading
- Create and manage listings
- Configurable price per share
- Support for different payment tokens
- Automatic fee distribution on trades
- Prevention of self-trading
- Share validation and balance checks

### Token Integration
- Support for SPL tokens as payment
- Automatic token account creation
- Multi-token support for trading
- Whitelisted token validation

### Security Features
- Authority validations on all transactions
- Pool-level trading controls
- Share balance validations
- Whitelisted program and token checks
- Protected custodial account operations

### Event Tracking
- Detailed transaction logging
- Event emission for:
  - Primary market sales
  - P2P Trades

## Account Structure

### Pool Account
```rust
pub struct Pool {
    #[max_len(32)]
    pub pool_name: String,          // 32 bytes
    pub admin_authority: Pubkey,    // 32 bytes
    pub total_shares: u64,          // 8 bytes
    pub available_shares: u64,      // 8 bytes
    pub is_frozen: bool,            // 1 byte

    // Fees
    pub ubc_mint: Pubkey,           // 32 bytes
    pub compute_mint: Pubkey,       // 32 bytes
    pub fee_ratio: u64,             // 8 bytes

    // Recipients
    pub custodial_account: Pubkey,  // 32 bytes
}
```

### Shareholder Account
```rust
pub struct Shareholder {
    pub pool: Pubkey,          // 32 bytes
    pub owner: Pubkey,         // 32 bytes
    pub shares: u64,           // 8 bytes
    pub available_shares: u64, // 8 bytes
}
```

### P2P Listing Account
```rust
pub struct ShareListing {
    pub pool: Pubkey,              // 32 bytes
    pub seller: Pubkey,            // 32 bytes
    pub shareholder: Pubkey,       // 32 bytes
    pub number_of_shares: u64,     // 8 bytes
    pub price_per_share: u64,      // 8 bytes
    pub desired_token: Pubkey,     // 32 bytes
    #[max_len(32)]
    pub listing_id: String, // 32 bytes
}
```

## Deploying

### Building the program

First, build the program using a nightly version until this [issue](https://solana.stackexchange.com/questions/17777/unexpected-cfg-condition-value-solana) is resolved.

```bash
RUSTUP_TOOLCHAIN="nightly-2024-11-19" anchor build
```

### Deploy or Upgrade to mainnet-beta

First add the path to your wallet in your `anchor.toml`.

It should look like this:

```toml
[provider]
cluster = "Localnet"
wallet = "~/.config/solana/some_key_pair.json"
```

To deploy a new program use the `anchor deploy` command.

```bash
anchor deploy
```

To upgrade the program use the `anchor upgrade` command.

```bash
anchor upgrade ./target/deploy/ubclaunchpad.so --program-id 4dWhc3nkP4WeQkv7ws4dAxp6sNTBLCuzhTGTf1FynDcf
```

## Working on the project

First, clone the project

```bash
git clone https://github.com/EtienneCClarke/ubc-swarm-launchpad-programs.git
cd ubc-swarm-launchpad-programs
```

Then install dependencies using npm

```bash
npm install
```