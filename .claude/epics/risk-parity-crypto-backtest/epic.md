---
name: risk-parity-crypto-backtest
status: backlog
created: 2025-09-06T08:34:36Z
progress: 0%
prd: .claude/prds/risk-parity-crypto-backtest.md
github: https://github.com/jieyujing/coin_portfolio/issues/1
---

# Epic: Risk Parity Crypto Backtest

## Overview
Build a focused cryptocurrency backtesting platform that implements risk parity portfolio construction using OKX V5 API data. The system will dynamically select top-N cryptocurrencies, apply equal risk contribution weighting with 5/25 rebalancing rules, and provide performance analysis through a clean web interface with benchmark comparisons.

## Architecture Decisions
- **Backend**: FastAPI for async portfolio calculations and API integration
- **Frontend**: React with lightweight charting library (Chart.js/Recharts)
- **Data Storage**: File-based caching for simplicity, avoiding database complexity
- **API Integration**: Direct OKX V5 REST API with built-in rate limiting
- **Deployment**: Single container deployment for initial version
- **Portfolio Engine**: NumPy/pandas for risk parity calculations

## Technical Approach

### Frontend Components
- **Main Dashboard**: Single-page application with parameter controls and results
- **Interactive Charts**: NAV performance curves and drawdown visualization
- **Parameter Controls**: Date range, asset count, rebalancing frequency selectors
- **Results Table**: Key metrics comparison (strategy vs benchmarks)
- **Export Button**: Simple CSV download functionality

### Backend Services
- **Portfolio API**: Core endpoint for backtesting with risk parity logic
- **Data Fetcher**: OKX API client with caching and error handling
- **Risk Calculator**: Correlation matrix computation and weight optimization
- **Rebalancing Engine**: 5/25 rule implementation with transaction costs
- **Benchmark Service**: BTC and equal-weight portfolio calculations

### Infrastructure
- **File-based Caching**: Store historical price data in JSON/parquet files
- **Rate Limiting**: Simple token bucket for OKX API compliance
- **Error Handling**: Retry logic with exponential backoff for API failures
- **Performance**: Vectorized operations for fast portfolio calculations

## Implementation Strategy
1. **MVP Core**: Basic risk parity engine with OKX data fetching
2. **Web Interface**: Simple React dashboard for parameter input and results display
3. **Benchmarking**: Add BTC and equal-weight comparisons
4. **Polish**: Export functionality and performance optimization

## Tasks Created
Implementation broken down into 8 focused tasks:

### Core Engine Tasks (Critical Path)
- [ ] **[#2](https://github.com/jieyujing/coin_portfolio/issues/2)**: OKX API Integration (16h)
  - Market cap rankings, historical data, rate limiting, caching
- [ ] **[#3](https://github.com/jieyujing/coin_portfolio/issues/3)**: Risk Parity Calculator (20h)  
  - Equal risk contribution algorithm, correlation matrices, optimization
- [ ] **[#4](https://github.com/jieyujing/coin_portfolio/issues/4)**: Rebalancing Logic Engine (14h)
  - 5/25 rules, buffer zones, transaction costs, portfolio tracking

### User Interface Tasks
- [ ] **[#5](https://github.com/jieyujing/coin_portfolio/issues/5)**: React Dashboard Interface (18h)
  - Parameter controls, interactive charts, results display, real-time updates
- [ ] **[#6](https://github.com/jieyujing/coin_portfolio/issues/6)**: Benchmark Comparison Engine (12h)
  - Bitcoin and equal-weight benchmarks, performance metrics, statistical analysis

### Supporting Tasks
- [ ] **[#7](https://github.com/jieyujing/coin_portfolio/issues/7)**: Export System Implementation (10h)
  - CSV downloads, transaction logs, performance summaries
- [ ] **[#8](https://github.com/jieyujing/coin_portfolio/issues/8)**: Performance Testing & Validation (16h)
  - Calculation accuracy, load testing, historical validation
- [ ] **[#9](https://github.com/jieyujing/coin_portfolio/issues/9)**: Documentation & User Guide (12h)
  - User tutorials, API docs, methodology explanations

**Total Estimated Effort: 118 hours (≈ 15 weeks at 8h/week)**
## Dependencies
- **External**: OKX V5 API availability and stable market data
- **Libraries**: pandas/numpy for calculations, React for UI
- **Infrastructure**: Simple file system for data caching
- **No Database**: Avoid PostgreSQL/Redis complexity for initial version

## Success Criteria (Technical)
- **Speed**: Complete 3-year backtest in <15 seconds
- **Accuracy**: Match manual risk parity calculations within 0.1%
- **Reliability**: Handle API failures gracefully with cached data
- **Usability**: Intuitive parameter selection with immediate results
- **Scalability**: Support 10+ concurrent users without performance degradation

## Estimated Effort
- **Overall Timeline**: 8-10 weeks (simplified from original 12-14 weeks)
- **Resource Requirements**: 1 full-stack developer
- **Critical Path**: OKX integration → Risk parity engine → Web dashboard
- **Simplified Scope**: Remove PostgreSQL, Redis, PDF exports, advanced UI features
