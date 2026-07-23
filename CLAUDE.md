# rulespec-mz Agent Notes

This repo stores Mozambique RuleSpec source registry materials, oracle references, and encoded policy rules. All encoded law lives under a single `mz/` namespace.

## Scope

- `mz/statutes/`: Mozambican laws — Lei 4/2007 (Lei da Protecção Social), Lei 20/2013 (IRPS revision), Lei 5/2009 (ISPC), Lei 3/2012 (IVA revision), and other primary law needed for tax-benefit modeling.
- `mz/regulations/`: decretos and regulamentos (Decreto 85/2009 PSSB regulation, INSS contribution regulations, ICE tables) made under the laws.
- `mz/policies/`: social-protection programme rules set administratively (PSSB operational rules, INAS direct social support).
- `programs/`: declarative compose specs (one per jurisdiction/program/period).
- `data/coverage/`, `data/oracles/`: coverage backlog and comparison references. These are never legal authority.

## Do

- Start from the furthest upstream source: Boletim da República texts (Imprensa Nacional) first, Autoridade Tributária code prints and INSS/INAS/MEF instruments next, agency guidance last — record the host in manifest metadata.
- Respect the two at.gov.mz capture hazards (see README): weak-DH TLS (use manifest `local_path` with recorded source_url + sha256; never disable verification) and the mislabeled "Lei-n1-05-2009" media entry (serves the IRPC code; the ISPC law lives in the BR 2009-01-12 I Série 3.º Suplemento gazette, sliced via `page_windows`).
- Add RuleSpec under `mz/statutes/`, `mz/regulations/`, or `mz/policies/` with companion `.test.yaml` files.
- Cite corpus paths from modules via `module.source_verification.corpus_citation_path` (or `corpus_citation_paths`).
- Use the MOZMOD v3.1 policy window (2015–24) as the validation frame: INSS 7% (4 er / 3 ee; self-employed 7%); IRPS schedules 1 and 3; ISPC 75.000,00MT or 3% of turnover; VAT 17% through 2022, 16% from 2023. Indexed/annual values must be corpus-grounded, never invented.
- Keep exact oracle versions in `data/oracles/oracle-index.json`. The SOUTHMOD bundle is licensed and non-redistributable — never commit bundle bytes, dataset rows, or model XML.
- Sync `axiom-encode` and `.axiom/toolchain.toml` before substantial encoding runs.

## Do Not

- Use AT calculators or third-party tax alerts as the first legal source when a law or instrument governs the rule.
- Invent, round, or interpolate any Mozambican monetary amount, rate band, or threshold. Every number must come verbatim from a captured official provision.
- Migrate MOZMOD, EUROMOD/SOUTHMOD, or agency calculator code mechanically as RuleSpec.
- Add generated source payload dumps, formula artifacts, `parameters.yaml`, or standalone YAML fixtures outside allowed RuleSpec roots.
- Hand-copy statute text into RuleSpec without a corpus `citation_path`.
