# GenAI Beyond the Basics: Advanced Concepts for Actuaries

Companion material for the **EAA Web Session E0595** of the European Actuarial Academy, held online on
**7 December 2026, 09:00–12:10 CET**, by **Dr Simon Hatzesberger**: seven Jupyter notebooks, one per block of the
programme.

**This repository is public until 21 December 2026** and will then be made private. Registered participants receive
the slides of the web session from the European Actuarial Academy; they are not part of this repository.

**You do not need to install anything to follow the session.** Every notebook is saved with its outputs, so you can
read it right here on GitHub (click its name below) or as HTML (folder [`html/`](html/), download and open in a
browser). To *run* the code yourself, use the **Open in Colab** buttons, or a local Python installation.

---

## Notebooks

| Programme | Read | Run | Covers |
|---|---|---|---|
| before the session | [`00_setup_check`](notebooks/00_setup_check.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/00_setup_check.ipynb) | Optional: checks packages, your API key and one test call |
| 09:10–09:45 | [`01_foundations_best_practices`](notebooks/01_foundations_best_practices.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/01_foundations_best_practices.ipynb) | Calling LLMs via API, instructions, reasoning effort, tokens and cost; a local open-weights model with Ollama; prompt-engineering best practices; limitations (hallucination, knowledge cut-off, prompt sensitivity) |
| 09:45–10:05 | [`02_structured_outputs`](notebooks/02_structured_outputs.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/02_structured_outputs.ipynb) | Extracting policy data from synthetic motor contracts in three stages (free text → flat schema → nested schema), checks in code, and ten contracts to a portfolio table |
| 10:05–10:25 | [`03_function_calling`](notebooks/03_function_calling.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/03_function_calling.ipynb) | The six-step function-calling round trip on a read-only database of German PKV mortality tables; checking every number; handling missing data |
| 10:35–10:55 | [`04_fine_tuning`](notebooks/04_fine_tuning.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/04_fine_tuning.ipynb) | Claim-cause coding: the OpenAI supervised fine-tuning workflow (and its wind-down), plus a real LoRA fine-tuning run of a small open-weights model |
| 10:55–11:15 | [`05_rag`](notebooks/05_rag.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/05_rag.ipynb) | Retrieval-augmented generation over an insurer's 458-page annual report: chunking, embeddings, retrieval, structured answers with source pages |
| 11:15–12:10 | [`06_agentic_ai`](notebooks/06_agentic_ai.ipynb) | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/simonhatzesberger/genai-beyond-the-basics-20261207/blob/main/notebooks/06_agentic_ai.ipynb) | A tool-using actuarial agent with guardrails; a multi-agent system of five agents (R analysis, translation, compilation, test runner, report) that migrates a legacy R chain-ladder script to Python, judged by a test suite on data the agents never see; outlook |

---

## Running the notebooks

### Option 1: Google Colab (recommended, nothing to install)

1. Click an **Open in Colab** button above and sign in with a Google account.
2. Add your OpenAI API key once as a Colab secret: key icon 🔑 in the left sidebar → *Add new secret* → name
   `OPENAI_API_KEY`, value = your key, switch on *Notebook access*.
3. *Runtime → Run all*. Colab warns that the notebook was not authored by Google; click *Run anyway*.

Each notebook installs the few packages Colab lacks and downloads its small data files from this repository by
itself. Company laptops sometimes block Colab or the Google sign-in; a private device usually works.

### Option 2: local Python

Python 3.10 or newer (tested with 3.13):

```bash
git clone https://github.com/simonhatzesberger/genai-beyond-the-basics-20261207.git
cd genai-beyond-the-basics-20261207
python -m venv .venv
.venv\Scripts\activate            # Windows;  macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
pip install -r requirements-lora.txt   # optional: only for part B of 04_fine_tuning (PyTorch, ~1 GB)
jupyter lab
```

Without Git, use the green **Code** button → **Download ZIP**. Set your key as an environment variable in the same
terminal before starting Jupyter (`$env:OPENAI_API_KEY = "sk-..."` in PowerShell, `export OPENAI_API_KEY="sk-..."`
on macOS/Linux), or put it into a file `openai_api_key.txt` in the `notebooks/` folder (never share or commit it).

**Optional local model.** Section 3 of `01_foundations_best_practices` runs an open-weights model on your own machine
with [Ollama](https://ollama.com): install it and run `ollama pull qwen3.5:4b` once (about 3.4 GB). Without Ollama,
that single cell is skipped.

### API key and costs

The notebooks call OpenAI's API with **your own key** (create one at <https://platform.openai.com/api-keys>; a paid
account with a small prepaid balance is enough). All notebooks use the model `gpt-6-luna`; running all of them once
costs well under USD 0.50 at the list prices of 22 November 2026. Do not send personal or confidential company data
to a model while experimenting.

---

## Repository structure

```
genai-beyond-the-basics-20261207/
├── README.md
├── requirements.txt          ← packages for all notebooks
├── requirements-lora.txt     ← optional: PyTorch etc. for part B of 04_fine_tuning
├── LICENSE                   ← MIT, for the code
├── LICENSE-CONTENT.md        ← CC BY 4.0, for the text and figures
├── notebooks/
│   ├── 00_setup_check.ipynb … 06_agentic_ai.ipynb
│   └── data/                 ← synthetic motor contracts, the mortality-table database, the R migration example (see data/README.md)
└── html/                     ← the notebooks as HTML, for reading without Jupyter
```

---

## Background

The use cases in notebooks 02, 03, 05 and the multi-agent part of 06 follow the case studies of

> Hatzesberger S, Nonneman I (2026) Advanced applications of generative AI in actuarial science: case studies
> beyond ChatGPT. *European Actuarial Journal* 16(2):481–523. <https://doi.org/10.1007/s13385-026-00464-9>
> (preprint: [arXiv:2506.18942](https://arxiv.org/abs/2506.18942))

Further case studies by the IAA AI Task Force: <https://github.com/IAA-AITF/Actuarial-AI-Case-Studies>.

Model names, prices and availability were checked on 22 November 2026 and change often; see OpenAI's
[pricing](https://developers.openai.com/api/docs/pricing) and [deprecations](https://developers.openai.com/api/docs/deprecations)
pages for the current state.

---

## Licences

| What | Licence |
|---|---|
| Code | MIT, see [LICENSE](LICENSE) |
| Notebook text, documentation, figures (author's own content) | CC BY 4.0, see [LICENSE-CONTENT.md](LICENSE-CONTENT.md) |
| Data in `notebooks/data/` | their own terms, see [notebooks/data/README.md](notebooks/data/README.md) |

The slides of the web session are not part of this repository and are not covered by these licences; they are
subject to the EAA's copyright and are available only to registered participants. EAA branding, third-party documents
(such as the annual report downloaded in `05_rag`) and installed packages are not covered by these licences either.

## Contact

Questions about the material: Dr Simon Hatzesberger, <https://github.com/simonhatzesberger>.
Questions about the web session, registration or logistics: the [European Actuarial Academy](https://actuarial-academy.com/).
