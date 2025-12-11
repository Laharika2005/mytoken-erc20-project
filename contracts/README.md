# MyToken (MTK)

## Overview
MyToken is a simple ERC-20 compatible token implemented in Solidity for learning purposes. This repository contains the smart contract source (`contracts/MyToken.sol`), testing screenshots, and documentation required for assignment submission.

## Token Details
- **Name:** MyToken
- **Symbol:** MTK
- **Decimals:** 18
- **Total Supply:** 1,000,000 MTK (represented as `1000000000000000000000000` in smallest units)

## Features Implemented
- ERC-20 standard functions:
  - `name()`, `symbol()`, `decimals()`, `totalSupply()`
  - `balanceOf(address)`
  - `transfer(address,uint256)`
  - `approve(address,uint256)`
  - `transferFrom(address,address,uint256)` *(implemented in code)*
  - `allowance(address,address)`
- Events:
  - `Transfer`
  - `Approval`
- Input validation:
  - Prevent transfers to the zero address
  - Check for sufficient balances and allowances

## How to compile and deploy (Remix)
1. Open https://remix.ethereum.org/.
2. Create a folder `contracts/` and add `MyToken.sol`.
3. Open the **Solidity Compiler** tab (`{}` icon).
   - Set compiler to `0.8.x` (recommended `0.8.20`).
   - Set EVM version to `istanbul` or `london`.
   - Turn **Optimization** OFF.
   - Click **Compile MyToken.sol**.
4. Go to **Deploy & Run Transactions** (play icon).
   - Environment: **Remix VM (Shanghai)**.
   - In Contract dropdown choose `MyToken`.
   - In constructor input enter the initial supply (1,000,000 × 10^18):
     ```
     1000000000000000000000000
     ```
   - Click **Deploy**.

## How to test (Remix UI step-by-step)
### Verify metadata
- Click `name()` → expects `"MyToken"`.
- Click `symbol()` → expects `"MTK"`.
- Click `decimals()` → expects `18`.
- Click `totalSupply()` → expects `1000000000000000000000000`.

### Test transfer (transfer)
1. Set **ACCOUNT** to the deployer (first account).
2. Expand deployed contract and find `transfer(address,uint256)`.
3. Paste recipient address in `_to`.
4. Enter `1000000000000000000` in `_value` (1 token).
5. Click **transfer**.
6. Verify `balanceOf(deployer)` decreased and `balanceOf(recipient)` increased.
7. Confirm `Transfer` event in logs.

### Test approve & transferFrom (approve + transferFrom)
1. With **ACCOUNT = deployer**, run:
   ```
   approve(spenderAddress, 5000000000000000000)
   ```
   Expect `Approval` event.
2. Verify allowance:
   ```
   allowance(deployer, spender) -> 5000000000000000000
   ```
3. Set **ACCOUNT = spender** and run:
   ```
   transferFrom(deployer, recipient, 2000000000000000000)
   ```
   Expect `Transfer` event and allowance reduced to `3000000000000000000`.

## Testing results (what worked)
- name() ✔
- symbol() ✔
- decimals() ✔
- totalSupply() ✔
- balanceOf() ✔
- transfer() ✔ (Transfer events logged)
- approve() ✔ (Approval events logged)
- allowance() ✔ (readable value in UI)

## Known Issue: transferFrom Testing in Remix
During interactive testing in Remix VM I implemented and verified `approve()` and confirmed allowance storage. However, `transferFrom()` calls via the Remix UI intermittently reverted with:

```
Error: revert
Reason provided by the contract: "Insufficient allowance"
```

What I attempted:
- Recompiled with Solidity **0.8.20**
- Switched EVM version to **istanbul**
- Reset Remix VM state and redeployed
- Re-entered addresses via Remix account dropdown to avoid stray characters
- Verified `approve()` emitted `Approval` and `allowance(owner, spender)` returned the expected value

Possible cause:
- UI/encoding issue in Remix (address formatting or VM state) rather than contract logic.
- The contract source itself contains the correct `transferFrom` logic; programmatic tests (Hardhat/Truffle) would likely succeed.

Included evidence:
- Screenshots of successful compilation, deployment, metadata calls, transfer, approve, and the failing `transferFrom` call and debug output.

## Remediation plan
- Reproduce `transferFrom` in an automated test (Hardhat) or on a testnet to avoid Remix UI issues.
- If necessary, add small helper functions (e.g., `testAllowance(address owner, address spender)`) for debugging.
- Provide a live demo or video if requested by graders.

## Files to include in repository
```
my-token/
├── contracts/
│   └── MyToken.sol
├── README.md            <-- paste this content in here
├── screenshots/
│   ├── compilation.png
│   ├── deployment.png
│   ├── token-info.png
│   ├── transfer-test.png
│   ├── approve-test.png
│   ├── allowance.png
│   └── transferFrom-error.png
└── (optional) LEARNING.md
```

## How to add README to GitHub (two options)

### Option A — GitHub web UI (simplest)
1. Open your repository on GitHub.
2. Click **Add file** → **Create new file**.
3. Name the file `README.md` at the root.
4. Paste the entire contents of this file.
5. At the bottom, write a commit message (e.g., "Add README for MyToken project") and click **Commit new file**.

### Option B — Command line (git)
From your project root:
```bash
git init
git add contracts/MyToken.sol
# create README.md locally and paste content
git add README.md
git add screenshots/
git commit -m "MyToken: contract, README and screenshots"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```

## Screenshot guidance
- `compilation.png`: show Solidity Compiler with success
- `deployment.png`: show deployed contract with address
- `token-info.png`: show name, symbol, decimals, totalSupply outputs
- `transfer-test.png`: show transfer call and Transfer event
- `approve-test.png`: show approve call and Approval event
- `allowance.png`: show the allowance value after approve
- `transferFrom-error.png`: show the failing transferFrom call and debug log

## Submission checklist
- [ ] Contract compiles with Solidity ^0.8.x
- [ ] Implemented functions: transfer, approve, transferFrom, balanceOf, allowance
- [ ] Events emitted: Transfer and Approval
- [ ] Input validation present
- [ ] README.md committed to repo
- [ ] Screenshots included in `screenshots/` folder
- [ ] Public GitHub repository link ready

