# Loyalty Chain (LoyChain)

A next-generation Web3 loyalty program built on the **Sui blockchain**. LoyChain enables businesses to create transparent, secure, and flexible loyalty ecosystems using NFT membership cards, custom tokens, and decentralized marketplaces.

## Overview

LoyChain leverages blockchain technology, smart contracts, and token economics to revolutionize customer engagement and retention strategies. The platform supports both **custodial** and **self-custodial** modes, allowing businesses and members to choose their preferred level of control.

**Whitepaper:** [docs/whitepaper.md](./docs/whitepaper.md)

## Features

- **Multi-Partner Ecosystem** - Multiple businesses can onboard as partners with their own companies
- **Custom Token Economy** - Partners can create and manage their own loyalty tokens
- **NFT Membership Cards** - Tiered NFT cards with configurable benefits and rewards
- **Marketplace Integration** - Built-in marketplace for trading loyalty assets
- **Dual Custody Model** - Support for both admin-managed (custodial) and user-managed (self-custodial) operations
- **On-Chain Events** - Full transparency with blockchain event emissions

## Architecture

```
loychain/
├── sources/
│   ├── main.move              # Entry points and initialization
│   ├── libs/
│   │   └── util.move          # Utility functions (hashing, URLs, type helpers)
│   ├── modules/
│   │   ├── cap.move           # Admin capability management
│   │   ├── partner.move       # Partner & Company registration
│   │   ├── member.move        # Member management
│   │   ├── nft.move           # NFT Card Tiers, Types, and Cards
│   │   └── market_place.move  # Decentralized marketplace
│   ├── services/
│   │   ├── member_nft.move    # Member NFT operations
│   │   ├── member_token.move  # Member token operations
│   │   ├── partner_nft.move   # Partner NFT minting & transfers
│   │   ├── partner_order.move # Order completion with rewards
│   │   ├── partner_token.move # Partner token minting
│   │   └── partner_treasury.move # Treasury cap management
│   └── tokens/
│       ├── loy.move           # LOY native token
│       └── token_managable.move # Generic token creation & management
└── tests/                     # Comprehensive test suite
```

## On-Chain Objects

### Core Objects

| Object | Description |
|--------|-------------|
| `AdminCap` | Administrative capability for privileged operations |
| `PartnerBoard` | Shared registry of all partners |
| `CompanyBoard` | Shared registry of all companies |
| `MemberBoard` | Shared registry of all members |
| `Partner` | Business entity with token name, companies, and NFT settings |
| `Company` | Sub-entity under a Partner |
| `Member` | User account with nickname and owner address |

### NFT Objects

| Object | Description |
|--------|-------------|
| `NFTCardTier` | Defines tier levels (e.g., Bronze, Silver, Gold) with benefits |
| `NFTCardType` | Card template under a tier with supply limits |
| `NFTCard` | Issued membership card with accumulated value and usage tracking |

### Token Objects

| Object | Description |
|--------|-------------|
| `LOY` | Native platform token |
| `TreasuryCap<T>` | Minting capability for custom tokens |
| `MarketPlace<T>` | Token-specific marketplace for trading |

## Use Cases

### Membership Cards

1. **Point System Card** - Earn points on purchases, automatically minted to customer wallet
2. **Discount Card** - Receive percentage/fixed discounts on purchases  
3. **Loyalty Card** - Tiered rewards with automatic upgrades based on accumulated value

### Vouchers

1. **Service Vouchers** - Self-contained NFTs redeemable for products or services
2. **Gift Cards** - Transferable value cards between users

### Interoperability

- **Presale** - Token presale mechanisms
- **Swap** - Token exchange functionality
- **Escrow** - Secure multi-party transactions
- **Auction** - NFT and token auctions
- **Marketplace** - Open trading of loyalty assets

## Actions & Permissions

### Admin Actions (Requires `AdminCap`)

- Register partners and companies
- Register members
- Create NFT card tiers and types
- Mint and transfer NFT cards
- Transfer treasury capabilities
- Complete orders

### Partner Actions (Self-Custody)

