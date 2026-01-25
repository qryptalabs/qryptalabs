# QRYPTA Contracts (QRYP)

Hybrid ERC-20 smart contracts for **BNB Chain** and **Ethereum**, combining standard market compatibility (DEX/CEX friendly) with an optional **post-quantum transfer verification lane** powered by **SP1 ZK proofs**, plus a resilience path via **M-of-N attester approvals**. Includes **ISO 20022-style payment references** and **compliance-ready controls**.

---

## Mainnet Deployments

- **BSC Mainnet (verified):** `0x5266fe1aD9B035d0ED6142f1A70e9D6F102c8153`

> Ethereum deployment: planned.

---

## Key Features

### Market Compatibility
- Standard **ERC-20 transfers** for DEX/CEX workflows
- Allowances (`approve/transferFrom`) supported

### Post-Quantum Transfer Verification (ZK / SP1)
- User registers a **PQC commitment/root** (`pqcRoot`)
- Transfers can be executed via **`quantumTransferZK`**
- On-chain verification uses:
  - `programVKey` (SP1 program verification key)
  - `sp1Gateway` verifier
  - `publicValues` binding: `from/to/amount/nonce/chainId/token/deadline/isoRefHash`

### Resilience Path (Attesters)
- Optional **M-of-N** attester approvals (`attestedQuantumTransfer`)
- Signatures are validated with strict ordering to prevent duplicates

### ISO 20022-Style Payment References
- Transfers can emit auditable ISO references (string) and `isoRefHash`
- Useful for reconciliation and settlement workflows

### Compliance & Safety Controls
- `pause/unpause` (hard stop)
- `freeze/unfreeze` per wallet
- KYC allowlist + denylist toggles
- `pqcOnly` enforcement per wallet
- Threshold-based enforcement for large transfers
- Configurable burn rate (bps)

---

## Architecture Overview

QRYPTA is designed as a **dual-mode token**:

1) **Normal lane** for standard market operations (DEX/CEX)
2) **High-security lane** where a transfer is proven off-chain (PQC verification inside the SP1 program) and verified on-chain using SP1 proof verification.

**Why this approach?**  
Native PQC signature verification (Dilithium/Falcon) is not practical inside the EVM. Instead, the contract verifies a ZK proof that attests PQC verification was performed correctly and that the transfer parameters match the on-chain requirements.

---

## SP1 / ZK Proof Interface (PublicValues)

The contract expects `publicValues` to bind all critical fields to prevent replay and cross-domain misuse:

- `from`
- `to`
- `amount`
- `nonce`
- `pqcRoot`
- `isoRefHash`
- `chainId`
- `token` (contract address)
- `deadline`

Any mismatch should cause the transfer to revert.

---

## Security Notes

This repo contains contracts with **admin and compliance controls**. The owner can adjust critical parameters (e.g., gateway/vKey, quorum, pause/freeze, burn bps). For production governance, consider:
- **Multisig ownership**
- **Timelock** for sensitive changes
- Clear public policy for upgrades/config changes

See: `SECURITY.md`

---

## Repository Structure (suggested)


