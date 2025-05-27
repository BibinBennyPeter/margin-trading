# Margin Trading Smart Contract

A Solidity-based margin trading manager for decentralized finance (DeFi) protocols. This contract enables users to deposit collateral, open leveraged long and short trading positions, and manage risk and liquidation—all on-chain.

## Features

- **Multi-collateral Support**: Accepts ETH and configurable ERC20 tokens as collateral.
- **Leverage Trading**: Supports up to configurable maximum leverage (default: 100x).
- **Long & Short Positions**: Open both LONG and SHORT positions with margin.
- **Stop Loss / Take Profit**: SL/TP parameters available per position.
- **On-chain Liquidation**: Integrates with a customizable liquidation engine.
- **Fee Structure**: Configurable open and close trading fees, with a dedicated fee collector.
- **Non-reentrancy**: Security against reentrancy attacks.
- **OpenZeppelin SafeERC20**: Secure handling of ERC20 tokens.

## Contracts

- [`MarginTradeManager.sol`](contracts/MarginTradeManager.sol)  
  Main contract for managing positions, margin, and trading logic.
- [`interfaces/ILiquidationEngine.sol`](contracts/interfaces/ILiquidationEngine.sol)  
  Interface for external liquidation logic.

## How It Works

1. **Deposit Margin**
   - Users can deposit ETH or supported ERC20 tokens.
2. **Open Position**
   - Open a leveraged LONG or SHORT position with parameters:
     - Position size
     - Leverage
     - SL/TP
     - Reduce-only flag
3. **Update / Close Position**
   - Update metrics or close position for realized PnL and margin.
   - Fees are calculated and deducted on both opening and closing.
4. **Liquidation**
   - Liquidation engine checks margin ratios and triggers position closure if needed.
5. **Withdraw Margin**
   - Withdraw unused margin at any time.

## Contract Overview

### Main Structs

- **Position**
  - `owner`: Position holder.
  - `collateralToken`: The collateral asset (ETH or ERC20).
  - `margin`: Current margin in collateral.
  - `positionSize`: Notional size of the position.
  - `entryPrice`: Price at position open.
  - `leverage`: Leverage used.
  - `positionType`: LONG or SHORT.
  - `realizedPnL`: Realized profit and loss.
  - ...and more.

### Key Functions

- `depositMargin()` / `depositMarginERC20(token, amount)`
- `withdrawMargin(positionId, amount)`
- `openPosition(positionSize, leverage, sltp, reduceOnly, positionType)`
- `closePosition(positionId)`
- `updatePosition(positionId)`

### Events

- `MarginDeposited`
- `MarginWithdrawn`
- `PositionOpened`
- `PositionClosed`
- `PositionUpdated`

## Security

- Uses OpenZeppelin's SafeERC20 for token transfers.
- Non-reentrancy guards.
- Only owner can change key parameters.
- Liquidation logic is modular via `ILiquidationEngine`.

## Contributing

PRs and issues are welcome! Please open an issue to discuss your proposed changes or improvements.

---
