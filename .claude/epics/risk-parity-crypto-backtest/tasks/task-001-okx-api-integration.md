---
name: okx-api-integration
title: OKX API Integration
epic: risk-parity-crypto-backtest
status: pending
priority: critical
created: 2025-09-06T08:38:40Z
updated: 2025-09-06T08:38:40Z
estimated_hours: 16
dependencies: []
tags: [backend, api, data]
---

# Task: OKX API Integration

## Description
Implement comprehensive OKX V5 API integration for cryptocurrency market data fetching, including market cap rankings, historical price data, and real-time updates with proper rate limiting and error handling.

## Acceptance Criteria
- [ ] **Market Cap Rankings**: Fetch and cache top-50 cryptocurrencies by market capitalization
- [ ] **Historical Data**: Retrieve 5+ years of daily OHLCV data for selected assets
- [ ] **Rate Limiting**: Implement token bucket algorithm respecting OKX API limits (20 requests/2s)
- [ ] **Error Handling**: Retry logic with exponential backoff for failed API calls
- [ ] **Data Validation**: Verify data completeness and handle missing/corrupt data points
- [ ] **Caching System**: Store fetched data in JSON/parquet files with timestamp tracking
- [ ] **API Key Management**: Secure configuration and rotation of OKX API credentials

## Technical Requirements
### API Endpoints Integration
- `/api/v5/market/tickers` - Market cap and 24h volume data
- `/api/v5/market/history-candles` - Historical OHLCV data
- `/api/v5/public/instruments` - Cryptocurrency instrument details

### Data Structure
```python
# Market data structure
{
    "symbol": "BTC-USDT",
    "market_cap_rank": 1,
    "price": 45000.0,
    "volume_24h": 1000000000.0,
    "timestamp": "2025-09-06T08:38:40Z"
}

# OHLCV data structure
{
    "symbol": "BTC-USDT",
    "timestamp": "2025-09-06T00:00:00Z",
    "open": 44500.0,
    "high": 45200.0,
    "low": 44300.0,
    "close": 45000.0,
    "volume": 15000.0
}
```

### Implementation Details
- **FastAPI Service**: Create dedicated API client class with async/await
- **Rate Limiting**: Use `asyncio.Semaphore` and `time.sleep()` for request throttling
- **Caching**: Implement file-based cache with TTL and automatic refresh
- **Error Recovery**: Handle HTTP errors, timeouts, and API maintenance windows
- **Logging**: Comprehensive logging for API calls, cache hits/misses, and errors

## Files to Create/Modify
- `backend/services/okx_client.py` - Main API client implementation
- `backend/models/market_data.py` - Pydantic models for data structures
- `backend/utils/cache.py` - File-based caching utilities
- `backend/config/okx_config.py` - API configuration and credentials
- `tests/test_okx_integration.py` - Comprehensive API client tests

## Testing Strategy
- **Unit Tests**: Mock API responses and test error scenarios
- **Integration Tests**: Real API calls with rate limiting validation
- **Cache Tests**: Verify cache invalidation and refresh logic
- **Performance Tests**: Measure API response times and throughput

## Success Metrics
- **Reliability**: >99% successful API call rate under normal conditions
- **Performance**: Fetch 3 years of data for 10 assets in <30 seconds
- **Cache Efficiency**: >95% cache hit rate for repeated historical data requests
- **Error Recovery**: Graceful degradation during API outages with cached data

## Dependencies
- OKX V5 API documentation and rate limits
- FastAPI and httpx for async HTTP client
- pandas/numpy for data manipulation
- pytest for testing framework

## Notes
- Prioritize data quality over speed - validate all API responses
- Consider implementing API key rotation for production deployment
- Monitor OKX API status and maintenance schedules