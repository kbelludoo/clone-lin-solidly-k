# clone-lin-solidly-k

Experimental i64-scale LIN clone of [transmissions11/solidly](https://github.com/transmissions11/solidly) BaseV1Pair **cubic k** (`x³y + y³x`) versus volatile `xy`.

- **LIN clone:** [`src/lin_solidly_k.lin`](src/lin_solidly_k.lin)
- **C11 oracle:** [`test/oracles/solidly_k_c11.c`](test/oracles/solidly_k_c11.c)
- **Proofs (results only) live in lin-open:** https://github.com/kbelludoo/lin-open
- **Event receipt:** `docs/events/EVENT_SOLIDLY_K_CLONE_LIN.rulel` in lin-open

This repository is the standalone LIN copy. It is **not** a Velodrome/Aerodrome pool, **not** a Pair factory, and does **not** claim Solidity uint256 / mainnet / WAD=1e18 parity (sequential 1e18 cubes overflow signed i64). The kernel is the Solidly **stable cubic invariant** plus Newton `_get_y`. It is **not** Uniswap V2 `xy`, **not** Uniswap V3 sqrt-price, **not** Saddle/Curve `getD`, and **not** Maker `rpow`.

Velodrome (Optimism) and Aerodrome (Base) inherit this MIT kernel. The pin is Solidly, not the BUSL Velodrome tree.

Upstream is MIT. This LIN kernel is an original uint64-scale implementation of the published `_k` / `_f` / `_d` / `_get_y` formulas at WAD'=1e6, not a verbatim Solidity dump. `_k` / `_get_y` are rejected by the Solidity transpiler (`REJ_SOLIDITY_SOLIDLY_K` / `REJ_SOLIDITY_GET_Y`).

Pinned upstream: commit `a71fe0dc42925709e138d5f1c9ecc4b2a2f3acf5`, `contracts/BaseV1-core.sol` sha256 `24f7939bf6e030f59889de6b4bad40e0814e910137044b8661d776ee6b33e59f` blob `5827d9fdff7442b4075cd0e0b7f236719e0d017c`.

Canonical goldens (uint64-scale, 128-bit product, WAD'=1e6, 1 bps fee unless noted):

- `k_stable(1e6,1e6)` = **2000000**; `k_vol(1e6,1e6)` = **1000000000000**
- `k_stable(2e6,2e6)` = **32000000**; `k_stable(1e6,2e6)` = **10000000**
- equal-pool 10k in: stable out = **9997**; volatile xy out = **9900**
- equal-pool 10k in, fee 0: stable out = **9999**
- equal-pool 100k in: stable **99939** vs volatile **90900**
- imbalanced 1.2e6/0.8e6 10k in: stable **9827** vs volatile **6610**
- naive i64 `(5e9*5e9)/1e6` wraps to **6553255926290**; 128-bit muldiv = **25000000000000**
- `1e18 * 1e18 / 1` fail-closes to **0**
