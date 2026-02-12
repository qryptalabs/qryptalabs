🛡️ QRYPTA: Post-Quantum ZK-Token Infrastructure
The first hybrid ERC-20 token on BNB Chain designed to survive the Quantum Future.

QRYPTA combines standard market compatibility (DEX/CEX ready) with a Post-Quantum Cryptography (PQC) verification lane, powered by SP1 Zero-Knowledge Proofs and ISO 20022 compliance standards.

🚨 The Problem
Quantum computers will break current blockchain signatures (ECDSA) within the next decade. Assets stored on standard blockchains today are at risk of future theft.

⚡ The Solution: QRYPTA
We verify Dilithium/Falcon (NIST-standard PQC signatures) off-chain using the SP1 zkVM and submit a ZK-Proof to the blockchain. This allows BNB Chain to verify quantum-safe transactions today without waiting for a hard fork.

🌍 Mainnet Deployments
Network	Contract Address	Status	Explorer
BNB and Ethereum Chain	0x5266fe1aD9B035d0ED6142f1A70e9D6F102c8153	
🟢 Live	View on BscScan https://bscscan.com/token/0x5266fe1ad9b035d0ed6142f1a70e9d6f102c8153
🟢 Live	View Ethereum	https://etherscan.io/token/0x5266fe1aD9B035d0ED6142f1A70e9D6F102c8153	

🏗️ Architecture & Features
QRYPTA operates as a Dual-Mode Token:

1. Market Lane (Standard)
Fully compatible with Uniswap, PancakeSwap, and CEXs.
Standard approve, transfer, transferFrom.
Burn Rate: Configurable basis points (bps) for deflationary mechanics.

2. Quantum-Secure Lane (High Security) 🛡️
ZK-Powered Verification: Transfers are signed with PQC keys (Dilithium), proven in Rust via SP1, and verified on-chain via quantumTransferZK.
SP1 Integration: Utilizing Succinct's zkVM to verify off-chain computation.
ISO 20022 Ready: Embedded isoRefHash for institutional banking reconciliation and compliance.
3. Resilience & Compliance
M-of-N Attestations: Optional multisig-style approvals for high-value transfers.
Compliance Hooks: freeze, unfreeze, and KYC Allowlist capabilities for regulated environments.

🧩 Technical Deep Dive: The ZK Proof
The quantumTransferZK function enforces strict binding of Public Values to prevent replay attacks:
Solidity
struct PublicValues {
    address from;
    address to;
    uint256 amount;
    uint256 nonce;
    bytes32 pqcRoot;    // Post-Quantum Identity Root
    bytes32 isoRefHash; // ISO 20022 Reference
    uint256 chainId;
    address token;      // Contract Address
    uint256 deadline;
}
If the ZK Proof does not mathematically match these values, the EVM reverts the transaction.

🚀 Getting Started (Developers)
Prerequisites

Rust
SP1 SDK
Foundry
Installation
Bash
# Clone the repository
git clone https://github.com/qryptalabs/core.git
# Install dependencies
npm install
cargo build --release
# Generate a Genesis Proof (Local)
cd local-prover
npm start

🗺️ Roadmap
[x] Phase 1: Smart Contract Deployment on BSC Mainnet.
[x] Phase 2: Local SP1 Prover Integration.
[ ] Phase 3: Succinct Prover Network Integration (In Progress).
[ ] Phase 4: Quantum-Secure Bridge to Ethereum.
[ ] Phase 5: Institutional Pilot with ISO 20022 Banking Partners.

🔒 Security
Contracts include administrative controls (Gateway updates, Pausable, Freezable).
Audits: Pending.
Governance: Currently managed by the deployer key. Transition to DAO Multisig planned for Q3 2026.

See SECURITY.md for bug bounty information.



