# LivePoll

LivePoll is a mini end-to-end Stellar + Soroban dApp: a multi-wallet polling app backed by a deployed Soroban smart contract on Stellar Testnet. It provides contract reads and writes, real-time event synchronization, transaction-progress feedback, basic caching, and automated tests.

## Level 3 Submission Checklist

- Live demo: https://online-live-poll-level-3-six.vercel.app/
- Demo video: https://drive.google.com/file/d/1klEitAoHwLG_QmheL4puycvb1m2CwXIp/view?usp=sharing
- Public repository: https://github.com/amanraj592430-del/online_live_poll_level_3
- Meaningful Level 3 commits: included in Git history

## Submission Evidence — Include These Files

> **Important:** The complete folders below must be included in the submitted repository/archive. This README documents the implementation, but it does not replace the source files required to verify it.

| Requirement | Source evidence to include | What it verifies |
| --- | --- | --- |
| Wallet connection | `src/lib/stellar.js` | `StellarWalletsKit` setup and wallet connection logic |
| Soroban contract structure and logic | `poll_contract/Cargo.toml`, `poll_contract/src/lib.rs` | Contract crate, poll storage, and contract entry points |
| Frontend-to-contract integration | `src/lib/stellar.js` and the React components under `src/` | RPC calls, contract invocation, signing, and UI state updates |
| Contract artifact | `public/contracts/poll_contract.wasm` | Compiled WASM/spec used by the frontend at runtime |
| Contract build/deploy flow | `scripts/` and `package.json` | Build, WASM sync, and Testnet deployment commands |
| Automated verification | `tests/` | Tests for frontend helper logic |

For review, do **not** submit only built assets, screenshots, or this README. Include the `poll_contract/`, `src/`, `scripts/`, and `tests/` directories as source-controlled files.

## What Can Be Verified from the Source

1. **Wallet support:** `src/lib/stellar.js` configures `StellarWalletsKit` for compatible wallets, including Freighter, xBull, Albedo, Rabet, Lobstr, Hana, Hot Wallet, and Klever.
2. **Contract implementation:** `poll_contract/src/lib.rs` contains the Soroban contract code for creating, voting on, closing, reading, and deleting polls.
3. **Contract calls in the frontend:** `src/lib/stellar.js` constructs and submits Soroban RPC transactions for the same public contract operations exposed in the UI.
4. **UI/contract matching:** React components call the integration helpers for create, vote, close, delete, and read actions; the event sync refreshes the displayed poll state.
5. **Runtime contract specification:** `npm run wasm:sync` copies the compiled contract WASM to `public/contracts/poll_contract.wasm`, which the frontend uses to load the contract specification.

## Submission Verification Steps

From the project root, a reviewer can verify the required evidence with:

```bash
# Confirm that the required source files are present
test -f poll_contract/src/lib.rs
test -f src/lib/stellar.js

# Inspect the contract entry points and frontend integration
rg "create_poll|vote|close_poll|delete_poll" poll_contract/src/lib.rs src/lib/stellar.js
rg "StellarWalletsKit" src/lib/stellar.js

# Build the contract and frontend, then run the test suite
npm install
npm run contract:build
npm run wasm:sync
npm test
npm run build
```

On Windows PowerShell, use the following source-presence check instead of `test -f`:

```powershell
Test-Path poll_contract/src/lib.rs
Test-Path src/lib/stellar.js
```

## Key Features

- Multi-wallet integration with `StellarWalletsKit`
- Soroban smart-contract reads and writes on Stellar Testnet
- Create, vote on, close, delete, and browse polls
- Read-only poll browsing without a connected wallet
- Transaction phases: `preparing`, `awaiting-signature`, `pending`, `success`, and `error`
- Wallet error handling for missing wallet, rejected requests, and insufficient balance
- Event polling and state synchronization
- `localStorage` caching for recently loaded poll data
- Automated tests for core helper logic
## Screenshots

🏠 Home Page
   ![Screenshot 2026-07-27 114138.png](screenshots/Screenshot%202026-07-27%20114138.png)   