- Register companies under their partnership
- Create and manage NFT card tiers/types
- Mint NFT cards to members
- Complete orders and reward members
- Manage their treasury

### Member Actions (Self-Custody)

- Self-register for membership
- Claim and transfer tokens
- Claim and transfer NFT cards

## Getting Started

### Prerequisites

Ensure you have the Sui CLI installed. The project is tested with:

```sh
sui --version
# sui 1.8.0-a583613cb
```

### Installation

If you need to install or upgrade Sui:

```sh
cargo install --locked --git https://github.com/MystenLabs/sui.git --branch devnet sui
```

### Running Tests

```sh
sui move test
```

### Building

```sh
sui move build
```

## Deployment

### Deploy to Testnet

```sh
sui client active-env
sui client active-address
sui client publish --gas-budget 300500500 > logs/publish-testnet.log
```

### Deployed Modules

The package deploys the following modules:

- `cap` - Admin capabilities
- `loy` - LOY token
- `main` - Entry points
- `market_place` - Marketplace functionality
- `member` - Member management
- `member_nft` - Member NFT services
- `member_token` - Member token services
- `nft` - NFT definitions
- `partner` - Partner management
- `partner_nft` - Partner NFT services
- `partner_order` - Order processing
- `partner_token` - Partner token services
- `partner_treasury` - Treasury management
- `token_managable` - Token utilities
- `util` - Helper functions

### Testnet Deployment

- **Package ID:** `0xa3372200d2719a4f4b15f471380403c23d8e9dfcc99670b3d8e70eb3e0d1b935`
- **Explorer:** [View on Sui Explorer](https://suiexplorer.com/object/0xa3372200d2719a4f4b15f471380403c23d8e9dfcc99670b3d8e70eb3e0d1b935?network=testnet)

#### Shared Objects (Testnet)

| Object | ID |
|--------|-----|
| PartnerBoard | `0xae879834b33cfc4079b0162d602f0fb026de8a3f05200575196d091e139180bf` |
| CompanyBoard | `0xfb1cb7788f5625e0992795dc501b8dbd1764501702502f32347522c7a8bf302b` |
| MemberBoard | `0xfa5c873e714f95be50b5b63c62c0ae249cb8aae596191d6321ccfc51237d2172` |

## Client Application

A frontend client for interacting with LoyChain is available at: [loy-client](https://github.com/channainfo/loy-client)

## Events

The following events are emitted on-chain for indexing and tracking:

- `PartnerCreateEvent` - Partner registration
- `CompanyCreatedEvent` - Company registration
- `MemberCreatedEvent` - Member registration
- `NFTCardTierCreatedEvent` - Card tier creation
- `NFTCardTypeCreatedEvent` - Card type creation

## Technical Details

### Token Specifications

| Property | Value |
|----------|-------|
| Decimals | 9 |
| Symbol | Derived from type name |

### Dependencies

```toml
[dependencies]
Sui = { git = "https://github.com/MystenLabs/sui.git", subdir = "crates/sui-framework/packages/sui-framework", rev = "testnet" }
```

## References

### Sui Documentation

- [Move Standard Library](https://github.com/MystenLabs/sui/tree/main/crates/sui-framework/packages/move-stdlib/sources)
- [Move Language Reference](https://github.com/move-language/move/tree/main/language/documentation/book/src)
- [Sui Framework](https://github.com/MystenLabs/sui/blob/main/crates/sui-framework/packages/sui-framework/sources)
- [Sui NFT Examples](https://github.com/MystenLabs/sui/tree/main/sui_programmability/examples/nfts)
- [Test Scenario](https://github.com/MystenLabs/sui/blob/main/crates/sui-framework/packages/sui-framework/sources/test/test_scenario.move)
- [Best Practices](https://docs.sui.io/testnet/build/dev_cheat_sheet)

### Additional Resources

- [Aptos vs Sui Comparison](https://medium.com/@fidika/aptos-vs-sui-detailed-dev-comparison-5d24df53eee8)

## License

This project is open source. See the repository for license details.
