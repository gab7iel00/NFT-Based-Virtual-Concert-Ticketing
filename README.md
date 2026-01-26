# 🎫 NFT-Based Virtual Concert Ticketing

[![Clarity](https://img.shields.io/badge/Clarity-v3-blue)](https://docs.stacks.co/clarity/)
[![Stacks](https://img.shields.io/badge/Stacks-Blockchain-orange)](https://stacks.co/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> 🎶 Revolutionary anti-scalping NFT concert ticketing system built on Stacks blockchain with automatic royalty distribution

## ✨ Key Features

- 🎟️ **NFT-Based Tickets**: Each ticket is a unique, verifiable non-fungible token
- 🚫 **Anti-Scalping Protection**: Automatic burn mechanism for overpriced tickets (>5x base price)
- 💰 **Artist Royalties**: Built-in royalty system (up to 25%) for all secondary sales
- 🔒 **Cryptographic Security**: Tamper-proof ticket validation and ownership
- 🎯 **Event Management**: Complete lifecycle management for concert events
- ⚡ **Resale Tracking**: Monitor and limit excessive ticket flipping (max 3 resales)
- 🎪 **Multi-Event Support**: Handle multiple concerts with unique configurations

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────┐
│           Smart Contract            │
├─────────────────────────────────────┤
│  🎪 Event Management                │
│  🎫 Ticket Minting & Transfer       │
│  💰 Royalty Distribution            │
│  🚫 Anti-Scalping Enforcement       │
│  🔐 Access Control                  │
└─────────────────────────────────────┘
```

## 🚀 Quick Start

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) v2.0+
- [Stacks CLI](https://docs.stacks.co/stacks-101/command-line-interface) 
- Node.js v16+ (for testing)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/NFT-Based-Virtual-Concert-Ticketing.git
cd NFT-Based-Virtual-Concert-Ticketing

# Verify contract compilation
clarinet check

# Run tests (if available)
clarinet test
```

## 📋 Contract Interface

### 🎪 Event Management

#### Create Event
```clarity
(create-event event-id venue event-date max-tickets base-price royalty-rate)
```

**Parameters:**
- `event-id` (uint): Unique identifier for the event
- `venue` (string-ascii 100): Concert venue name
- `event-date` (uint): Unix timestamp of event date
- `max-tickets` (uint): Maximum number of tickets available  
- `base-price` (uint): Base ticket price in microSTX
- `royalty-rate` (uint): Royalty percentage (0-25%)

**Example:**
```clarity
;; Create "Summer Music Festival 2024" 
(contract-call? .NFT-Concert-Tix create-event 
  u1 
  "Madison Square Garden" 
  u1735689600  ;; Jan 1, 2025 timestamp
  u10000       ;; 10,000 tickets max
  u50000000    ;; 50 STX base price  
  u10)         ;; 10% artist royalty
```

### 🎫 Ticket Operations

#### Mint Ticket
```clarity
(mint-ticket event-id recipient token-uri)
```

**Example:**
```clarity
;; Mint ticket for a fan
(contract-call? .NFT-Concert-Tix mint-ticket 
  u1 
  'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM 
  (some "https://api.concert.com/metadata/1"))
```

#### Transfer Ticket
```clarity
(transfer token-id sender recipient)
```

**Example:**
```clarity
;; Transfer ticket to another user
(contract-call? .NFT-Concert-Tix transfer 
  u1001 
  'ST1SENDER... 
  'ST1RECIPIENT...)
```

#### Purchase from Secondary Market
```clarity
(buy-ticket token-id max-price)
```

**Example:**
```clarity
;; Buy ticket with price protection
(contract-call? .NFT-Concert-Tix buy-ticket 
  u1001        ;; token-id
  u75000000)   ;; max willing to pay (75 STX)
```

#### Set Resale Price
```clarity
(set-token-price token-id new-price)
```

**Example:**
```clarity
;; Set ticket price for resale
(contract-call? .NFT-Concert-Tix set-token-price 
  u1001        ;; your token-id
  u60000000)   ;; 60 STX asking price
```

### 🔥 Anti-Scalping Mechanism

#### Automatic Burn Triggers
- **Price Violation**: Tickets priced >500% of base price are auto-burned
- **Resale Abuse**: Tickets resold >3 times are flagged for burning
- **Manual Override**: Contract owner can burn problematic tickets

```clarity
(burn-scalped-ticket token-id)
```

### 📊 Query Functions

#### Get Event Information
```clarity
(get-event-info event-id)
;; Returns: {artist, venue, event-date, max-tickets, base-price, royalty-rate}
```

#### Get Ticket Price
```clarity
(get-token-price token-id)
;; Returns: current asking price in microSTX
```

#### Check Resale Count
```clarity
(get-resale-count token-id)
;; Returns: number of times ticket has been resold
```

#### Verify Token Ownership
```clarity
(get-owner token-id)
;; Returns: current owner principal
```

## 🔐 Access Control & Security

### Permission Levels

| Role | Permissions |
|------|-------------|
| **Contract Owner** | Create events, authorize minters, burn tickets |
| **Event Artist** | Mint tickets for their events, receive royalties |
| **Authorized Minter** | Mint tickets (requires owner approval) |
| **Ticket Holder** | Transfer, resell, set prices for owned tickets |

### Security Features

- ✅ **Ownership Verification**: Only token owners can transfer/sell
- ✅ **Price Validation**: Maximum scalping price enforcement (5x base)
- ✅ **Burn Protection**: Prevents transfer of burned tokens
- ✅ **Royalty Automation**: Automatic STX distribution to artists
- ✅ **Event Validation**: Future-dated events only

## 🧪 Testing & Development

### Local Testing
```bash
# Start Clarinet console for interactive testing
clarinet console

# Example test sequence
(contract-call? .NFT-Concert-Tix create-event u1 "Test Venue" u9999999999 u100 u1000000 u5)
(contract-call? .NFT-Concert-Tix mint-ticket u1 tx-sender none)
(contract-call? .NFT-Concert-Tix get-ticket-count u1)
```

### Contract Verification
```bash
# Check contract syntax and compilation
clarinet check

# Analyze contract for potential issues
clarinet analyze
```

## 🚀 Deployment

### Testnet Deployment
```bash
# Deploy to Stacks testnet
clarinet deploy --testnet

# Verify deployment
stx call_read_only_fn ST1234...CONTRACT get-last-token-id --testnet
```

### Mainnet Deployment
```bash
# Deploy to mainnet (production)
clarinet deploy --mainnet
```

## 📈 Contract Statistics

| Metric | Value |
|--------|-------|
| **Lines of Code** | 155 |
| **Public Functions** | 11 |
| **Read-only Functions** | 8 |
| **Private Functions** | 1 |
| **Data Maps** | 9 |
| **Constants** | 8 |
| **Max Token URI Length** | 256 characters |
| **Max Venue Name Length** | 100 characters |

## 🎯 Use Cases

### 🎵 Concert Promoters
- Create events with customizable anti-scalping rules
- Set artist royalty rates (0-25%)
- Control ticket supply and pricing

### 🎤 Artists & Venues  
- Receive automatic royalties on all secondary sales
- Mint tickets for VIP experiences or special access
- Prevent ticket scalping and fraud

### 🎪 Fans & Collectors
- Own provably authentic concert tickets
- Safely resell tickets at fair market prices
- Build collections of memorable concert experiences

## 🛠️ Advanced Configuration

### Authorize Additional Minters
```clarity
;; Allow venue or ticketing partner to mint
(contract-call? .NFT-Concert-Tix authorize-minter 'ST1VENUE...)

;; Revoke minting permissions
(contract-call? .NFT-Concert-Tix revoke-minter 'ST1VENUE...)
```

### Custom Token URIs
```clarity
;; Mint with metadata URL
(contract-call? .NFT-Concert-Tix mint-ticket 
  u1 
  'ST1FAN... 
  (some "ipfs://QmHash.../metadata.json"))
```

## 🤝 Contributing

1. 🍴 Fork the repository
2. 🔄 Create feature branch (`git checkout -b feature/amazing-feature`)
3. ✅ Add tests for new functionality
4. 📝 Commit changes (`git commit -m 'Add amazing feature'`)
5. 🚀 Push to branch (`git push origin feature/amazing-feature`)
6. 🔃 Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🌟 Impact & Vision

**Revolutionizing the Concert Industry:**

- 🎯 **Eliminate Fraud**: Cryptographically secure ticket authenticity
- 💰 **Fair Pricing**: Anti-scalping mechanisms protect fans
- 🎵 **Artist Revenue**: Guaranteed royalties on secondary sales  
- 🌐 **Global Access**: Borderless ticket ownership and transfer
- 📊 **Transparency**: Public, auditable ticket transactions
- 🔮 **Future-Proof**: Built on decentralized blockchain infrastructure

## 🎉 Community

Join the revolution in decentralized ticketing:

- 💬 [Discord Community](https://discord.gg/stacks)
- 🐦 [Twitter Updates](https://twitter.com/stacks)
- 📚 [Stacks Documentation](https://docs.stacks.co)

---

**🎵 Built with ❤️ for artists, fans, and the future of live music**

*Empowering fair, transparent, and secure concert ticketing for everyone.*