📝 Create Poll
      ![Screenshot 2026-07-27 114154.png](screenshots/Screenshot%202026-07-27%20114154.png)
🗳️ Voting
    ![Screenshot 2026-07-27 114210.png](screenshots/Screenshot%202026-07-27%20114210.png)
✅ CI/CD
    ![Screenshot 2026-07-27 115150.png](screenshots/Screenshot%202026-07-27%20115150.png)

## Mobile responsive screenshots

Below is a mobile view screenshot demonstrating the responsive layout on narrow screens. Replace the placeholder with a real phone-sized screenshot captured from the dev tools or a device.

 ![WhatsApp Image 2026-07-27 at 11.31.10 AM.jpeg](screenshots/WhatsApp%20Image%202026-07-27%20at%2011.31.10%20AM.jpeg)
## Deployed Contract

- Network: `Stellar Testnet`
- Contract address: `CDPYFRUN6ZRKUIKZR45AMWF7SYPQJL4WRJIBJI2SR3DWRMMANTXXRMD2`
- Contract explorer: https://stellar.expert/explorer/testnet/contract/CDPYFRUN6ZRKUIKZR45AMWF7SYPQJL4WRJIBJI2SR3DWRMMANTXXRMD2

## Verifiable Contract Transactions

- Deploy transaction: `0e1e13467216b3056b5351fd7d10ea59e2bc3d3000056fe236e42d5e2cb4bcdd`
- Deploy transaction explorer: https://stellar.expert/explorer/testnet/tx/0e1e13467216b3056b5351fd7d10ea59e2bc3d3000056fe236e42d5e2cb4bcdd
- Sample `create_poll` transaction: `e5a4df2c3ef97235d1b33ebe043cb66ab5642d53f0319caabc9f98e2239712c8`
- Sample transaction explorer: https://stellar.expert/explorer/testnet/tx/e5a4df2c3ef97235d1b33ebe043cb66ab5642d53f0319caabc9f98e2239712c8

## Local Setup

Run all commands from the project root.

```bash
npm install
npm run contract:build
npm run wasm:sync
npm run dev
```

To create a local environment file:

```powershell
Copy-Item .env.example .env.local
```

Build for production with:

```bash
npm run build
```

## Environment Variables

```env
VITE_STELLAR_RPC_URL=https://soroban-testnet.stellar.org
VITE_STELLAR_NETWORK_PASSPHRASE=Test SDF Network ; September 2015
VITE_STELLAR_CONTRACT_ID=CDPYFRUN6ZRKUIKZR45AMWF7SYPQJL4WRJIBJI2SR3DWRMMANTXXRMD2
VITE_STELLAR_READ_ACCOUNT=
VITE_STELLAR_EXPLORER_URL=https://stellar.expert/explorer/testnet
VITE_POLL_CONTRACT_WASM_URL=/contracts/poll_contract.wasm
```

## Tests

```bash
npm test
```

Include terminal output showing at least three passing tests with the submission.

## Project Structure

```text
src/                         React frontend
src/lib/stellar.js           Wallet, RPC, contract, and event helpers
src/lib/pollCache.js         Poll cache helpers
src/lib/pollLogic.js         Pure UI helper logic
poll_contract/               Soroban contract crate
poll_contract/src/lib.rs     Contract source code
public/contracts/            Compiled contract WASM consumed by the frontend
scripts/                     Build and deployment helpers
tests/                       Automated test suite
```

## Deployment

This is a standard Vite build.

- Node.js: `^20.19.0` or `>=22.12.0`
- Build command: `npm run build`
- Output directory: `dist`
- Configure the environment variables above in the hosting provider.

## Demo Walkthrough

1. Open the live app and show the read-from-contract panel updating.
2. Connect a supported wallet.
3. Create a poll and show `awaiting-signature` → `pending` → `success`.
4. Vote on the poll and show the event feed or vote count updating.
5. Open the contract and transaction links in Stellar Expert.

## Additional Documentation

- [Frontend guide](./FRONTEND.md)
- [Contract guide](./poll_contract/README.md)
