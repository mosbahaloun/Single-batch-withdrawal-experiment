# Blockchain Voting System with Zero-Knowledge Proofs

A privacy-preserving decentralized voting system using zk-SNARKs (zero-knowledge proofs) for anonymous voting. Built with Hardhat, Circom, and Solidity.

## 🎯 Overview

This system implements a privacy-preserving voting mechanism inspired by Tornado Cash's architecture, featuring:
- **Anonymous voting** through zero-knowledge proofs
- **Batch withdrawal optimization** for efficient vote tallying (8 votes per batch)
- **MiMC hash function** for Merkle tree commitments
- **Groth16 proof system** via snarkjs
- Comprehensive gas cost analysis and performance metrics

## 🚧 Project Status

**Frontend:** Under development  
**Backend & Circuits:** Fully functional  
**Testing:** Complete test suite available in `Backend/test/sample-test.js`

## ✨ Key Features

### Zero-Knowledge Privacy
- Voters create commitments without revealing identity
- Nullifier hash prevents double voting
- Merkle tree structure ensures vote integrity

### Batch Processing
- Process 8 votes simultaneously in a single withdrawal
- Significantly reduces gas costs compared to individual processing
- Custom Circom circuits for batch verification

### Performance Monitoring
- Real-time gas cost tracking
- Timing statistics (mean, p50, p90, p99)
- GBP cost estimation based on current ETH prices

## 📋 Prerequisites

- Node.js v14 or higher
- npm or yarn
- Hardhat

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
cd YOUR-REPO-NAME
```

### 2. Install Dependencies
```bash
# Root dependencies
npm install

# Backend dependencies
cd Backend
npm install
cd ..
```

### 3. Install Circomlib
```bash
cd circuit
npm install circomlib
cd ..
```

## 🧪 Testing

The complete voting system functionality is tested in `Backend/test/sample-test.js`.

### What the Test Does

**Test 1: Full Voting Flow (40 deposits + 5 batch withdrawals)**
- Deploys contracts (Hasher, Verifier, Tornado)
- Adds 10 candidates
- Performs 40 deposits (vote commitments)
- Executes 5 batch withdrawals (8 votes each)
- Measures:
  - Gas usage for each operation
  - Timing statistics (witness generation, proof generation, transaction inclusion)
  - Total cost in ETH and GBP

**Test 2: Voter Registration (40 addVoter calls)**
- Measures gas costs for adding voters
- Tracks per-transaction and aggregate metrics

### Run the Tests

**Option 1: Single Terminal**
```bash
cd Backend
npx hardhat test test/sample-test.js
```

**Option 2: With Local Node (Recommended)**

Terminal 1 - Start local blockchain:
```bash
npx hardhat node
```

Terminal 2 - Run tests:
```bash
cd Backend
npx hardhat test test/sample-test.js --network localhost
```

### Expected Output

The test will display:
```
✅ Deposit 1 Gas Used: [gas amount]
...
✅ Withdraw 1 Gas Used: [gas amount]
...
📊 Total Gas Used (40 deposits + 5 withdrawals of 8): [total]
💷 Estimated GBP Cost: £[amount]
⏱ Deposit witness (deposit circuit): n=40 mean=...ms p50=...ms p90=...ms p99=...ms
⏱ Withdraw proof gen (batch of 8): n=5 mean=...ms p50=...ms p90=...ms p99=...ms
```

## 📁 Project Structure
```
├── Backend/
│   ├── contracts/          # Solidity smart contracts
│   │   ├── Tornado.sol    # Main voting contract
│   │   ├── Verifier.sol   # Groth16 verifier
│   │   └── Hasher.sol     # MiMC hasher
│   ├── test/
│   │   └── sample-test.js # Complete test suite ⭐
│   ├── scripts/           # Deployment scripts
│   ├── circuit/           # Circuit utilities
│   └── utils/             # Helper functions
│       ├── deposit.wasm   # Compiled deposit circuit
│       ├── BatchWithdraw_8.wasm  # Compiled batch withdraw circuit
│       └── setup_final.zkey      # Proving key
├── circuit/               # Circom circuit definitions
│   ├── BatchWithdraw_8.circom   # 8-vote batch circuit
│   ├── deposit.circom           # Deposit circuit
│   ├── withdraw.circom          # Single withdraw circuit
│   └── utils/                   # Circuit utilities (MiMC, Merkle)
├── frontend/              # Next.js interface (in development)
└── test/                  # Additional test files
```

## 🔧 Technical Details

### Circuit Parameters
- **Merkle Tree Depth:** 10 levels (supports up to 1,024 deposits)
- **Batch Size:** 8 votes per withdrawal
- **Public Signals:** 24 per batch (8 roots + 8 nullifiers + 8 recipients)

### Gas Costs (Approximate)
- **Add Candidate:** ~50-70k gas
- **Deposit:** ~200-250k gas per vote
- **Batch Withdrawal (8 votes):** ~800k-1M gas
- **Add Voter:** ~50-80k gas

### Key Algorithms
- **Hash Function:** MiMC-5 Sponge
- **Proof System:** Groth16 (via snarkjs)
- **Merkle Tree:** Custom implementation with default leaf values

## 🛠️ Technologies

| Component | Technology |
|-----------|-----------|
| Smart Contracts | Solidity ^0.8.0 |
| Development Framework | Hardhat |
| Zero-Knowledge Circuits | Circom 2.x |
| Proof Generation | snarkjs |
| Testing | Chai + Mocha |
| Hash Function | MiMC-5 |
| Frontend (WIP) | Next.js, ethers.js |

## 📊 Performance Metrics

The test suite provides detailed metrics:

1. **Witness Calculation Time** - Circuit input processing
2. **Merkle Path Computation** - MiMC hash calculations
3. **Proof Generation Time** - zk-SNARK proof creation (per batch)
4. **Transaction Inclusion Time** - On-chain confirmation latency
5. **Gas Costs** - Per operation and aggregate totals

## 🔐 Security Considerations

⚠️ **Educational Project:** This is a demonstration of zk-SNARK voting mechanisms. Production deployment would require:
- Professional security audit
- Formal verification of circuits
- Enhanced key management
- Rate limiting and DoS protection
- Circuit parameter optimization

## 🗺️ Roadmap

- [x] Core smart contracts
- [x] Zero-knowledge circuits (deposit, batch withdraw)
- [x] Comprehensive test suite with metrics
- [x] Gas optimization (batch processing)
- [x] Candidate management system
- [ ] Complete frontend interface
- [ ] Voter authentication UI
- [ ] Real-time vote tallying dashboard
- [ ] Production security hardening

## 📝 License

MIT

## 🤝 Contributing

Contributions welcome! Please open an issue or submit a PR.

## 📚 Learn More

- [Circom Documentation](https://docs.circom.io/)
- [snarkjs](https://github.com/iden3/snarkjs)
- [Tornado Cash Whitepaper](https://tornado.cash/)
- [Groth16 Protocol](https://eprint.iacr.org/2016/260.pdf)

---

**Note:** This project demonstrates privacy-preserving voting using zero-knowledge proofs. All functionality can be tested via the test suite while the frontend is being developed.
