# Introduction

The project aims to build a Decentralized Application (DApp) where institutions can
issue digital certificates (degrees, transcripts) that are stored on the InterPlanetary
File System (IPFS) and anchored to a blockchain via a Smart Contract. Employers can
instantly verify the authenticity of these credentials by scanning a QR code,
eliminating the need for manual verification or physical documents.

Traditional paper-based and centralized digital certificates are prone to forgery, loss,
and bureaucratic delays in verification. Manual verification processes are timeconsuming and often require contacting the issuing institution directly.
By leveraging blockchain's immutability and transparency, we can create a "single source of truth." Once a certificate hash is recorded on the blockchain, it cannot be
altered or deleted. This provides a decentralized, trustless verification mechanism.

# Funcation requirements

The system involves three main modules: Institute (Admin), Student, and Employer
(Verifier).

1. _Step1. Certificate Generation (Off-Chain)._
   a. The University admin logs into the DApp dashboard.
   b. They upload the student's certificate file (PDF).
   c. The system uploads this file to IPFS (via services like Pinata), which
   returns a unique Content Identifier (CID).
   d. Alternatively, the system generates a SHA-256 hash of the file data for an
   extra layer of verification.
2. _Step 2: On-Chain Anchoring (Smart Contract)_
   a. The Smart Contract (deployed on Anvil) stores the mapping: Certificate
   ID -> IPFS CID -> Student Wallet Address.
   b. The admin triggers a transaction to "Issue" the certificate. The IPFS CID
   and the SHA-256 hash of the document are written to the blockchain.
   c. The contract also includes a revoke() function to invalidate a certificate if
   necessary (e.g., in cases of disciplinary action).
3. _Step 3: QR Code Generation_
   a. The system generates a QR Code that contains the unique Certificate ID
   and the verification URL of your DApp.
   b. This QR code is embedded into the PDF certificate or sent to the student.
4. _Step 4: Verification (Public)_
   a. An employer scans the QR code.
   b. The DApp queries the blockchain for the Certificate ID.
   c. The DApp retrieves the IPFS CID from the blockchain and fetches the
   original file from IPFS.
   d. The system compares the retrieved file's hash with the on-chain hash. If
   they match, the certificate is authentic; if not, it is fraudulent.

# _Implementation_

## Phase 1

### Tools:

Node.js, Hardhat (Development Environment), Solidity (Language), MetaMask (Wallet), OpenZeppelin (Security Libraries), Anvil (Local Testnet).

### Action:

Write the Certificate.sol smart contract.

### Local Development Environment:

Anvil,Anvil is a ready-made local Ethereum testnet node bundled with the Foundry toolkit. It allows us to develop and test the entire smart contract logic without any faucet waits, network delays, or external dependencies.

### Connecting to MetaMask:

Add a custom network with:

- RPC URL: http://127.0.0.1:8545
- Chain ID: 31337
- Currency Symbol: ETH

### Deploying and Testing:

Deploy the Certificate.sol contract to Anvil using Hardhat, then run all
issue/verify/revoke tests. Every transaction is instant and free.

## Phase 2

#### Tools:

Python(Django/Flask) or Node.js (express), Web3.py or Ether.js.

#### Action:

- Create an endpoint for "Issue Certificate." This endpoint takes the file upload,
  sends it to Pinata API (IPFS), and returns the CID.
- Use the CID to call the Smart Contract issueCertificate function using a
  university private key.
- Generate a SHA-256 hash of the file content and store it alongside the CID.

## Phase 3

#### Tools:

React.js, Tailwind CSS, Ether.js

#### Action:

- _Admin Dashboard_: Allow admins to upload files, view issuance history, and
  revoke certificates.
- _Student Portal_: Allow students to view their certificates and download the
  generated QR code.
- _Public Verification Page_: A simple interface where anyone can scan or upload
  a QR code image to verify the credential.

## Phase 4

#### Testing and Optimization

- Local Testing with Anvil: Run comprehensive unit tests for all contract
  functions. Test edge cases like duplicate issuance, unauthorized access, and
  revocation. Anvil's instant mining allows rapid iteration without any waiting.
- Gas Optimization: Although Anvil provides free transactions, optimize Solidity
  code (e.g., use calldata instead of memory, pack variables) to demonstrate
  best practices.
- Security: Ensure only authorized wallets (university addresses) can call the
  issueCertificate function (use OpenZeppelin Ownable or AccessControl).
- Usability: Test the QR code scanning from different distances and lighting
  conditions.

#### Expected outcomes & Evaluation

- Functional: A working DApp where a user can upload a certificate, get a QR
  code, and verify it independently without relying on a central server.
- Performance: Verification time should be under 2 seconds (dependent on
  IPFS gateway speed and local RPC response time).
- Cost: Demonstrate that on Anvil, all transactions are completely free and
  instant, making it ideal for development and academic demonstrations.

## Tools:

### Local Blockchain Testnet:

Anvil (bundled with Foundry), a ready-made local Ethereum testnet for rapid smart
contract development and testing. Provides 10 pre-funded accounts, instant
transaction mining, and requires no internet connection after installation.

#### Storage:

IPFS (Pinata) for decentralized, off-chain file storage.

#### Smart Contracts:

Solidity for logic (Issue/Revoke/Verify).

#### Backend:

Python Django (Recommended for simplicity in file handling) or Node.js.

#### Frontend:

React.js with Ethers.js for blockchain interaction.
