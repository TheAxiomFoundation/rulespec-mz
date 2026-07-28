# rulespec-mz

Mozambique RuleSpec source registry.

This repository targets the Mozambican tax-benefit surface simulated by MOZMOD (the SOUTHMOD tax-benefit microsimulation model for Mozambique, UNU-WIDER; v3.1, policy years 2015–24): personal income tax IRPS under the Código do IRPS (Lei 33/2007, tranche-2 capture) as revised by Lei 20/2013 (captured; incl. Art. 65-A), the ISPC simplified small-taxpayer tax created by Lei 5/2009 (annual 75.000,00MT or 3 percent of turnover), VAT under the Código do IVA (Lei 32/2007, tranche-2) as revised by Lei 3/2012 (17 percent standard through 2022, 16 percent from 2023 — the 2023 instrument is a tranche-2 capture), excise duty ICE (Lei 17/2009, tranche-2) and the fuel tax, INSS social insurance contributions under Lei 4/2007 (7 percent of gross income: 4 percent employer, 3 percent employee; self-employed 7 percent), and the PSSB basic social subsidy with INAS direct social support (Decreto 85/2009 and INAS program instruments, tranche-2).

All encoded law lives under a single `mz/` namespace. The validation frame is MOZMOD v3.1 (report CR-MOZMOD-v3.1, §§1–2 and Tables 2.4–2.5).

## Source Priority

Policy must come from the furthest upstream available source: Boletim da República texts (Imprensa Nacional) and official consolidations first, Autoridade Tributária code prints and INSS/INAS/MEF instruments next, agency guidance only after the governing instrument is identified. Record the host in manifest metadata. Two at.gov.mz capture hazards are documented in the corpus manifest: the server negotiates TLS DH parameters below modern OpenSSL minimums (fetch out-of-band from the recorded source_url and hand bytes to the extractor via manifest `local_path`; never disable verification), and the AT media entry named "Lei-n1-05-2009" serves the wrong bytes (Decreto 21/2002, the Código do IRPC) — the ISPC law is captured from the AT-hosted Boletim da República 2009-01-12 I Série 3.º Suplemento gazette instead.

## Corpus binding

`.axiom/toolchain.toml` pins the immutable signed corpus release this repository consumes (`mz-rulespec-2026-07-23`). The shared validate workflow verifies the release object signature, content hash, and waiver-set hash on every push.

## Layout

- `mz/statutes/`, `mz/regulations/`, `mz/policies/`: encoded RuleSpec modules with mandatory companion `.test.yaml` files.
- `programs/`: declarative compose specs (one per jurisdiction/program/period).
- `data/oracles/`, `data/coverage/`: comparison-oracle references and the MOZMOD instrument map. Never legal authority.

## Parity program

Tracked on issue #1: tranche-2 captures (Lei 33/2007 Código do IRPS; Lei 32/2007 Código do IVA; Lei 17/2009 ICE; the 2023 VAT-rate instrument; Decreto 85/2009 PSSB regulation; INAS program instruments) and MOZMOD parity tests per instrument.

## Listing gates

This repo carries `app_visibility = "experimental"` in `.axiom/registry.toml` and stays out of app surfaces until:

1. The encoded surface covers the flagship calculation (IRPS gross-to-net for a formal employee) end to end with companion tests.
2. Oracle parity suites exist and pass against MOZMOD for the encoded surface.
3. Citation paths are stable (lei/decreto-number form against the Boletim da República prints).
