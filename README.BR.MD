# Dataset de avaliação SMART–AROMA

[English](README.md)

Este repositório documenta o dataset utilizado no projeto Smart Coffee Fog Swarm - AROMA. Ele contém perguntas em inglês e expectativas estruturadas que abrangem recuperação documental, monitoramento do solo, meteorologia, observações de descargas atmosféricas e combinações dessas fontes de evidência.

## Visão geral do dataset

| Propriedade | Descrição |
| --- | --- |
| Unidade de avaliação | Uma pergunta do usuário e suas expectativas estruturadas de comportamento |
| Número de casos | 93 |
| Idioma principal | Inglês (`en-US`) |
| Arquivo principal | `dataset/db.jsonl` |
| Arquivo complementar em português | `dataset/db.br.jsonl` |
| Famílias de cenários | 11 |
| Uso previsto | Avaliação e testes de regressão de sistemas de agentes com respostas fundamentadas em evidências |
| Respostas de referência | Os casos distribuídos em inglês não contêm o campo `expected_output` |

O dataset principal e o README original estão em inglês. Este arquivo apresenta a mesma documentação em português do Brasil. O arquivo de perguntas em português é mantido como versão complementar e não deve ser contabilizado como 93 perguntas independentes adicionais. O arquivo em inglês é o dataset principal documentado aqui.

## Arquivos e formatos

| Arquivo | Finalidade |
| --- | --- |
| [`dataset/db.jsonl`](dataset/db.jsonl) | Dataset canônico em inglês; cada linha não vazia contém um objeto JSON independente. |
| [`dataset/db.json`](dataset/db.json) | Os mesmos 93 casos em inglês, armazenados em um array JSON. |
| [`dataset/db.br.json`](dataset/db.br.json) | Os mesmos 93 casos da versão em português, armazenados em um array JSON. |
| [`dataset/db.br.jsonl`](dataset/db.br.jsonl) | Perguntas complementares em português com expectativas estruturadas. |

Os arquivos JSONL armazenam um caso por linha. Os arquivos JSON armazenam todos os casos em um único array, preservando os campos e a ordem de sua respectiva versão JSONL. Os arquivos em inglês e português são versões linguísticas do conjunto de perguntas, e não coleções independentes que devam ser somadas.

## Cobertura dos cenários

| Cenário | Descrição | Casos |
| --- | --- | ---: |
| `rag_only` | Recuperação documental | 12 |
| `soil_only` | Condições do solo | 10 |
| `soil_weather` | Perguntas operacionais sobre solo e meteorologia | 3 |
| `weather_current` | Observações meteorológicas atuais | 10 |
| `weather_forecast` | Previsões meteorológicas | 10 |
| `weather_history` | Observações meteorológicas históricas | 8 |
| `lightning_current` | Avaliação atual de descargas atmosféricas | 8 |
| `lightning_history` | Observações históricas de descargas atmosféricas | 6 |
| `weather_lightning` | Integração de meteorologia e descargas atmosféricas | 10 |
| `rag_soil_weather` | Integração de documentação, solo e meteorologia | 10 |
| `controlled_failure` | Evidências ausentes, indisponíveis ou parciais | 6 |
| **Total** | | **93** |

## Origem e elaboração

