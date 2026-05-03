# ScoutBot Inbox — Pending Triage

**This file is auto-appended to by ScoutBot's nightly run (todo_extractor.py).**
**Items here are PROPOSED, not active. Claude Chat triages → promotes accepted items to WORKSPACE.md "Open tasks" → removes promoted items from this file.**

## Triage workflow

1. ScoutBot writes new SCT-AUTO-N rows under "## Pending items" each night.
2. Claude Chat reviews during morning session, deduplicates against WORKSPACE + LESSONS + research_queue + scout_quality_log.
3. Accepted items: copy to WORKSPACE.md "Open tasks by bot" with appropriate priority.
4. Rejected items: log to scout_quality_log.md with reason.
5. After triage, remove the row from this file's "Pending items" section.
| SCT-AUTO-935 | CopyBot | Extract wallet address from the referenced Polymarket account in this thread and run `python3 harvester.py --add {wal... | — | ACTIVE | — | 7.9 | 2026-04-25 |
| SCT-AUTO-936 | CopyBot | Define a behavioral pattern rule in CopyBot's wallet scoring for pre-resolution concentration in low-liquidity weathe... | — | ACTIVE | — | 7.9 | 2026-04-25 |
| SCT-AUTO-937 | CopyBot | Flag to WeatherBot maintainer: physical sensor manipulation is a plausible NOAA data integrity attack vector — assess... | — | ACTIVE | — | 7.9 | 2026-04-25 |
| SCT-AUTO-938 | CopyBot | Extract wallet address from the referenced thread and run `python3 harvester.py --add {wallet}` — validate on-chain P... | — | ACTIVE | — | 7.8 | 2026-04-25 |
| SCT-AUTO-939 | CopyBot | If on-chain P&L confirms >= $200K, flag wallet for CopyBot basket consensus (MON-5) as NICHE_SPECIALIST (split-featur... | — | ACTIVE | — | 7.8 | 2026-04-25 |
| SCT-AUTO-940 | CopyBot | Assess whether CopyBot's current inclusion pipeline misclassifies split-feature traders due to near-zero cost basis d... | — | ACTIVE | — | 7.8 | 2026-04-25 |
| SCT-AUTO-941 | CopyBot | Extract wallet address from thread and run `python3 harvester.py --add {wallet}` — validate on-chain deposit size, en... | — | ACTIVE | — | 7.3 | 2026-04-25 |
| SCT-AUTO-942 | CopyBot | Audit MON-36 / RULE 22 contradiction guard to determine whether a short-window all-in-then-reverse sequence on a fres... | — | ACTIVE | — | 7.3 | 2026-04-25 |
| SCT-AUTO-943 | ALL | Benchmark `polyfill-rs` vs. `py_clob_client` on `market_is_tradeable` and `get_best_ask` call latency under live Poly... | — | ACTIVE | — | 7.2 | 2026-04-25 |
| SCT-AUTO-944 | ALL | Assess PyO3 FFI binding feasibility for exposing `polyfill-rs` market data functions to the Python bot suite's hot pa... | — | ACTIVE | — | 7.2 | 2026-04-25 |
| SCT-AUTO-945 | ALL | If FFI binding is feasible, evaluate KrajekBot's 15m price drift check as a secondary integration candidate after Pol... | — | ACTIVE | — | 7.2 | 2026-04-25 |
| SCT-AUTO-946 | ALL | Review five-agent LLM ensemble voting architecture in `muxprotocol/kalshi-trading-bot` for applicability to CopyBot's... | — | ACTIVE | — | 7.2 | 2026-04-25 |
| SCT-AUTO-947 | ALL | Audit fractional Kelly criterion implementation for cross-market position sizing validation — determine whether it ca... | — | ACTIVE | — | 7.2 | 2026-04-25 |
| SCT-AUTO-948 | CopyBot | Audit size capping and safety rail logic in `figure-markets/polymarket-copy-trading-bot` for compatibility and gap an... | — | ACTIVE | — | 7.1 | 2026-04-25 |
| SCT-AUTO-949 | CopyBot | Review portfolio tracking and performance history methodology for trader classification — assess whether scoring or w... | — | ACTIVE | — | 7.1 | 2026-04-25 |
| SCT-AUTO-950 | CopyBot | Cross-validate contradiction detection logic against CopyBot's RULE 22 implementation — determine whether the referen... | — | ACTIVE | — | 7.1 | 2026-04-25 |
| SCT-AUTO-951 | ALL | Audit EIP-712 signing implementation in `signum` against the bot suite's current order-signing and key management app... | — | ACTIVE | — | 6.2 | 2026-04-25 |
| SCT-AUTO-952 | ALL | Cross-reference `signum`'s Rust/Python execution bridge pattern against SCT-AUTO-944 PyO3 feasibility assessment — de... | — | ACTIVE | — | 6.2 | 2026-04-25 |
| SCT-AUTO-953 | ALL | Establish a lightweight Chainlink release-watch process — flag any future release changelog entries referencing oracl... | — | ACTIVE | — | 6.0 | 2026-04-25 |
| SCT-AUTO-954 | CopyBot | Extract wallet address associated with @bored2boar Twitter account; run `python3 harvester.py --add {wallet_address}`... | — | ACTIVE | — | 5.8 | 2026-04-25 |
| SCT-AUTO-955 | CopyBot | If on-chain P&L on the @megaeth TGE market exceeds 30% ROI with confirmed pre-announcement entry, flag @bored2boar as... | — | ACTIVE | — | 5.8 | 2026-04-25 |
| SCT-AUTO-956 | ALL | Verify Polymarket perps availability and liquidity via API — confirm whether perps order flow is accessible programma... | — | ACTIVE | — | 5.7 | 2026-04-25 |
| SCT-AUTO-957 | ALL | If SCT-AUTO-956 confirms accessible perps liquidity, evaluate whether a perps hedging layer should be introduced into... | — | ACTIVE | — | 5.7 | 2026-04-25 |
| SCT-AUTO-958 | CopyBot | Review WebSocket order book refresh implementation in `Elshen7/PolymarketBot` — assess whether its streaming cadence ... | — | ACTIVE | — | 5.6 | 2026-04-25 |
| SCT-AUTO-959 | CopyBot | Evaluate two-leg arb state machine architecture for structural patterns applicable to CopyBot's MON-5 basket consensu... | — | ACTIVE | — | 5.6 | 2026-04-25 |
| SCT-AUTO-960 | CopyBot | Run `python3 harvester.py --add 0x5b6331e7ff0831a3fe2ed12004747db1a9c911a4` to ingest neutralwave23's trade history; ... | — | ACTIVE | — | 5.4 | 2026-04-25 |
| SCT-AUTO-961 | CopyBot | If SCT-AUTO-960 confirms ≥3 winning markets with consistent category concentration and no single trade accounting for... | — | ACTIVE | — | 5.4 | 2026-04-25 |
| SCT-AUTO-962 | ALL | If a multi-venue expansion roadmap is formally initiated, evaluate SocialPredict's API surface, liquidity depth, and ... | — | ACTIVE | — | 5.3 | 2026-04-25 |
| SCT-AUTO-963 | CopyBot | Run `python3 harvester.py --add 0x84cfffc3f16dcc353094de30d4a45226eccd2f63` to ingest mooseborzoi's trade history; pu... | — | ACTIVE | — | 5.2 | 2026-04-25 |
| SCT-AUTO-964 | CopyBot | If SCT-AUTO-963 confirms ≥3 winning markets with consistent category concentration and no single trade accounting for... | — | ACTIVE | — | 5.2 | 2026-04-25 |
| SCT-AUTO-965 | ALL | Retrieve and review the arbitrage guide linked in @kirillk_web3's post — assess whether it covers Kalshi/Polymarket o... | — | ACTIVE | — | 5.1 | 2026-04-25 |
| SCT-AUTO-966 | CopyBot | Run `python3 harvester.py --add 0x6ac5bb06a9eb05641fd5e82640268b92f3ab4b6e` to ingest Lakersfan111's trade history; v... | — | ACTIVE | — | 5.1 | 2026-04-25 |
| SCT-AUTO-967 | CopyBot | If SCT-AUTO-966 confirms realized P&L dominance, ≥3 winning markets with consistent category concentration, and no si... | — | ACTIVE | — | 5.1 | 2026-04-25 |
| SCT-AUTO-968 | ALL | Obtain official Polymarket migration announcement, target timeline, and new chain RPC/API specs — confirm chain ident... | — | ACTIVE | — | 8.2 | 2026-04-26 |
| SCT-AUTO-969 | ALL | Once specs are available, audit all five bots for hard-coded or Polygon-assumed dependencies — gas estimation logic (... | — | ACTIVE | — | 8.2 | 2026-04-26 |
| SCT-AUTO-970 | ALL | Assess cross-chain liquidity fragmentation risk during the transition window — model the impact on PolyArb oracle lag... | — | ACTIVE | — | 8.2 | 2026-04-26 |
| SCT-AUTO-971 | CopyBot | Run `python3 harvester.py --add 0x2dc13c6bda81b202281e796953a7323de675b33c` to ingest xifutloong3's trade history; pu... | — | ACTIVE | — | 7.2 | 2026-04-26 |
| SCT-AUTO-972 | CopyBot | Run `python3 harvester.py --add 0x91583ceb1ebec79951a068e1d7d02c1ea590fa7b` to ingest GlobalizeTheHorchata's trade hi... | — | ACTIVE | — | 6.9 | 2026-04-26 |
| SCT-AUTO-973 | CopyBot | Run `python3 harvester.py --add 0x375191b48ef5761e0c621b8e4a5374e964937dfd` to ingest bondfolio's trade history; vali... | — | ACTIVE | — | 6.9 | 2026-04-26 |
| SCT-AUTO-974 | CopyBot | Run `python3 harvester.py --add 0xefddbb135e2cc2648e3ca6a6b3d4fa4994d5017f` to ingest maxgreen's trade history; valid... | — | ACTIVE | — | 6.6 | 2026-04-26 |
| SCT-AUTO-975 | CopyBot | Run `python3 harvester.py --add 0xc97485a44146b5db0b8b121358f57cf297cdb33b` to ingest lu1zzz's trade history; validat... | — | ACTIVE | — | 6.6 | 2026-04-26 |
| SCT-AUTO-976 | CopyBot | Run `python3 harvester.py --add 0x21f333307ac6d2e65ac82f014b3dd2b64cbc35e3` to ingest hi774c's trade history; validat... | — | ACTIVE | — | 6.5 | 2026-04-26 |
| SCT-AUTO-977 | CopyBot | Run `python3 harvester.py --add 0xdd92232bcdfbbac04132b3cbacbf32c2e5b16b2a` to ingest Jdhdhduu's trade history; valid... | — | ACTIVE | — | 6.3 | 2026-04-26 |
| SCT-AUTO-978 | CopyBot | Run `python3 harvester.py --add 0x12ec6d71325afac2b4d25b3de94185e2c48d41ae` to ingest olegio's trade history; validat... | — | ACTIVE | — | 6.2 | 2026-04-26 |
| SCT-AUTO-979 | CopyBot | Run `python3 harvester.py --add 0x39d3c773be30fcc73161fc6768f46d563a779ef0` to ingest matanovik's trade history; vali... | — | ACTIVE | — | 6.1 | 2026-04-26 |
| SCT-AUTO-980 | CopyBot | Run `python3 harvester.py --add 0xa38a455bbdd4b68486548b7e19da99903f4f821d` to ingest takeormake's trade history; val... | — | ACTIVE | — | 6.1 | 2026-04-26 |
| SCT-AUTO-981 | CopyBot | Run `python3 harvester.py --add 0xb652e5dabc3fccd3c939acab0108f99866842db4` to ingest trade history; validate realize... | — | ACTIVE | — | 5.8 | 2026-04-26 |
| SCT-AUTO-982 | CopyBot | Run `python3 harvester.py --add 0x54d200bbe9f87704e0aa0839178187e36f41ac37` to ingest trade history; validate realize... | — | ACTIVE | — | 5.8 | 2026-04-26 |
| SCT-AUTO-983 | CopyBot | Reach out to @LunarResearcher to clarify the green-column filtering metric (profitability ratio, ROI, volume-adjusted... | — | ACTIVE | — | 5.6 | 2026-04-26 |
| SCT-AUTO-984 | CopyBot | Run `python3 harvester.py --add 0x9495425feeb0c250accb89275c97587011b19a27` to ingest trade history; validate realize... | — | ACTIVE | — | 5.4 | 2026-04-26 |
| SCT-AUTO-985 | ALL | Review figure-markets/polymarket-arbitrage-bot commits from 2026-01 to 2026-04 for edge refinements in BTC/ETH/SOL/XR... | — | ACTIVE | — | 5.0 | 2026-04-26 |
| SCT-AUTO-986 | ALL | Monitor FundingPredicts launch timeline and TVL growth; if TVL exceeds $1M, reassess spread and liquidity conditions ... | — | ACTIVE | — | 5.0 | 2026-04-26 |
| SCT-AUTO-987 | CopyBot | Analyze opinion-whale-tracker source for wallet-discovery and pre-repricing detection patterns reusable in Polymarket... | — | ACTIVE | — | 8.0 | 2026-04-27 |
| SCT-AUTO-988 | CopyBot | Assess Opinion market (BNB Chain) liquidity depth and CLOB structure; if average daily volume and spread conditions a... | — | ACTIVE | — | 8.0 | 2026-04-27 |
| SCT-AUTO-989 | CopyBot | Run `python3 harvester.py --add 0x951bd740ef` to ingest full trade history; validate realized vs. unrealized P&L spli... | — | ACTIVE | — | 7.6 | 2026-04-27 |
| SCT-AUTO-990 | CopyBot | Retrieve and analyze @bored2boar's published strategy documentation for replicable edge patterns (entry timing logic,... | — | ACTIVE | — | 7.6 | 2026-04-27 |
| SCT-AUTO-991 | ALL | Review Kronos foundation model architecture and training signal composition in alex-jb/orallexa-ai-trading-agent; ass... | — | ACTIVE | — | 7.2 | 2026-04-27 |
| SCT-AUTO-992 | ALL | Compare Bull/Bear/Judge adversarial consensus implementation against CopyBot's MON-5 consensus gates; determine wheth... | — | ACTIVE | — | 7.2 | 2026-04-27 |
| SCT-AUTO-993 | CopyBot | Run `python3 harvester.py --add 0x4bff30af91642dc7d2b19a8664378fe55c45fc26` to ingest full trade history for Sassy-Bu... | — | ACTIVE | — | 7.1 | 2026-04-27 |
| SCT-AUTO-994 | ALL | Evaluate kalshi-neural-predictor accuracy metrics (Brier score, log-loss) and feature engineering approach; determine... | — | ACTIVE | — | 7.0 | 2026-04-27 |
| SCT-AUTO-995 | ALL | Assess generalization gap between Kalshi and Polymarket market structure (resolution rules, user base, liquidity prof... | — | ACTIVE | — | 7.0 | 2026-04-27 |
| SCT-AUTO-996 | CopyBot | Run `python3 harvester.py --add 0xaa075924e1dc7cff3b9fab67401126338c4d2125` to ingest full trade history for rustin; ... | — | ACTIVE | — | 6.6 | 2026-04-27 |
| SCT-AUTO-997 | CopyBot | Run `python3 harvester.py --add 0x72d815133f9f8b6529e911cf3be492846ce05213` to ingest full trade history for Vatrer; ... | — | ACTIVE | — | 6.5 | 2026-04-27 |
| SCT-AUTO-998 | CopyBot | Run `python3 harvester.py --add 0xf705fa045201391d9632b7f3cde06a5e24453ca7` to ingest full trade history; validate re... | — | ACTIVE | — | 6.5 | 2026-04-27 |
| SCT-AUTO-999 | CopyBot | Run `python3 harvester.py --add 0xbf3020eb8556e5ec402dbdda2fedbc0da7788b95` to ingest full trade history for REWGTD; ... | — | ACTIVE | — | 6.4 | 2026-04-27 |
| SCT-AUTO-1000 | ALL | Analyze LunarResearcher's YES+NO sum detection logic — extract threshold parameters and trigger conditions from the t... | — | ACTIVE | — | 6.3 | 2026-04-27 |
| SCT-AUTO-1001 | ALL | Review atomic dual-side execution implementation described in the thread; assess whether PolyArb's current execution ... | — | ACTIVE | — | 6.3 | 2026-04-27 |
| SCT-AUTO-1002 | ALL | Audit PolyArb's oracle lag assumptions for BTC markets — determine whether the current model accounts for a ~5-minute... | — | ACTIVE | — | 5.7 | 2026-04-27 |
| SCT-AUTO-1003 | ALL | Run live feed delta comparison between Chainlink BTC price resolution and Binance/Coinbase spot at 1-minute granulari... | — | ACTIVE | — | 5.7 | 2026-04-27 |
| SCT-AUTO-1004 | CopyBot | Run `python3 harvester.py --add 0xe5b70fd855af9258d9463992e4f1ed7987905ee3` to ingest full trade history for alwaysfa... | — | ACTIVE | — | 5.4 | 2026-04-27 |
| SCT-AUTO-1005 | CopyBot | Run `python3 harvester.py --add 0x92b5f14c68ccae0cd7d0ba52b098ffaab990fec6` to ingest full trade history for Schnoodl... | — | ACTIVE | — | 5.4 | 2026-04-27 |
| SCT-AUTO-1006 | CopyBot | Run `python3 harvester.py --add 0xdf17f4a8dd01a4cfa6fc3da323a2baee5f8697d1` to ingest full trade history for Clear-Co... | — | ACTIVE | — | 5.4 | 2026-04-27 |
| SCT-AUTO-1007 | ALL | Audit all public-facing dashboards and API endpoints across the full bot suite — verify access is restricted to authe... | — | ACTIVE | — | 5.1 | 2026-04-27 |
| SCT-AUTO-1008 | ALL | Review operational security posture for remote or mobile access scenarios — determine whether team members access liv... | — | ACTIVE | — | 5.1 | 2026-04-27 |
| SCT-AUTO-1009 | CopyBot | Audit GUI trader-discovery UX patterns in 0xsebasneuron's implementation against CopyBot's activity API monitoring ap... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1010 | CopyBot | Review safe-by-default execution gates in 0xsebasneuron's implementation for edge cases not covered by CopyBot's RULE... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1011 | CopyBot | Compare 0xsebasneuron's arbitrage detection logic against CopyBot's price drift check and MON-30 flow spike model — d... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1012 | CopyBot | Extract the full wallet address from the tweet URL or linked on-chain source and run `python3 harvester.py --add <ful... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1013 | CopyBot | Evaluate whether CopyBot's MON-36 contradiction guard and RULE 22 logic can be inverted or extended to flag YES+NO co... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1014 | CopyBot | Cross-reference this wallet's active markets against MON-5 basket consensus signals to determine whether YES+NO mispr... | — | ACTIVE | — | 6.7 | 2026-04-28 |
| SCT-AUTO-1015 | CopyBot | Resolve @durrrrrrrr to a full 0x wallet address via `polymarket.com/@durrrrrrrr` and run `python3 harvester.py --add ... | — | ACTIVE | — | 6.1 | 2026-04-28 |
| SCT-AUTO-1016 | CopyBot | If on-chain validation passes, analyze trade timing and market-category distribution for options-derived behavioral p... | — | ACTIVE | — | 6.1 | 2026-04-28 |
| SCT-AUTO-1017 | CopyBot | Run `python3 harvester.py --add 0xcc87cc69b76dd01063a66825b25d6c89f1fac830` to ingest full trade history for tteqkaq;... | — | ACTIVE | — | 6.1 | 2026-04-28 |
| SCT-AUTO-1018 | CopyBot | Run `python3 harvester.py --add 0x4c353dd347c2e7d8bcdc5cd6ee569de7baf23e2f` to ingest full trade history for WangXing... | — | ACTIVE | — | 5.9 | 2026-04-28 |
| SCT-AUTO-1019 | ALL | Extract and document the Kelly sizing formula and backtesting methodology from the repo; validate against KrajekBot's... | — | ACTIVE | — | 5.8 | 2026-04-28 |
| SCT-AUTO-1020 | ALL | Review the vector search and OpenAI market insight synthesis pattern for news relevance ranking; assess whether the r... | — | ACTIVE | — | 5.8 | 2026-04-28 |
| SCT-AUTO-1021 | ALL | Review PR #22043 in the v2.44.0 release to confirm no changes to oracle feed endpoints, update cadences, or relay-pat... | — | ACTIVE | — | 5.8 | 2026-04-28 |
| SCT-AUTO-1022 | ALL | Audit v2.44.0 release notes for any Pyth or Chainlink API surface changes that could affect cross-oracle feed routing... | — | ACTIVE | — | 5.8 | 2026-04-28 |
| SCT-AUTO-1023 | CopyBot | Run `python3 harvester.py --add 0x6f8b2886707673629af6bbf7314b8081021601aa` to ingest full trade history for sunflowa... | — | ACTIVE | — | 5.4 | 2026-04-28 |
| SCT-AUTO-1024 | CopyBot | Run `python3 harvester.py --add 0xd3dff2e3b903372799b240c2338dd423086082fa` to ingest full trade history for 0xD3D; v... | — | ACTIVE | — | 5.4 | 2026-04-28 |
| SCT-AUTO-1025 | CopyBot | Run `python3 harvester.py --add 0x002217232504477043ab14d6ca12d45b95085f11` to ingest full trade history for 0xaabec1... | — | ACTIVE | — | 5.4 | 2026-04-28 |
| SCT-AUTO-1026 | CopyBot | Run `python3 harvester.py --add 0x0adedcb79423aed988a849827ff80b8cc7b3ed94` to ingest full trade history for 0Ebe; va... | — | ACTIVE | — | 5.3 | 2026-04-28 |
| SCT-AUTO-1027 | CopyBot | Run `python3 harvester.py --add 0xe40172522c7c64afa2d052ddae6c92cd0f417b88` to ingest full trade history for BoomLaLa... | — | ACTIVE | — | 5.3 | 2026-04-28 |
| SCT-AUTO-1028 | CopyBot | Run `python3 harvester.py --add 0xb7f55b6d32c2ee3768192d676cd66354f51fc669` to ingest full trade history for cassiusw... | — | ACTIVE | — | 5.1 | 2026-04-28 |
| SCT-AUTO-1029 | ALL | Review flumine order lifecycle and position reconciliation patterns as architectural reference for CopyBot state mana... | — | ACTIVE | — | 5.0 | 2026-04-28 |
| SCT-AUTO-1031 | CopyBot | Run `python3 harvester.py --add 0xe613b515bd46b1585a8b137a4d291d9b80bd540e` to ingest full trade history for Gucky-Gu... | — | ACTIVE | — | 7.7 | 2026-04-29 |
| SCT-AUTO-1032 | CopyBot | Run `python3 harvester.py --add 0xfa21179f2cef2f61cb78e8340ae62b17f9271438` to ingest full trade history for BABELCON... | — | ACTIVE | — | 7.7 | 2026-04-29 |
| SCT-AUTO-1033 | CopyBot | Run `python3 harvester.py --add 0x68146921df11eab44296dc4e58025ca84741a9e7` to ingest trade history for LynxTitan; va... | — | ACTIVE | — | 7.4 | 2026-04-29 |
| SCT-AUTO-1034 | CopyBot | Run `python3 harvester.py --add 0x76b8007b407fa14c650138b25fb25e22ac149a33` to ingest trade history for hayes99; vali... | — | ACTIVE | — | 6.9 | 2026-04-29 |
| SCT-AUTO-1035 | CopyBot | Run `python3 harvester.py --add 0x5bb0de4e97698184ead80c80cb17a26cd6f6814b` to ingest full trade history for bobe2.1;... | — | ACTIVE | — | 6.9 | 2026-04-29 |
| SCT-AUTO-1036 | CopyBot | Run `python3 harvester.py --add 0x0e1b30c63f6bac4fa1eacc7b852fa7e72e93ecaf` to ingest full trade history for sjhfccha... | — | ACTIVE | — | 6.9 | 2026-04-29 |
| SCT-AUTO-1037 | CopyBot | Run `python3 harvester.py --add 0xed107a85a4585a381e48c7f7ca4144909e7dd2e5` to ingest full trade history for bobe2; v... | — | ACTIVE | — | 6.9 | 2026-04-29 |
| SCT-AUTO-1038 | ALL | Document Predict Raven state machine for crash recovery and reconnection logic; map patterns against CopyBot and Kraj... | — | ACTIVE | — | 6.8 | 2026-04-29 |
| SCT-AUTO-1039 | ALL | Extract market selection heuristics from Predict Raven codebase if available; cross-reference against ScoutBot's curr... | — | ACTIVE | — | 6.8 | 2026-04-29 |
| SCT-AUTO-1040 | ALL | Monitor Predict Raven's live P&L trajectory on an ongoing basis; flag if sustained directional edge emerges as signal... | — | ACTIVE | — | 6.8 | 2026-04-29 |
| SCT-AUTO-1041 | CopyBot | Run `python3 harvester.py --add 0x96489abcb9f583d6835c8ef95ffc923d05a86825` to ingest full trade history for anoin123... | — | ACTIVE | — | 6.8 | 2026-04-29 |
| SCT-AUTO-1042 | CopyBot | Run `python3 harvester.py --add 0xdaad6f960d507dba148c1ff908db5a28743169cc` to ingest full trade history for junkbond... | — | ACTIVE | — | 6.4 | 2026-04-29 |
| SCT-AUTO-1043 | ALL | Review poly-web3 API surface for gas-free split/merge operations; assess compatibility with the suite's existing CLOB... | — | ACTIVE | — | 6.3 | 2026-04-29 |
| SCT-AUTO-1044 | ALL | Test Safe wallet redemption flow against the suite's current wallet infrastructure; confirm whether gas-free redempti... | — | ACTIVE | — | 6.3 | 2026-04-29 |
| SCT-AUTO-1045 | CopyBot | Run `python3 harvester.py --add 0x4f77be59e16ea2734b1e2118379d02ac321d47f4` to ingest full trade history; validate $5... | — | ACTIVE | — | 6.3 | 2026-04-29 |
| SCT-AUTO-1046 | ALL | Review PolyArb API specification for Solana wallet integration compatibility with the BTC Up/Down module; confirm whe... | — | ACTIVE | — | 6.2 | 2026-04-29 |
| SCT-AUTO-1047 | ALL | Benchmark module oracle feed latency and resolution against PolyArb's current Chainlink 5-minute BTC feed; flag any l... | — | ACTIVE | — | 6.2 | 2026-04-29 |
| SCT-AUTO-1048 | CopyBot | Run `python3 harvester.py --add 0xd809c54478c5e50fe2aa20acccf0857baa0a6542` to ingest full trade history; validate $6... | — | ACTIVE | — | 6.1 | 2026-04-29 |
| SCT-AUTO-1049 | ALL | Audit PolyClaw's hedge discovery algorithm and LLM prompt engineering for novel position-correlation heuristics; asse... | — | ACTIVE | — | 6.0 | 2026-04-29 |
| SCT-AUTO-1050 | ALL | Evaluate whether the OpenClaw framework's CLOB execution path introduces latency overhead relative to the suite's exi... | — | ACTIVE | — | 6.0 | 2026-04-29 |
| SCT-AUTO-1051 | CopyBot | Run `python3 harvester.py --add 0x24c8cf69a0e0a17eee21f69d29752bfa32e823e1` to ingest full trade history; validate $6... | — | ACTIVE | — | 5.9 | 2026-04-29 |
| SCT-AUTO-1052 | ALL | Audit the linked GitHub repository for novel analytics, orchestration patterns, or py_clob_client-compatible utilitie... | — | ACTIVE | — | 5.7 | 2026-04-29 |
| SCT-AUTO-1053 | ALL | Cross-reference the 108 tools against PolyClaw (SCT-AUTO-1049/1050) and the BTC Up/Down module (SCT-AUTO-1046/1047) a... | — | ACTIVE | — | 5.7 | 2026-04-29 |
| SCT-AUTO-1054 | ALL | Evaluate @poly_parlay basket construction for higher-order position correlation signals; assess whether parlay EV sys... | — | ACTIVE | — | 5.7 | 2026-04-29 |
| SCT-AUTO-1055 | ALL | Cross-validate the claimed 40% bot-volume figure against on-chain trade frequency, wallet clustering, and inter-trade... | — | ACTIVE | — | 5.2 | 2026-04-29 |
| SCT-AUTO-1056 | ALL | Monitor CLOB order book latency spreads for systematic algorithmic flow patterns; if adversarial bot volume is confir... | — | ACTIVE | — | 5.2 | 2026-04-29 |
| SCT-AUTO-1057 | ALL | Flag @bored2boar as a low-signal marketing account in ScoutBot's source-quality registry; deprioritize future tweets ... | — | ACTIVE | — | 5.1 | 2026-04-29 |
| SCT-AUTO-1058 | ALL | Audit ANPS-TradeMeUp entity extraction and impact-scoring pipeline for compatibility with ScoutBot's signal ingestion... | — | ACTIVE | — | 5.0 | 2026-04-29 |
| SCT-AUTO-1059 | ALL | Evaluate the multi-agent consensus pattern for applicability to KrajekBot confirmation logic or PolyArb news-driven t... | — | ACTIVE | — | 5.0 | 2026-04-29 |
| SCT-AUTO-1060 | ALL | Evaluate Envio HyperIndex integration as a replacement or supplement to existing RPC-based market state queries; benc... | — | ACTIVE | — | 7.5 | 2026-04-30 |
| SCT-AUTO-1061 | ALL | Audit indexed CTFExchange V2 contract ABIs for settlement, liquidity pool, and order fields not currently ingested by... | — | ACTIVE | — | 7.5 | 2026-04-30 |
| SCT-AUTO-1062 | ALL | Assess indexer uptime and reorg-handling behavior on Polygon; if the indexer exhibits chain-reorg lag or downtime pat... | — | ACTIVE | — | 7.5 | 2026-04-30 |
| SCT-AUTO-1063 | ALL | Cross-reference the full directory against current suite API and data-feed dependencies; flag any tools not already c... | — | ACTIVE | — | 7.3 | 2026-04-30 |
| SCT-AUTO-1064 | ALL | Audit the directory's copy-trading and alert-systems sections for wallet discovery or anomaly-detection methods not c... | — | ACTIVE | — | 7.3 | 2026-04-30 |
| SCT-AUTO-1065 | ALL | Identify the 3–5 highest-signal analytics or API tools in the directory not currently integrated into the suite; for ... | — | ACTIVE | — | 7.3 | 2026-04-30 |
| SCT-AUTO-1066 | CopyBot | Add us391 (0x3f3aa7005f8006bfcc367d43a25cde25509fe8fd) to the harvester pipeline and pull full trade history; analyze... | — | ACTIVE | — | 6.9 | 2026-04-30 |
| SCT-AUTO-1067 | CopyBot | Evaluate whether us391's low-volume, high-ROI profile warrants a separate CopyBot tracking tier for efficiency-optimi... | — | ACTIVE | — | 6.9 | 2026-04-30 |
| SCT-AUTO-1068 | CopyBot | Add melon64 (0x13337ad24df8614481895cd33d4e9d70ac50e463) to the harvester pipeline and pull full trade history; analy... | — | ACTIVE | — | 6.3 | 2026-04-30 |
| SCT-AUTO-1069 | ALL | Audit the V2 WebSocket integration for real-time spread detection patterns not currently implemented in PolyArb; cros... | — | ACTIVE | — | 5.5 | 2026-04-30 |
| SCT-AUTO-1070 | ALL | Review gas optimization heuristics (batch execution, order routing logic) for applicability to PolyArb's execution la... | — | ACTIVE | — | 5.5 | 2026-04-30 |
| SCT-AUTO-1071 | CopyBot | Add 11122 (0x011f2d377e56119fb09196dffb0948ae55711122) to the harvester pipeline at low priority; pull trade history ... | — | ACTIVE | — | 5.4 | 2026-04-30 |
| SCT-AUTO-1072 | CopyBot | Audit Struct API field coverage (PnL granularity, position history depth, activity timestamps) against the current `p... | — | ACTIVE | — | 5.2 | 2026-04-30 |
| SCT-AUTO-1073 | CopyBot | Add AV23IUa (0xdb859a551fcf56e49416160911476bea7307152f) to the harvester pipeline at low priority; use the 11.2% ROI... | — | ACTIVE | — | 5.2 | 2026-04-30 |
| SCT-AUTO-1074 | CopyBot | Integrate GhostGuard into CopyBot's post-execution monitoring layer immediately after `paper_execute()` CLOB fill con... | — | ACTIVE | — | 8.7 | 2026-05-01 |
| SCT-AUTO-1075 | CopyBot | Implement ghost fill detection within the existing position tracker to flag RULE 22 contradictions where tracked posi... | — | ACTIVE | — | 8.7 | 2026-05-01 |
| SCT-AUTO-1076 | CopyBot | Backtest GhostGuard detection logic against historical CopyBot trade logs to quantify fill authenticity rate and esta... | — | ACTIVE | — | 8.7 | 2026-05-01 |
| SCT-AUTO-1077 | CopyBot | Pull archived wallet address from the linked tweet thread and add bin8888 to the harvester pipeline immediately; conf... | — | ACTIVE | — | 7.9 | 2026-05-01 |
| SCT-AUTO-1078 | CopyBot | Monitor the anonymized wallet for continued oil-market activity; if active trading persists post-anonymization, asses... | — | ACTIVE | — | 7.9 | 2026-05-01 |
| SCT-AUTO-1079 | CopyBot | If bin8888 qualifies for active mirroring, define a commodity-market MIN_PRICE gate variant accounting for oil market... | — | ACTIVE | — | 7.9 | 2026-05-01 |
| SCT-AUTO-1080 | CopyBot | Add 0xcf609d3256f0f37f0595e5dc64012fa3a8fea6f5 to the harvester pipeline immediately (`python3 harvester.py --add 0xc... | — | ACTIVE | — | 7.4 | 2026-05-01 |
| SCT-AUTO-1081 | CopyBot | Define position-sizing guards for mirroring this wallet given its small capital base (~$13k volume); assess whether C... | — | ACTIVE | — | 7.4 | 2026-05-01 |
| SCT-AUTO-1082 | CopyBot | Identify and archive the wallet list for the BTC Up/Down bot cluster (412 wallets); pull on-chain trade history for t... | — | ACTIVE | — | 7.3 | 2026-05-01 |
| SCT-AUTO-1083 | CopyBot | Stress-test the claimed 0.6–0.9% per-capture edge against historical Polymarket BTC Up/Down market spreads and the cu... | — | ACTIVE | — | 7.3 | 2026-05-01 |
| SCT-AUTO-1084 | CopyBot | Assess distributional integrity of the 412-wallet cluster — determine whether top-performer 9% ROI is genuinely conce... | — | ACTIVE | — | 7.3 | 2026-05-01 |
| SCT-AUTO-1085 | CopyBot | Add rebotsliaf (0x206191d01c411a5b346d11a2df2d463a356b8ec9) to the harvester pipeline (`python3 harvester.py --add 0x... | — | ACTIVE | — | 7.0 | 2026-05-01 |
| SCT-AUTO-1086 | CopyBot | Apply the same position-sizing guard assessment used for 0xcf609d32 (SCT-AUTO-1081) to rebotsliaf given comparable sm... | — | ACTIVE | — | 7.0 | 2026-05-01 |
| SCT-AUTO-1087 | CopyBot | Add cryptoaxel to the harvester pipeline (`python3 harvester.py --add cryptoaxel`); resolve on-chain wallet address, ... | — | ACTIVE | — | 6.6 | 2026-05-01 |
| SCT-AUTO-1088 | CopyBot | If cryptoaxel qualifies for mirroring, define a Dogecoin-market-specific copy gate scoping CopyBot triggers exclusive... | — | ACTIVE | — | 6.6 | 2026-05-01 |
| SCT-AUTO-1089 | CopyBot | Validate distributional integrity of the 100% accuracy claim — determine whether it reflects a small sample of trades... | — | ACTIVE | — | 6.6 | 2026-05-01 |
| SCT-AUTO-1090 | ALL | Review PaulieB14/polymarket-orderbook-substreams indexing schema for compatibility with py_clob_client stream handler... | — | ACTIVE | — | 6.5 | 2026-05-01 |
| SCT-AUTO-1091 | ALL | Benchmark end-to-end Substreams delivery latency against current REST polling baseline on MON-30 flow spike detection... | — | ACTIVE | — | 6.5 | 2026-05-01 |
| SCT-AUTO-1092 | ALL | Test orderbook skew metrics from the Substreams schema as candidate inputs to INSIDER_SPIKE and STAT_ANOMALY gates; d... | — | ACTIVE | — | 6.5 | 2026-05-01 |
| SCT-AUTO-1093 | CopyBot | Resolve full wallet address from 0xb91 truncation and pull on-chain trade history to confirm $129,844 P&L and trade t... | — | ACTIVE | — | 6.3 | 2026-05-01 |
| SCT-AUTO-1094 | CopyBot | Use the 0xb91 trade profile (single-event spike, magnitude, timing delta vs. public announcement) as a calibration in... | — | ACTIVE | — | 6.3 | 2026-05-01 |
| SCT-AUTO-1095 | CopyBot | Monitor 0xb91 for subsequent trading activity — if post-event behavior reveals a repeatable non-insider pattern with ... | — | ACTIVE | — | 6.3 | 2026-05-01 |
| SCT-AUTO-1096 | CopyBot | Add swisstony (0x204f72f35326db932158cba6adff0b9a1da95e14) to the harvester pipeline (`python3 harvester.py --add 0x2... | — | ACTIVE | — | 6.3 | 2026-05-01 |
| SCT-AUTO-1097 | CopyBot | If category analysis reveals a high-ROI specialist niche, assess whether a category-gated copy trigger (analogous to ... | — | ACTIVE | — | 6.3 | 2026-05-01 |
| SCT-AUTO-1098 | CopyBot | Add 0x1De39f5A4b1313B99fd5278036b2964cC03ca1fA to the harvester pipeline (`python3 harvester.py --add 0x1de39f5a4b131... | — | ACTIVE | — | 6.0 | 2026-05-01 |
| SCT-AUTO-1099 | ALL | Review article methodology and assess whether the harvester's P&L ingestion pipeline needs a realized-vs-unrealized d... | — | REJECTED | — | 5.4 | 2026-05-01 |
| SCT-AUTO-1100 | ALL | Backtest whether accounting-memory bias explains CopyBot false positives on previously onboarded high-leaderboard wal... | — | REJECTED | — | 5.4 | 2026-05-01 |
| SCT-AUTO-1101 | ALL | If decomposition is feasible, add a realized-P&L filter as a pre-condition gate in the harvester qualification logic,... | — | REJECTED | — | 5.4 | 2026-05-01 |
| SCT-AUTO-1102 | CopyBot | Add PDJ88 (0x0e1d01759cfa75782134472a7af5963da9d50c53) to the harvester pipeline (`python3 harvester.py --add 0x0e1d0... | — | ACTIVE | — | 7.8 | 2026-05-02 |
| SCT-AUTO-1103 | CopyBot | Assess whether PDJ88's low absolute volume (~$20K) creates meaningful capacity constraints for mirroring — determine ... | — | ACTIVE | — | 7.8 | 2026-05-02 |
| SCT-AUTO-1104 | CopyBot | Add RisottoPasta (0x49f206f4fb2210a28af4795e4e0b6532cf0bd7b4) to the harvester pipeline (`python3 harvester.py --add ... | — | ACTIVE | — | 6.8 | 2026-05-02 |
| SCT-AUTO-1105 | CopyBot | Identify the wallet behind the MegaETH launch date prediction — cross-reference Polymarket leaderboard and MegaETH la... | — | ACTIVE | — | 6.7 | 2026-05-02 |
| SCT-AUTO-1106 | CopyBot | Once wallet is identified, run `python3 harvester.py --add {wallet}` to confirm on-chain P&L and volume; apply realiz... | — | ACTIVE | — | 6.7 | 2026-05-02 |
| SCT-AUTO-1107 | CopyBot | If validation passes, assess whether a category gate scoped to launch-date and event-timing markets would isolate the... | — | ACTIVE | — | 6.7 | 2026-05-02 |
| SCT-AUTO-1108 | ALL | Audit current order execution paths across PolyArb, CopyBot, WeatherBot, and KrajekBot against the py-builder-relayer... | — | ACTIVE | — | 6.6 | 2026-05-02 |
| SCT-AUTO-1109 | ALL | Run load and failure-mode testing on the relayer API via the client — measure p50/p99 submission latency, characteriz... | — | ACTIVE | — | 6.6 | 2026-05-02 |
| SCT-AUTO-1110 | CopyBot | Archive the full tweet content and any disclosed terminal settings or detection criteria before the claimed 24-hour d... | — | ACTIVE | — | 6.4 | 2026-05-02 |
| SCT-AUTO-1111 | CopyBot | Cross-reference any disclosed detection logic against CopyBot's MON-30 flow spike detection and `check_anomaly()` imp... | — | ACTIVE | — | 6.4 | 2026-05-02 |
| SCT-AUTO-1112 | CopyBot | Attempt independent validation of the claimed 0–5 wallets/day hit rate by running the harvester across recent high-fl... | — | ACTIVE | — | 6.4 | 2026-05-02 |
| SCT-AUTO-1113 | NewsBot | Survey active Polymarket markets with oil/energy exposure (conflict, OPEC, sanctions, macro) and assess whether WTI f... | — | ACTIVE | — | 5.9 | 2026-05-02 |
| SCT-AUTO-1114 | NewsBot | If SCT-AUTO-1113 surfaces viable categories, implement an 'oil exposure' tag in NewsBot's event classification pipeli... | — | ACTIVE | — | 5.9 | 2026-05-02 |
| SCT-AUTO-1115 | ALL | Review MemePit's viral-attention signal logic and assess whether Kalshi meme-market volume/velocity patterns are obse... | — | ACTIVE | — | 5.6 | 2026-05-02 |
| SCT-AUTO-1116 | CopyBot | Run `python3 harvester.py --add 0x492442eab586f242b53bda933fd5de859c8a3782` to pull on-chain P&L and volume; apply re... | — | ACTIVE | — | 5.6 | 2026-05-02 |
| SCT-AUTO-1117 | CopyBot | If SCT-AUTO-1116 validates the edge, profile market category distribution across this wallet's trade history to deter... | — | ACTIVE | — | 5.6 | 2026-05-02 |
| SCT-AUTO-1118 | CopyBot | Run `python3 harvester.py --add 0x986b16df5791e7dc4c6f6ef3524eff0efe6812e9` to pull on-chain P&L and volume; apply re... | — | ACTIVE | — | 5.6 | 2026-05-02 |
| SCT-AUTO-1119 | ALL | Review rs-clob-client-v2 source and open issues for any undocumented CLOB API behaviors, rate limit details, or order... | — | ACTIVE | — | 5.4 | 2026-05-02 |
| SCT-AUTO-1120 | CopyBot | Run `python3 harvester.py --add 0xdc40c985063d50aa00cc5f637f7e83203b3b89cf` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.2 | 2026-05-02 |
| SCT-AUTO-1121 | CopyBot | Run `python3 harvester.py --add 0x47b3badd653cc86c4d24a66712e58818b235039c` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.2 | 2026-05-02 |
| SCT-AUTO-1122 | CopyBot | Run `python3 harvester.py --add 0xfdc0bd67fbd71aa8edd00121eb2a7fcbddc34b85` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.2 | 2026-05-02 |
| SCT-AUTO-1123 | CopyBot | Run `python3 harvester.py --add 0x37d366a769f58ef478f72c6b1bf730424b2b709a` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1124 | CopyBot | Run `python3 harvester.py --add 0x27abdfc9393c72a6330a3be987da4b46c726e521` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1125 | CopyBot | Run `python3 harvester.py --add 0x23073ad0c9dff45353cedb11760570b995663934` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1126 | CopyBot | Run `python3 harvester.py --add 0xa113d92c8bdd65c4f7e47a086a24ae3b125c284e` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1127 | CopyBot | Run `python3 harvester.py --add 0x64e93f87d8a0c1b2cde6f20d71f211372a95eb4c` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1128 | CopyBot | Run `python3 harvester.py --add 0xc0ff6a9ac424210cf218fda5c5753324c34a9953` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 5.0 | 2026-05-02 |
| SCT-AUTO-1129 | CopyBot | Run `python3 harvester.py --add 0x45c9c799e0e6ddf19c50e9dac5ab5a925f9b414b` to pull on-chain P&L; apply realized-vs-u... | — | ACTIVE | — | 6.7 | 2026-05-03 |
| SCT-AUTO-1130 | ALL | Review Mamba attention mechanism applicability to ScoutBot multi-signal aggregation (news event → confidence scoring)... | — | ACTIVE | — | 6.1 | 2026-05-03 |
| SCT-AUTO-1131 | ALL | Assess whether SAMBA's graph component can model correlated Polymarket markets (e.g., related political events) to im... | — | ACTIVE | — | 6.1 | 2026-05-03 |
| SCT-AUTO-1132 | CopyBot | Analyze entry/exit trigger logic for news-signal latency and slippage against CopyBot's MIN_PRICE/MAX_PRICE gate; det... | — | ACTIVE | — | 5.5 | 2026-05-03 |
| SCT-AUTO-1133 | CopyBot | Cross-reference Alpaca execution fee model with Polymarket/Kalshi fee structures; confirm whether SX odds checker equ... | — | ACTIVE | — | 5.5 | 2026-05-03 |
| SCT-AUTO-1134 | CopyBot | Evaluate copy-trade confidence threshold implementation against CopyBot MON-36 contradiction guard; flag any threshol... | — | ACTIVE | — | 5.5 | 2026-05-03 |
| SCT-AUTO-1135 | ALL | Evaluate arbitrage scanner patterns for structural reuse in PolyArb's oracle-lag detection or CopyBot's SX Bet cross-... | — | ACTIVE | — | 5.2 | 2026-05-03 |
| SCT-AUTO-1136 | ALL | Audit Limitless Exchange API against py_clob_client interface; determine whether a thin adapter layer is sufficient f... | — | ACTIVE | — | 5.2 | 2026-05-03 |
| SCT-AUTO-1137 | CopyBot | Run `python3 harvester.py --add 0xf6f687d9c728a4fc5590d71e2e53b9d418e20e74` to establish on-chain baseline; defer ROI... | — | ACTIVE | — | 5.1 | 2026-05-03 |

## Schema (preserved from AllBots_TODO_CURRENT.md format)

**Status values:** ACTIVE (default on scout write), DONE (triage flipped to completed), REJECTED (triage flipped to rejected with rationale recorded in the Rejected section below).

`| ID | Bot | Task | Priority | Status | Gate | Score | Added |`

Status starts as ACTIVE. Priority typically blank for ScoutBot-promoted (set by triage).

---

## Historical SCT-AUTO IDs (already triaged before 2026-04-24 cutover — DO NOT re-promote)

These IDs appeared in the legacy /root/docs/AllBots_TODO_CURRENT.md snapshot at /tmp/AllBots_TODO_CURRENT.md.pre_scoutbot_redirect_20260424 (md5 0e339b7181fb6749e031f776f950ac2e). Listed here so todo_extractor.py dedup will skip them on the first post-cutover run. Do not edit this list.

SCT-AUTO-1 SCT-AUTO-2 SCT-AUTO-3 SCT-AUTO-4 SCT-AUTO-5 SCT-AUTO-6 SCT-AUTO-7 SCT-AUTO-8 SCT-AUTO-9 SCT-AUTO-10
SCT-AUTO-11 SCT-AUTO-12 SCT-AUTO-13 SCT-AUTO-14 SCT-AUTO-15 SCT-AUTO-16 SCT-AUTO-17 SCT-AUTO-18 SCT-AUTO-19 SCT-AUTO-20
SCT-AUTO-21 SCT-AUTO-22 SCT-AUTO-23 SCT-AUTO-24 SCT-AUTO-25 SCT-AUTO-26 SCT-AUTO-27 SCT-AUTO-28 SCT-AUTO-29 SCT-AUTO-30
SCT-AUTO-31 SCT-AUTO-32 SCT-AUTO-33 SCT-AUTO-34 SCT-AUTO-35 SCT-AUTO-36 SCT-AUTO-37 SCT-AUTO-38 SCT-AUTO-39 SCT-AUTO-40
SCT-AUTO-41 SCT-AUTO-42 SCT-AUTO-43 SCT-AUTO-44 SCT-AUTO-45 SCT-AUTO-46 SCT-AUTO-47 SCT-AUTO-48 SCT-AUTO-49 SCT-AUTO-50
SCT-AUTO-51 SCT-AUTO-52 SCT-AUTO-53 SCT-AUTO-54 SCT-AUTO-55 SCT-AUTO-56 SCT-AUTO-57 SCT-AUTO-58 SCT-AUTO-59 SCT-AUTO-60
SCT-AUTO-61 SCT-AUTO-62 SCT-AUTO-63 SCT-AUTO-64 SCT-AUTO-65 SCT-AUTO-66 SCT-AUTO-67 SCT-AUTO-68 SCT-AUTO-69 SCT-AUTO-70
SCT-AUTO-71 SCT-AUTO-72 SCT-AUTO-73 SCT-AUTO-74 SCT-AUTO-75 SCT-AUTO-76 SCT-AUTO-77 SCT-AUTO-78 SCT-AUTO-79 SCT-AUTO-80
SCT-AUTO-81 SCT-AUTO-82 SCT-AUTO-83 SCT-AUTO-84 SCT-AUTO-85 SCT-AUTO-86 SCT-AUTO-87 SCT-AUTO-88 SCT-AUTO-89 SCT-AUTO-90
SCT-AUTO-91 SCT-AUTO-92 SCT-AUTO-93 SCT-AUTO-94 SCT-AUTO-95 SCT-AUTO-96 SCT-AUTO-97 SCT-AUTO-98 SCT-AUTO-99 SCT-AUTO-100
SCT-AUTO-101 SCT-AUTO-102 SCT-AUTO-103 SCT-AUTO-104 SCT-AUTO-105 SCT-AUTO-106 SCT-AUTO-107 SCT-AUTO-108 SCT-AUTO-109 SCT-AUTO-110
SCT-AUTO-111 SCT-AUTO-112 SCT-AUTO-113 SCT-AUTO-114 SCT-AUTO-115 SCT-AUTO-116 SCT-AUTO-117 SCT-AUTO-118 SCT-AUTO-119 SCT-AUTO-120
SCT-AUTO-121 SCT-AUTO-122 SCT-AUTO-123 SCT-AUTO-124 SCT-AUTO-125 SCT-AUTO-126 SCT-AUTO-127 SCT-AUTO-128 SCT-AUTO-129 SCT-AUTO-130
SCT-AUTO-131 SCT-AUTO-132 SCT-AUTO-133 SCT-AUTO-134 SCT-AUTO-135 SCT-AUTO-136 SCT-AUTO-137 SCT-AUTO-138 SCT-AUTO-139 SCT-AUTO-140
SCT-AUTO-141 SCT-AUTO-142 SCT-AUTO-143 SCT-AUTO-144 SCT-AUTO-145 SCT-AUTO-146 SCT-AUTO-147 SCT-AUTO-148 SCT-AUTO-149 SCT-AUTO-150
SCT-AUTO-151 SCT-AUTO-152 SCT-AUTO-153 SCT-AUTO-154 SCT-AUTO-155 SCT-AUTO-156 SCT-AUTO-157 SCT-AUTO-158 SCT-AUTO-159 SCT-AUTO-160
SCT-AUTO-161 SCT-AUTO-162 SCT-AUTO-163 SCT-AUTO-164 SCT-AUTO-165 SCT-AUTO-166 SCT-AUTO-167 SCT-AUTO-168 SCT-AUTO-169 SCT-AUTO-170
SCT-AUTO-171 SCT-AUTO-172 SCT-AUTO-173 SCT-AUTO-174 SCT-AUTO-175 SCT-AUTO-176 SCT-AUTO-177 SCT-AUTO-178 SCT-AUTO-179 SCT-AUTO-180
SCT-AUTO-181 SCT-AUTO-182 SCT-AUTO-183 SCT-AUTO-184 SCT-AUTO-185 SCT-AUTO-186 SCT-AUTO-187 SCT-AUTO-188 SCT-AUTO-189 SCT-AUTO-190
SCT-AUTO-191 SCT-AUTO-192 SCT-AUTO-193 SCT-AUTO-194 SCT-AUTO-195 SCT-AUTO-196 SCT-AUTO-197 SCT-AUTO-198 SCT-AUTO-199 SCT-AUTO-200
SCT-AUTO-201 SCT-AUTO-202 SCT-AUTO-203 SCT-AUTO-204 SCT-AUTO-205 SCT-AUTO-206 SCT-AUTO-207 SCT-AUTO-208 SCT-AUTO-209 SCT-AUTO-210
SCT-AUTO-211 SCT-AUTO-212 SCT-AUTO-213 SCT-AUTO-214 SCT-AUTO-215 SCT-AUTO-216 SCT-AUTO-217 SCT-AUTO-218 SCT-AUTO-219 SCT-AUTO-220
SCT-AUTO-221 SCT-AUTO-222 SCT-AUTO-223 SCT-AUTO-224 SCT-AUTO-225 SCT-AUTO-226 SCT-AUTO-227 SCT-AUTO-228 SCT-AUTO-229 SCT-AUTO-230
SCT-AUTO-231 SCT-AUTO-232 SCT-AUTO-233 SCT-AUTO-234 SCT-AUTO-235 SCT-AUTO-236 SCT-AUTO-237 SCT-AUTO-238 SCT-AUTO-239 SCT-AUTO-240
SCT-AUTO-241 SCT-AUTO-242 SCT-AUTO-243 SCT-AUTO-244 SCT-AUTO-245 SCT-AUTO-246 SCT-AUTO-247 SCT-AUTO-248 SCT-AUTO-249 SCT-AUTO-250
SCT-AUTO-251 SCT-AUTO-252 SCT-AUTO-253 SCT-AUTO-254 SCT-AUTO-255 SCT-AUTO-256 SCT-AUTO-257 SCT-AUTO-258 SCT-AUTO-259 SCT-AUTO-260
SCT-AUTO-261 SCT-AUTO-262 SCT-AUTO-263 SCT-AUTO-264 SCT-AUTO-265 SCT-AUTO-266 SCT-AUTO-267 SCT-AUTO-268 SCT-AUTO-269 SCT-AUTO-270
SCT-AUTO-271 SCT-AUTO-272 SCT-AUTO-273 SCT-AUTO-274 SCT-AUTO-275 SCT-AUTO-276 SCT-AUTO-277 SCT-AUTO-278 SCT-AUTO-279 SCT-AUTO-280
SCT-AUTO-281 SCT-AUTO-282 SCT-AUTO-283 SCT-AUTO-284 SCT-AUTO-285 SCT-AUTO-286 SCT-AUTO-287 SCT-AUTO-288 SCT-AUTO-289 SCT-AUTO-290
SCT-AUTO-291 SCT-AUTO-292 SCT-AUTO-293 SCT-AUTO-294 SCT-AUTO-295 SCT-AUTO-296 SCT-AUTO-297 SCT-AUTO-298 SCT-AUTO-299 SCT-AUTO-300
SCT-AUTO-301 SCT-AUTO-302 SCT-AUTO-303 SCT-AUTO-304 SCT-AUTO-305 SCT-AUTO-306 SCT-AUTO-307 SCT-AUTO-308 SCT-AUTO-309 SCT-AUTO-310
SCT-AUTO-311 SCT-AUTO-312 SCT-AUTO-313 SCT-AUTO-314 SCT-AUTO-315 SCT-AUTO-316 SCT-AUTO-317 SCT-AUTO-318 SCT-AUTO-319 SCT-AUTO-320
SCT-AUTO-321 SCT-AUTO-322 SCT-AUTO-323 SCT-AUTO-324 SCT-AUTO-325 SCT-AUTO-326 SCT-AUTO-327 SCT-AUTO-328 SCT-AUTO-329 SCT-AUTO-330
SCT-AUTO-331 SCT-AUTO-332 SCT-AUTO-333 SCT-AUTO-334 SCT-AUTO-335 SCT-AUTO-336 SCT-AUTO-337 SCT-AUTO-338 SCT-AUTO-339 SCT-AUTO-340
SCT-AUTO-341 SCT-AUTO-342 SCT-AUTO-343 SCT-AUTO-344 SCT-AUTO-345 SCT-AUTO-346 SCT-AUTO-347 SCT-AUTO-348 SCT-AUTO-349 SCT-AUTO-350
SCT-AUTO-351 SCT-AUTO-352 SCT-AUTO-353 SCT-AUTO-354 SCT-AUTO-355 SCT-AUTO-356 SCT-AUTO-357 SCT-AUTO-358 SCT-AUTO-359 SCT-AUTO-360
SCT-AUTO-361 SCT-AUTO-362 SCT-AUTO-363 SCT-AUTO-364 SCT-AUTO-365 SCT-AUTO-366 SCT-AUTO-367 SCT-AUTO-368 SCT-AUTO-369 SCT-AUTO-370
SCT-AUTO-371 SCT-AUTO-372 SCT-AUTO-373 SCT-AUTO-374 SCT-AUTO-375 SCT-AUTO-376 SCT-AUTO-377 SCT-AUTO-378 SCT-AUTO-379 SCT-AUTO-380
SCT-AUTO-381 SCT-AUTO-382 SCT-AUTO-383 SCT-AUTO-384 SCT-AUTO-385 SCT-AUTO-386 SCT-AUTO-387 SCT-AUTO-388 SCT-AUTO-389 SCT-AUTO-390
SCT-AUTO-391 SCT-AUTO-392 SCT-AUTO-393 SCT-AUTO-394 SCT-AUTO-395 SCT-AUTO-396 SCT-AUTO-397 SCT-AUTO-398 SCT-AUTO-399 SCT-AUTO-400
SCT-AUTO-401 SCT-AUTO-402 SCT-AUTO-403 SCT-AUTO-404 SCT-AUTO-405 SCT-AUTO-406 SCT-AUTO-407 SCT-AUTO-408 SCT-AUTO-409 SCT-AUTO-410
SCT-AUTO-411 SCT-AUTO-412 SCT-AUTO-413 SCT-AUTO-414 SCT-AUTO-415 SCT-AUTO-416 SCT-AUTO-417 SCT-AUTO-418 SCT-AUTO-419 SCT-AUTO-420
SCT-AUTO-421 SCT-AUTO-422 SCT-AUTO-423 SCT-AUTO-424 SCT-AUTO-425 SCT-AUTO-426 SCT-AUTO-427 SCT-AUTO-428 SCT-AUTO-429 SCT-AUTO-430
SCT-AUTO-431 SCT-AUTO-432 SCT-AUTO-433 SCT-AUTO-434 SCT-AUTO-435 SCT-AUTO-436 SCT-AUTO-437 SCT-AUTO-438 SCT-AUTO-439 SCT-AUTO-440
SCT-AUTO-441 SCT-AUTO-442 SCT-AUTO-443 SCT-AUTO-444 SCT-AUTO-445 SCT-AUTO-446 SCT-AUTO-447 SCT-AUTO-448 SCT-AUTO-449 SCT-AUTO-450
SCT-AUTO-451 SCT-AUTO-452 SCT-AUTO-453 SCT-AUTO-454 SCT-AUTO-455 SCT-AUTO-456 SCT-AUTO-457 SCT-AUTO-458 SCT-AUTO-459 SCT-AUTO-460
SCT-AUTO-461 SCT-AUTO-462 SCT-AUTO-463 SCT-AUTO-464 SCT-AUTO-465 SCT-AUTO-466 SCT-AUTO-467 SCT-AUTO-468 SCT-AUTO-469 SCT-AUTO-470
SCT-AUTO-471 SCT-AUTO-472 SCT-AUTO-473 SCT-AUTO-474 SCT-AUTO-475 SCT-AUTO-476 SCT-AUTO-477 SCT-AUTO-478 SCT-AUTO-479 SCT-AUTO-480
SCT-AUTO-481 SCT-AUTO-482 SCT-AUTO-483 SCT-AUTO-484 SCT-AUTO-485 SCT-AUTO-486 SCT-AUTO-487 SCT-AUTO-488 SCT-AUTO-489 SCT-AUTO-490
SCT-AUTO-491 SCT-AUTO-492 SCT-AUTO-493 SCT-AUTO-494 SCT-AUTO-495 SCT-AUTO-496 SCT-AUTO-497 SCT-AUTO-498 SCT-AUTO-499 SCT-AUTO-500
SCT-AUTO-501 SCT-AUTO-502 SCT-AUTO-503 SCT-AUTO-504 SCT-AUTO-505 SCT-AUTO-506 SCT-AUTO-507 SCT-AUTO-508 SCT-AUTO-509 SCT-AUTO-510
SCT-AUTO-511 SCT-AUTO-512 SCT-AUTO-513 SCT-AUTO-514 SCT-AUTO-515 SCT-AUTO-516 SCT-AUTO-517 SCT-AUTO-518 SCT-AUTO-519 SCT-AUTO-520
SCT-AUTO-521 SCT-AUTO-522 SCT-AUTO-523 SCT-AUTO-524 SCT-AUTO-525 SCT-AUTO-526 SCT-AUTO-527 SCT-AUTO-528 SCT-AUTO-529 SCT-AUTO-530
SCT-AUTO-531 SCT-AUTO-532 SCT-AUTO-533 SCT-AUTO-534 SCT-AUTO-535 SCT-AUTO-536 SCT-AUTO-537 SCT-AUTO-538 SCT-AUTO-539 SCT-AUTO-540
SCT-AUTO-541 SCT-AUTO-542 SCT-AUTO-543 SCT-AUTO-544 SCT-AUTO-545 SCT-AUTO-546 SCT-AUTO-547 SCT-AUTO-548 SCT-AUTO-549 SCT-AUTO-550
SCT-AUTO-551 SCT-AUTO-552 SCT-AUTO-553 SCT-AUTO-554 SCT-AUTO-555 SCT-AUTO-556 SCT-AUTO-557 SCT-AUTO-558 SCT-AUTO-559 SCT-AUTO-560
SCT-AUTO-561 SCT-AUTO-562 SCT-AUTO-563 SCT-AUTO-564 SCT-AUTO-565 SCT-AUTO-566 SCT-AUTO-567 SCT-AUTO-568 SCT-AUTO-569 SCT-AUTO-570
SCT-AUTO-571 SCT-AUTO-572 SCT-AUTO-573 SCT-AUTO-574 SCT-AUTO-575 SCT-AUTO-576 SCT-AUTO-577 SCT-AUTO-578 SCT-AUTO-579 SCT-AUTO-580
SCT-AUTO-581 SCT-AUTO-582 SCT-AUTO-583 SCT-AUTO-584 SCT-AUTO-585 SCT-AUTO-586 SCT-AUTO-587 SCT-AUTO-588 SCT-AUTO-589 SCT-AUTO-590
SCT-AUTO-591 SCT-AUTO-592 SCT-AUTO-593 SCT-AUTO-594 SCT-AUTO-595 SCT-AUTO-596 SCT-AUTO-597 SCT-AUTO-598 SCT-AUTO-599 SCT-AUTO-600
SCT-AUTO-601 SCT-AUTO-602 SCT-AUTO-603 SCT-AUTO-604 SCT-AUTO-605 SCT-AUTO-606 SCT-AUTO-607 SCT-AUTO-608 SCT-AUTO-609 SCT-AUTO-610
SCT-AUTO-611 SCT-AUTO-612 SCT-AUTO-613 SCT-AUTO-614 SCT-AUTO-615 SCT-AUTO-616 SCT-AUTO-617 SCT-AUTO-618 SCT-AUTO-619 SCT-AUTO-620
SCT-AUTO-621 SCT-AUTO-622 SCT-AUTO-623 SCT-AUTO-624 SCT-AUTO-625 SCT-AUTO-626 SCT-AUTO-627 SCT-AUTO-628 SCT-AUTO-629 SCT-AUTO-630
SCT-AUTO-631 SCT-AUTO-632 SCT-AUTO-633 SCT-AUTO-634 SCT-AUTO-635 SCT-AUTO-636 SCT-AUTO-637 SCT-AUTO-638 SCT-AUTO-639 SCT-AUTO-640
SCT-AUTO-641 SCT-AUTO-642 SCT-AUTO-643 SCT-AUTO-644 SCT-AUTO-645 SCT-AUTO-646 SCT-AUTO-647 SCT-AUTO-648 SCT-AUTO-649 SCT-AUTO-650
SCT-AUTO-651 SCT-AUTO-652 SCT-AUTO-653 SCT-AUTO-654 SCT-AUTO-655 SCT-AUTO-656 SCT-AUTO-657 SCT-AUTO-658 SCT-AUTO-659 SCT-AUTO-660
SCT-AUTO-661 SCT-AUTO-662 SCT-AUTO-663 SCT-AUTO-664 SCT-AUTO-665 SCT-AUTO-666 SCT-AUTO-667 SCT-AUTO-668 SCT-AUTO-669 SCT-AUTO-670
SCT-AUTO-671 SCT-AUTO-672 SCT-AUTO-673 SCT-AUTO-674 SCT-AUTO-675 SCT-AUTO-676 SCT-AUTO-677 SCT-AUTO-678 SCT-AUTO-679 SCT-AUTO-680
SCT-AUTO-681 SCT-AUTO-682 SCT-AUTO-683 SCT-AUTO-684 SCT-AUTO-685 SCT-AUTO-686 SCT-AUTO-687 SCT-AUTO-688 SCT-AUTO-689 SCT-AUTO-690
SCT-AUTO-691 SCT-AUTO-692 SCT-AUTO-693 SCT-AUTO-694 SCT-AUTO-695 SCT-AUTO-696 SCT-AUTO-697 SCT-AUTO-698 SCT-AUTO-699 SCT-AUTO-700
SCT-AUTO-701 SCT-AUTO-702 SCT-AUTO-703 SCT-AUTO-704 SCT-AUTO-705 SCT-AUTO-706 SCT-AUTO-707 SCT-AUTO-708 SCT-AUTO-709 SCT-AUTO-710
SCT-AUTO-711 SCT-AUTO-712 SCT-AUTO-713 SCT-AUTO-714 SCT-AUTO-715 SCT-AUTO-716 SCT-AUTO-717 SCT-AUTO-718 SCT-AUTO-719 SCT-AUTO-720
SCT-AUTO-721 SCT-AUTO-722 SCT-AUTO-723 SCT-AUTO-724 SCT-AUTO-725 SCT-AUTO-726 SCT-AUTO-727 SCT-AUTO-728 SCT-AUTO-729 SCT-AUTO-730
SCT-AUTO-731 SCT-AUTO-732 SCT-AUTO-733 SCT-AUTO-734 SCT-AUTO-735 SCT-AUTO-736 SCT-AUTO-737 SCT-AUTO-738 SCT-AUTO-739 SCT-AUTO-740
SCT-AUTO-741 SCT-AUTO-742 SCT-AUTO-743 SCT-AUTO-744 SCT-AUTO-745 SCT-AUTO-746 SCT-AUTO-747 SCT-AUTO-748 SCT-AUTO-749 SCT-AUTO-750
SCT-AUTO-751 SCT-AUTO-752 SCT-AUTO-753 SCT-AUTO-754 SCT-AUTO-755 SCT-AUTO-756 SCT-AUTO-757 SCT-AUTO-758 SCT-AUTO-759 SCT-AUTO-760
SCT-AUTO-761 SCT-AUTO-762 SCT-AUTO-763 SCT-AUTO-764 SCT-AUTO-765 SCT-AUTO-766 SCT-AUTO-767 SCT-AUTO-768 SCT-AUTO-769 SCT-AUTO-770
SCT-AUTO-771 SCT-AUTO-772 SCT-AUTO-773 SCT-AUTO-774 SCT-AUTO-775 SCT-AUTO-776 SCT-AUTO-777 SCT-AUTO-778 SCT-AUTO-779 SCT-AUTO-780
SCT-AUTO-781 SCT-AUTO-782 SCT-AUTO-783 SCT-AUTO-784 SCT-AUTO-785 SCT-AUTO-786 SCT-AUTO-787 SCT-AUTO-788 SCT-AUTO-789 SCT-AUTO-790
SCT-AUTO-791 SCT-AUTO-792 SCT-AUTO-793 SCT-AUTO-794 SCT-AUTO-795 SCT-AUTO-796 SCT-AUTO-797 SCT-AUTO-798 SCT-AUTO-799 SCT-AUTO-800
SCT-AUTO-801 SCT-AUTO-802 SCT-AUTO-803 SCT-AUTO-804 SCT-AUTO-805 SCT-AUTO-806 SCT-AUTO-807 SCT-AUTO-808 SCT-AUTO-809 SCT-AUTO-810
SCT-AUTO-811 SCT-AUTO-812 SCT-AUTO-813 SCT-AUTO-814 SCT-AUTO-815 SCT-AUTO-816 SCT-AUTO-817 SCT-AUTO-818 SCT-AUTO-819 SCT-AUTO-820
SCT-AUTO-821 SCT-AUTO-822 SCT-AUTO-823 SCT-AUTO-824 SCT-AUTO-825 SCT-AUTO-826 SCT-AUTO-827 SCT-AUTO-828 SCT-AUTO-829 SCT-AUTO-830
SCT-AUTO-831 SCT-AUTO-832 SCT-AUTO-833 SCT-AUTO-834 SCT-AUTO-835 SCT-AUTO-836 SCT-AUTO-837 SCT-AUTO-838 SCT-AUTO-839 SCT-AUTO-840
SCT-AUTO-841 SCT-AUTO-842 SCT-AUTO-843 SCT-AUTO-844 SCT-AUTO-845 SCT-AUTO-846 SCT-AUTO-847 SCT-AUTO-848 SCT-AUTO-849 SCT-AUTO-850
SCT-AUTO-851 SCT-AUTO-852 SCT-AUTO-853 SCT-AUTO-854 SCT-AUTO-855 SCT-AUTO-856 SCT-AUTO-857 SCT-AUTO-858 SCT-AUTO-859 SCT-AUTO-860
SCT-AUTO-861 SCT-AUTO-862 SCT-AUTO-863 SCT-AUTO-864 SCT-AUTO-865 SCT-AUTO-866 SCT-AUTO-867 SCT-AUTO-868 SCT-AUTO-869 SCT-AUTO-870
SCT-AUTO-871 SCT-AUTO-872 SCT-AUTO-873 SCT-AUTO-874 SCT-AUTO-875 SCT-AUTO-876 SCT-AUTO-877 SCT-AUTO-878 SCT-AUTO-879 SCT-AUTO-880
SCT-AUTO-881 SCT-AUTO-882 SCT-AUTO-883 SCT-AUTO-884 SCT-AUTO-885 SCT-AUTO-886 SCT-AUTO-887 SCT-AUTO-888 SCT-AUTO-889 SCT-AUTO-890
SCT-AUTO-891 SCT-AUTO-892 SCT-AUTO-893 SCT-AUTO-894 SCT-AUTO-895 SCT-AUTO-896 SCT-AUTO-897 SCT-AUTO-898 SCT-AUTO-899 SCT-AUTO-900
SCT-AUTO-901 SCT-AUTO-902 SCT-AUTO-903 SCT-AUTO-904 SCT-AUTO-905 SCT-AUTO-906 SCT-AUTO-907 SCT-AUTO-908 SCT-AUTO-909 SCT-AUTO-910
SCT-AUTO-911 SCT-AUTO-912 SCT-AUTO-913 SCT-AUTO-914 SCT-AUTO-915 SCT-AUTO-916 SCT-AUTO-917 SCT-AUTO-918 SCT-AUTO-919 SCT-AUTO-920
SCT-AUTO-921 SCT-AUTO-922 SCT-AUTO-923 SCT-AUTO-924 SCT-AUTO-925 SCT-AUTO-926 SCT-AUTO-927 SCT-AUTO-928 SCT-AUTO-929 SCT-AUTO-930
SCT-AUTO-931 SCT-AUTO-932 SCT-AUTO-933 SCT-AUTO-934 

---

## Pending items

(none yet — first ScoutBot run after 2026-04-24 cutover will append here)

## 2026-04-28 — SCT-AUTO-1000 — drift triage: /opt/copybot working tree
- 6 data/* files modified (operational state — likely benign, verify)
- fix_watchlist.py deleted (tracked file removal, intent unconfirmed)
- 30+ untracked: .bak.*, tier3 snapshots, audits/, backups/, monitors/, logs/
- Surfaced during Phase 3 attribution commit 2026-04-28 ~05:55 UTC (74d4d8f in /opt/copybot)
- Action: dedicated triage session — git diff data/, confirm fix_watchlist.py deletion intentional, decide on untracked dir fates
- Priority: low (does not block Phase 3); medium if working tree drift accumulates further

## 2026-04-28 — SCT-AUTO-1001 — session-template path bug: /opt/scoutbot/SCOUTBOT_INBOX.md does not exist
- Every session block's Task 0 currently runs: `grep "$(date '+%Y-%m-%d')" /opt/scoutbot/SCOUTBOT_INBOX.md || echo "no SCT-AUTO items today"`
- That path does not exist; the real inbox is at /root/.agent/memory/working/SCOUTBOT_INBOX.md
- Likely moved during INFRA-RETIRE-CLEANUP 2026-04-24 (per WORKSPACE: "ScoutBot redirected to write into new SCOUTBOT_INBOX.md")
- The `||` fallback swallows the missing-file error so the morning check has been silently no-op'ing
- Surfaced 2026-04-28 06:07 UTC when a same-template Task 6 `cat >>` accidentally created /opt/scoutbot/SCOUTBOT_INBOX.md as a fresh file with SCT-AUTO-0001
- Action: update session_start_paste.md and any chat-template snippets to point at /root/.agent/memory/working/SCOUTBOT_INBOX.md
- Priority: medium (silent failure in routine check, masks legitimate inbox items)

## 2026-04-28 — SCT-AUTO-1030 — bulk review marker: 2026-04-24 to 2026-04-28 backlog
- 95 table-format items (Pass B) + 2 section-format items (Pass A) skimmed in dual-format pass on 2026-04-28 ~06:18 UTC
  - Per-date table counts: 04-25=33, 04-26=19, 04-27=22, 04-28=21
  - Section-format: my own SCT-AUTO-1000 (drift triage) and 1001 (template path bug)
- Bulk-skim filters surfaced no operationally critical items — bulk is wallet-add research leads, repo-audit comparisons, and pattern-extraction tasks. CopyBot/ALL split visible in routing column.
- Items NOT individually promoted: parked in place, considered reviewed; future sessions should treat 04-24 to 04-28 as already-triaged and only act on items dated 04-29 onwards.
- Items requiring follow-up if reactivated: sort by score descending and pick from top.
- Root cause of blind window: SCT-AUTO-1001 (broken Task 0 grep path); fixed 2026-04-28 via Standing pre-tasks section in /root/.agent/session_start_paste.md.

## Rejected

### SCT-AUTO-1099 — REJECTED 2026-05-01
**Original task:** Review article methodology and assess whether the harvester's P&L ingestion pipeline needs a realized-vs-unrealized decomposition.
**Rationale:** Cited article ("Investment Memory: Why Most Portfolio Systems Lie to You", quantjourney.substack.com) does not bridge to CopyBot's domain. Article addresses institutional Portfolio Management Systems — IBOR vs ABOR, bitemporal storage, the pro-forma-backtest trap. It does not address third-party wallet P&L, realized-vs-unrealized as a scoring filter, or anything mappable to copy-trading false positives. The realized-vs-unrealized decomposition direction itself has independent merit via CB-METRICS-BLINDSPOT-OPEN-BOOK-WALLETS (WORKSPACE 2026-04-30 evening), but is not justified by this source.
**Lesson:** scout-summary-overreach (LESSONS commit `e7cebc8`)

### SCT-AUTO-1100 — REJECTED 2026-05-01
**Original task:** Backtest whether accounting-memory bias explains CopyBot false positives on previously onboarded high-leaderboard wallets.
**Rationale:** "Accounting-memory bias" is a framing the scout invented; the cited article does not use the term and does not define a phenomenon mappable to copy-trading false positives. False positives on high-leaderboard wallets are explained by hold-to-resolution patterns hiding MTM losses (the 0xaa075924e1 martingale-class case), a mechanism unrelated to the article's pro-forma-backtest critique. Backtesting the wrong-named hypothesis would not produce useful signal.
**Lesson:** scout-summary-overreach (LESSONS commit `e7cebc8`)

### SCT-AUTO-1101 — REJECTED 2026-05-01
**Original task:** Add a realized-P&L filter as a pre-condition gate in the harvester qualification logic.
**Rationale:** The proposal itself has merit and pairs with CB-METRICS-BLINDSPOT-OPEN-BOOK-WALLETS's downstream gate (upstream qualification + downstream scoring as complementary defenses). Rejected here only because the citation chain (this article justifies this filter) is not valid. If the qualification-gate filter is implemented, it should be tracked under CB-METRICS-BLINDSPOT-OPEN-BOOK-WALLETS in WORKSPACE, not as a derivative of this scout entry.
**Lesson:** scout-summary-overreach (LESSONS commit `e7cebc8`)
