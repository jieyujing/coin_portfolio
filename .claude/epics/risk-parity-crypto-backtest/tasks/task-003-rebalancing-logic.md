---
name: rebalancing-logic
title: Rebalancing Logic Engine
epic: risk-parity-crypto-backtest
status: pending
priority: high
created: 2025-09-06T08:38:40Z
updated: 2025-09-06T08:38:40Z
estimated_hours: 14
dependencies: [risk-parity-calculator]
tags: [backend, portfolio, trading]
---

# Task: Rebalancing Logic Engine

## Description
Implement sophisticated portfolio rebalancing system with 5/25 rules, market cap buffer zones, transaction cost modeling, and forced periodic rebalancing. Handle asset entry/exit decisions and optimize trade execution timing.

## Acceptance Criteria
- [ ] **5/25 Rebalancing Rules**: Trigger rebalancing when absolute deviation ≥5% OR relative deviation ≥25%
- [ ] **Buffer Zone Logic**: Assets enter at rank 8-9, exit at rank 12-15 (configurable)
- [ ] **Transaction Cost Model**: Include realistic trading fees, slippage, and market impact
- [ ] **Forced Rebalancing**: Mandatory rebalancing every 100 days regardless of thresholds
- [ ] **Trade Optimization**: Minimize transaction costs while maintaining risk parity targets
- [ ] **Portfolio Tracking**: Maintain accurate position tracking with drift calculation
- [ ] **Rebalancing History**: Log all rebalancing events with detailed attribution

## Technical Requirements
### 5/25 Rule Implementation
```python
def check_rebalancing_trigger(current_weights, target_weights, threshold_config):
    """
    Check if rebalancing is needed based on 5/25 rules
    """
    absolute_dev = np.abs(current_weights - target_weights)
    relative_dev = absolute_dev / target_weights
    
    # Rule 1: Absolute deviation >= 5%
    absolute_trigger = np.any(absolute_dev >= threshold_config['absolute_threshold'])
    
    # Rule 2: Relative deviation >= 25%
    relative_trigger = np.any(relative_dev >= threshold_config['relative_threshold'])
    
    return absolute_trigger or relative_trigger
```

### Buffer Zone System
```python
def update_portfolio_composition(current_assets, market_cap_rankings, buffer_config):
    """
    Handle asset entry/exit based on market cap buffer zones
    """
    entry_threshold = buffer_config['entry_rank']  # 8-9
    exit_threshold = buffer_config['exit_rank']   # 12-15
    
    # Add new assets entering top ranks
    new_assets = [asset for asset in market_cap_rankings 
                  if asset.rank <= entry_threshold and asset not in current_assets]
    
    # Remove assets falling below exit threshold
    exit_assets = [asset for asset in current_assets 
                   if market_cap_rankings[asset].rank >= exit_threshold]
    
    return new_assets, exit_assets
```

### Transaction Cost Model
- **Trading Fees**: 0.1% per trade (OKX spot trading fee)
- **Slippage**: 0.05% for liquid assets, 0.2% for smaller caps
- **Market Impact**: Proportional to trade size and asset liquidity
- **Spread Costs**: Bid-ask spread impact for immediate execution

## Files to Create/Modify
- `backend/engines/rebalancing_engine.py` - Core rebalancing logic
- `backend/models/rebalancing.py` - Rebalancing event data structures
- `backend/services/transaction_costs.py` - Cost modeling and calculation
- `backend/utils/portfolio_tracking.py` - Position and drift tracking
- `tests/test_rebalancing.py` - Comprehensive rebalancing tests

## Implementation Details
### Rebalancing Decision Flow
1. **Daily Check**: Evaluate current vs target weights
2. **Trigger Assessment**: Check 5/25 rules and forced timing
3. **Composition Update**: Handle asset entry/exit from buffer zones
4. **Cost Analysis**: Calculate transaction costs for proposed trades
5. **Trade Optimization**: Minimize costs while meeting risk targets
6. **Execution Simulation**: Apply trades and update portfolio state
7. **Event Logging**: Record detailed rebalancing attribution

### Optimization Logic
- **Minimal Trading**: Only trade assets exceeding rebalancing thresholds
- **Proportional Adjustment**: Scale trades to minimize total transaction costs
- **Timing Flexibility**: Defer non-critical trades if costs are excessive
- **Risk Priority**: Maintain risk parity targets over cost minimization

## Testing Strategy
- **Unit Tests**: Individual rebalancing rule components
- **Scenario Tests**: Various market conditions and portfolio states
- **Cost Model Tests**: Verify transaction cost calculations
- **Historical Tests**: Backtest rebalancing decisions on historical data
- **Edge Case Tests**: Extreme market movements and composition changes

## Success Metrics
- **Trigger Accuracy**: Correct rebalancing decisions 99%+ of the time
- **Cost Efficiency**: Average transaction costs <0.5% per rebalancing event
- **Risk Maintenance**: Portfolio stays within 2% of risk parity targets between rebalances
- **Performance**: Rebalancing decision in <1 second for 20-asset portfolio
- **Documentation**: Complete audit trail for all rebalancing events

## Dependencies
- Risk parity calculator for target weight computation
- Market cap data from OKX API for composition decisions
- Historical price data for transaction cost estimation

## Edge Cases to Handle
- **Market Crashes**: Extreme volatility causing frequent rebalancing
- **New Asset Listings**: Handle assets entering top market caps
- **Delisted Assets**: Forced exit when assets become unavailable
- **Data Gaps**: Missing price data affecting rebalancing decisions
- **API Failures**: Graceful degradation when unable to fetch latest data

## Notes
- Balance between theoretical optimality and practical implementation
- Consider crypto market microstructure (24/7 trading, no closing prices)
- Implement configurable parameters for different risk preferences
- Plan for extension to more sophisticated rebalancing algorithms