A documentação do dataset no projeto original relaciona as perguntas à coleção Postman, ao [conjunto anterior de perguntas da Fazenda NSAAB](https://github.com/SMART-AI-Agents-A2A/smartquestions), a regressões confirmadas na interface e a documentos técnicos selecionados. Oito perguntas operacionais foram reformuladas a partir de consultas da coleção Postman. A cobertura documental foi organizada com base em 11 pares de fontes PDF/Markdown do projeto original.

As entradas foram adaptadas aos agentes, às skills e aos contratos de evidência do Smart Coffee Fog Swarm. Valores de sensores registrados anteriormente e textos de respostas antigas não foram adotados como respostas de referência fixas. Os documentos de origem podem estar em outros idiomas além do inglês.

## Configuração de geração

| Parâmetro | Valor |
| --- | --- |
| Provedor | OpenAI |
| Modelo | GPT-5.6 |
| Temperatura | `0.7` |
| Top-p | `1.0` |
| Fontes utilizadas | Documentos técnicos sobre cafeicultura e agronomia |
| Idiomas de saída | Inglês (`en-US`) e português do Brasil (`pt-BR`) |

A temperatura de `0.7` foi escolhida para equilibrar diversidade linguística e coerência técnica na formulação das perguntas. O top-p permanece neutro para que o comportamento da amostragem seja controlado principalmente pela temperatura. Esses parâmetros descrevem apenas a elaboração do dataset; os modelos e as configurações de inferência utilizados na execução ou na avaliação dos casos devem ser documentados separadamente em cada experimento.

## Estrutura dos registros

| Campo | Significado |
| --- | --- |
| `id` | Identificador estável do caso. |
| `input` | Pergunta do usuário, em inglês no dataset principal. |
| `scenario` | Família de cenário utilizada para agrupamento e filtragem. |
| `tags` | Campo reservado; todos os casos atuais utilizam `[]`. |
| `expected_agents` | Capacidades de agentes esperadas, expressas pelos nomes dos agentes do sistema original. |
| `expected_skills` | Identificadores das skills esperadas no sistema original. |
| `expected_evidence_layers` | Camadas de evidência esperadas, como documental, observacional, previsão ou espacial. |
| `accepted_data_quality_statuses` | Estados de qualidade dos dados aceitos para o cenário. |
| `faithfulness_required` | Indica se a fidelidade às evidências recuperadas ou observadas deve ser avaliada. |
| `retrieval_context_policy` | Comportamento exigido quando não há contexto de recuperação derivado das evidências. |
| `expected_route_type` | Classificação pública de roteamento esperada, quando informada. |
| `expected_response_mode` | Intenção de resposta esperada, quando informada. |
| `expected_tool_calls` | Expectativas parciais de chamadas de ferramentas e argumentos, quando informadas. |
| `response_policy` | Metadados adicionais de política de resposta, quando informados. |

Os campos opcionais não estão presentes em todos os casos. Os nomes dos agentes e das skills expressam o contrato da implementação original. Outras arquiteturas precisam de um mapeamento explícito de capacidades; a correspondência entre trajetórias internas de execução não é automaticamente um critério válido de comparação.

## Interpretação dos casos

Cada caso combina uma pergunta do usuário com expectativas sobre as evidências e as capacidades necessárias para respondê-la. Essas expectativas documentam o comportamento avaliado no projeto original; não constituem respostas textuais de referência.

O dataset não fornece um campo `expected_output`. As respostas operacionais dependem das observações, das previsões e dos documentos recuperados disponíveis, e não de valores fixos de sensores incorporados às perguntas.

Os seis casos `controlled_failure` representam informações indisponíveis, ausentes ou parciais. Eles descrevem o comportamento esperado nessas condições; as perguntas, por si só, não provocam falhas em uma fonte.

As políticas de contexto distinguem os casos que exigem evidência (`require_context`) daqueles que permitem dispensar a avaliação de fidelidade quando não há contexto (`skip_faithfulness_if_context_missing`).

## Escopo e limitações

O dataset concentra-se nos agentes e nos contratos de evidência do Smart Coffee Fog Swarm / AROMA. Sua cobertura é específica desse projeto e não representa todas as perguntas possíveis sobre agricultura.

As perguntas distinguem condições observadas, previsões, orientações documentais e informações ausentes. Observações ausentes não devem ser interpretadas como valores zero, e a ausência de detecções de descargas atmosféricas não deve ser interpretada como garantia de segurança. Conclusões agronômicas exigem evidências de suporte aplicáveis.

A revisão por especialistas e a concordância entre avaliadores não estão documentadas nesta versão.
