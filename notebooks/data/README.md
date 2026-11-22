# Data for the notebooks

Small data files the notebooks read. On your own machine they are read from this folder; in Colab each notebook
downloads the file it needs once from this folder on GitHub.

| File | What it is | Used in |
| --- | --- | --- |
| `riscbac_en_first10.jsonl` | ten synthetic car-insurance contracts from the RISCBAC corpus | `02_structured_outputs` |
| `mortality_tables.sqlite` | the German private-health mortality tables PKV-Sterbetafel 2022 to 2025, in SQLite | `03_function_calling`, `06_agentic_ai` |
| `migration/` | a legacy R chain-ladder script, its triangle, hidden test triangles and R's results | `06_agentic_ai`, section 4 |

## `riscbac_en_first10.jsonl`

10 lines, 1,630,859 bytes, UTF-8, LF line endings,
sha256 `a2d76ac5884f66bd65740c5c906fff4c5ac82b37bcdfd037b056a20d7c1607a7`.

**What it contains.** One JSON object per line with a single field, `"text"`: the full text of one synthetic
automobile insurance contract, 157,208 to 160,536 characters, with pages separated by `<###NEW_PAGE###>`. Each
contract is issued on the Quebec owner's form (`Q.P.F. B 1 - OWNER'S FORM`) with Quebec endorsements (Q.E.F.): the
declarations — the named insured and address, the contract period, the vehicle, the coverages with their amounts,
deductibles and premiums, the endorsements, the annual premium and tax, the main driver and the discounts — and a
cover page, the general conditions and notices. The notebook `02_structured_outputs` sends the model only the declarations pages, 3,925 to
4,963 characters per contract.

**Source.** RISCBAC, a corpus of 10,000 synthetic bilingual (English and French) automobile insurance contracts
generated on the model of Quebec's regulatory standard form, published by David Beauchemin and Richard Khoury:

> Beauchemin D, Khoury R (2023) RISC: Generating Realistic Synthetic Bilingual Insurance Contract. arXiv:2304.04212.
> https://arxiv.org/abs/2304.04212

Dataset: <https://huggingface.co/datasets/davebulaval/RISCBAC>. This file holds the first ten English contracts, as its
name says, unmodified. It is the same sample as in the lecturer's earlier EAA seminars; it has
not been compared byte for byte with the copy on Hugging Face.

**Licence.** CC BY 4.0, as declared on the dataset's Hugging Face card. The contracts are synthetic: they describe no
real person, vehicle, insurer or policy. They carry artefacts of the generator, which the notebook points out: every
contract uses the Quebec form while no address is in Quebec, one main driver is 15 years old at inception, some taxes
are printed with floating-point noise (`94.22999999999999$`), and the serial numbers are labelled in French.

Attribution: RISCBAC — synthetic Quebec automobile insurance contracts, D. Beauchemin and R. Khoury (2023),
arXiv:2304.04212, via Hugging Face `davebulaval/RISCBAC`, licensed under CC BY 4.0
(<https://creativecommons.org/licenses/by/4.0/>); first ten English contracts, unmodified.

## `mortality_tables.sqlite`

53,248 bytes, an SQLite 3 database, sha256 `ce714b3cbcbddafa560a74eebbfb3ffc6616e94091e83c0d8d95795b8f0a90bf`.

**What it contains.** One table, `mortality_table`, with the columns `id`, `mortality_table` (the table's name),
`gender` (`MALE` or `FEMALE`), `age` and `probability` (the one-year death probability $q_x$), and SQLite's own
`sqlite_sequence`; no indexes, no views. 824 rows: four tables × two sexes × ages 0 to 102, with no gaps and no
duplicates. The table names are `PKV-Sterbetafel 2022`, `PKV-Sterbetafel 2023`, `PKV-Sterbetafel 2024` and
`PKV-Sterbetafel 2025`. The notebooks open the file read-only and query it with SQL parameters; the model itself never touches the file.

**Source.** The values are the PKV-Sterbetafeln 2022 to 2025, the mortality tables that the Bundesanstalt für
Finanzdienstleistungsaufsicht (BaFin) publishes each year under § 159 (1) VAG for German private health insurance,
based on the calculations of the Verband der Privaten Krankenversicherung. BaFin's download page for the 2025 table
gives as an example that 288 of one million men aged 30 do not reach age 31 ($q_{30}$ = 0.000288); the database holds
the same value. The file is the same as in the lecturer's earlier EAA seminars. It ends at age 102; whether BaFin's
tables continue beyond that age was not checked, and only the value above was compared with BaFin's publication.

These are health-insurance tables. They are used here for look-ups and comparisons, not to price life insurance.

Attribution: PKV-Sterbetafeln 2022, 2023, 2024 and 2025, published by the Bundesanstalt für
Finanzdienstleistungsaufsicht (BaFin), <https://www.bafin.de>.


## `migration/`

The R-to-Python migration example of `06_agentic_ai`, section 4.

| File | What it is |
| --- | --- |
| `legacy/chain_ladder.R` | the legacy chain-ladder reserving script (198 lines of R), unchanged |
| `legacy/triangle.csv` | its incremental claims triangle (10 origin years × 10 development periods) |
| `legacy/r_output_triangle.txt` | R's printout for that triangle, used when R is not installed |
| `hidden/triangle_B.csv` | a second, synthetic triangle that the agents never see (test data) |
| `hidden/triangle_gap.csv` | the given triangle with one cell removed; R stops on it (test data) |
| `hidden/ground_truth_r.json` | R's results for both triangles (with tail factors 1.00 and 1.05 for the given one) and R's error message for the gap triangle, computed with R 4.5.2 and written with 17 significant digits |

**Source.** `chain_ladder.R` and `triangle.csv` are the example of the case study *Actuarial legacy code migration
with a multi-agent system* published with Hatzesberger & Nonneman (2026), *European Actuarial Journal* 16(2):481–523,
at <https://github.com/IAA-AITF/Actuarial-AI-Case-Studies> (`case-studies/2026/actuarial_legacy_code_migration_multi-agent_system`),
© 2025 International Actuarial Association, MIT licence. The hidden triangles and R's results were prepared by the
author for his EAA seminars: triangle B is synthetic, generated with a fixed seed; the results come from running the
unchanged R script.
