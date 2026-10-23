# SMART–AROMA Evaluation Dataset

[Português](README.BR.MD)

This repository documents the dataset used in the Smart Coffee Fog Swarm - AROMA project. It contains English-language questions and structured expectations covering documentary retrieval, soil monitoring, weather, lightning observations, and combinations of these evidence sources.

## Dataset overview

| Property | Description |
| --- | --- |
| Evaluation unit | One user question and its structured behavioral expectations |
| Number of cases | 93 |
| Primary language | English (`en-US`) |
| Primary file | `dataset/db.jsonl` |
| Supplementary Portuguese file | `dataset/db.br.jsonl` |
| Scenario families | 11 |
| Intended use | Evaluation and regression testing of evidence-grounded agent systems |
| Reference answers | No `expected_output` field in the distributed English cases |

The primary dataset and this documentation are in English. The Portuguese file is retained as a supplementary language version. It must not be counted as 93 additional independent questions. The English file is the primary dataset documented here.

## Files and formats

| File | Purpose |
| --- | --- |
| [`dataset/db.jsonl`](dataset/db.jsonl) | Canonical English dataset; each non-empty line is an independent JSON object. |
| [`dataset/db.json`](dataset/db.json) | The same 93 English cases stored as a JSON array. |
| [`dataset/db.br.json`](dataset/db.br.json) | The same 93 cases from the Portuguese version stored as a JSON array. |
| [`dataset/db.br.jsonl`](dataset/db.br.jsonl) | Supplementary Portuguese questions with structured expectations. |

The JSONL files store one case per line. The JSON files store all cases in a single array, preserving the fields and order of their corresponding JSONL version. The English and Portuguese files are language versions of the question set, rather than independent collections to be added together.

## Scenario coverage

| Scenario | Description | Cases |
| --- | --- | ---: |
| `rag_only` | Documentary retrieval | 12 |
| `soil_only` | Soil conditions | 10 |
| `soil_weather` | Operational soil and weather questions | 3 |
| `weather_current` | Current weather observations | 10 |
| `weather_forecast` | Weather forecasts | 10 |
| `weather_history` | Historical weather observations | 8 |
| `lightning_current` | Current lightning assessment | 8 |
| `lightning_history` | Historical lightning observations | 6 |
| `weather_lightning` | Weather and lightning integration | 10 |
| `rag_soil_weather` | Documentary, soil, and weather integration | 10 |
| `controlled_failure` | Missing, unavailable, or partial evidence | 6 |
| **Total** | | **93** |

## Provenance and construction

The original project's dataset documentation traces the questions to its Postman collection, the earlier [Fazenda NSAAB question set](https://github.com/SMART-AI-Agents-A2A/smartquestions), confirmed interface regressions, and curated technical documents. Eight operational questions were reformulated from Postman seeds. Documentary coverage was designed around 11 PDF/Markdown source pairs in the original project.

The inputs were adapted to the agents, skills, and evidence contracts of Smart Coffee Fog Swarm. Previously recorded sensor values and old response text were not adopted as fixed reference answers. Source documents may be in languages other than English.

## Generation configuration

| Parameter | Value |
| --- | --- |
| Provider | OpenAI |
| Model | GPT-5.6 |
| Temperature | `0.7` |
| Top-p | `1.0` |
| Source material | Technical documents on coffee cultivation and agronomy |
| Output languages | English (`en-US`) and Brazilian Portuguese (`pt-BR`) |

A temperature of `0.7` was selected to balance linguistic diversity with technical coherence across the generated questions. Top-p remains neutral so that sampling behavior is controlled primarily by temperature. These parameters describe dataset preparation only; models and inference settings used to run or evaluate the cases must be documented separately for each experiment.

## Record structure

| Field | Meaning |
| --- | --- |
| `id` | Stable case identifier. |
| `input` | User question in English in the primary dataset. |
| `scenario` | Scenario family used for grouping and filtering. |
| `tags` | Reserved field; all current cases use `[]`. |
| `expected_agents` | Expected agent capabilities, expressed using the original system's agent names. |
| `expected_skills` | Expected skill identifiers from the original system. |
| `expected_evidence_layers` | Expected evidence layers, such as documentary, observational, forecast, or spatial. |
| `accepted_data_quality_statuses` | Data-quality states accepted for the scenario. |
| `faithfulness_required` | Whether faithfulness to retrieved or observed evidence must be evaluated. |
| `retrieval_context_policy` | Required behavior when evidence-derived retrieval context is missing. |
| `expected_route_type` | Expected public routing classification, when supplied. |
| `expected_response_mode` | Expected response intent, when supplied. |
| `expected_tool_calls` | Partial tool-call and argument expectations, when supplied. |
| `response_policy` | Additional response-policy metadata, when supplied. |

Optional fields are not present in every case. Agent and skill names express the original implementation's contract. Other architectures need an explicit capability mapping; matching internal execution trajectories is not automatically a valid comparison criterion.

## Interpretation of the cases

Each case combines a user question with expectations about the evidence and capabilities needed to answer it. These expectations document the behavior assessed in the original project; they are not textual reference answers.

The dataset does not provide an `expected_output` field. Operational answers depend on the available observations, forecasts, and retrieved documents, rather than on fixed sensor values embedded in the questions.

The six `controlled_failure` cases represent unavailable, missing, or partial information. They describe the expected behavior under those conditions; their questions do not themselves cause a source to fail.

The context policies distinguish cases that require evidence (`require_context`) from those that allow faithfulness assessment to be skipped when context is absent (`skip_faithfulness_if_context_missing`).

