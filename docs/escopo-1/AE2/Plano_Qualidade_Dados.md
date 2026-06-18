# Plano de Qualidade de Dados

**Projeto:** Monitoramento e Prevenção de Focos de Dengue com Participação Cidadã  
**Data:** Junho/2026

---

## Objetivo

Definir checagens objetivas e verificáveis para garantir a qualidade, confiabilidade e consistência dos dados antes de iniciar a análise exploratória (EDA) e a construção do dashboard. Todas as checagens devem ser executadas via scripts (Python/Pandas) e documentadas com seus resultados.

---

## Checagens de Qualidade

### Checagem 1 — Duplicidade de Registros

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `notificacoes_dengue` |
| **Descrição** | Verificar se existem registros duplicados com base nos campos `dt_notificacao`, `id_localidade`, `sexo`, `faixa_etaria` e `dt_sintomas`. |
| **Regra** | Não deve haver linhas completamente idênticas nos campos listados. |
| **Ação corretiva** | Remover duplicatas, mantendo o primeiro registro encontrado. Registrar a quantidade removida no log de limpeza. |
| **Métrica** | `total_duplicatas / total_registros × 100` — Meta: < 1%. |

---

### Checagem 2 — Nulos em Campos Obrigatórios

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `notificacoes_dengue`, `dados_climaticos` |
| **Descrição** | Verificar campos definidos como "Obrigatório" no Dicionário de Dados que contenham valores nulos (`NULL`, vazio, `NaN`). |
| **Campos verificados** | `notificacoes_dengue`: `dt_notificacao`, `dt_sintomas`, `semana_epidemiologica`, `id_localidade`, `sexo`, `faixa_etaria`, `classificacao_final`. `dados_climaticos`: `dt_registro`, `id_localidade`, `temperatura_media`, `precipitacao_mm`. |
| **Regra** | Campos obrigatórios devem ter 0% de nulos. |
| **Ação corretiva** | Registros com nulos em campos-chave (`dt_notificacao`, `id_localidade`) serão descartados. Campos como `sexo` e `faixa_etaria` com nulo podem ser marcados como "Ignorado" ou "Não informado" se representarem < 5% dos dados. |
| **Métrica** | Percentual de completude por campo — Meta: ≥ 95% em todos os campos obrigatórios. |

---

### Checagem 3 — Domínio de Valores (Categorias Válidas)

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `notificacoes_dengue` |
| **Descrição** | Verificar se os campos categóricos contêm apenas valores pertencentes ao domínio esperado definido no Dicionário de Dados. |
| **Campos e domínios** | `sexo`: {`M`, `F`, `I`}. `faixa_etaria`: {`0-4`, `5-9`, `10-19`, `20-39`, `40-59`, `60+`}. `classificacao_final`: {`Dengue`, `Dengue com Sinais de Alarme`, `Dengue Grave`, `Descartado`, `Inconclusivo`}. `evolucao_caso`: {`Cura`, `Óbito pelo Agravo`, `Óbito por Outras Causas`, `Ignorado`}. |
| **Regra** | 100% dos valores devem pertencer ao domínio. Valores fora do domínio indicam erro de preenchimento ou codificação diferente na fonte. |
| **Ação corretiva** | Mapear valores encontrados fora do domínio para categorias válidas (ex.: `"Masc"` → `"M"`, `"Fem"` → `"F"`). Valores impossíveis de mapear serão categorizados como `"Ignorado"`. |
| **Métrica** | `registros_fora_dominio / total_registros × 100` — Meta: 0%. |

---

### Checagem 4 — Consistência Temporal (Datas)

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `notificacoes_dengue` |
| **Descrição** | Verificar a consistência lógica entre as datas do mesmo registro. |
| **Regras** | 1. `dt_sintomas` ≤ `dt_notificacao` (sintomas surgem antes ou no dia da notificação). 2. `dt_notificacao` ≤ data atual (não pode haver notificações futuras). 3. Se `dt_encerramento` estiver preenchida, deve ser ≥ `dt_notificacao`. 4. As datas devem estar dentro do período de análise definido (2022–2025). |
| **Ação corretiva** | Registros com `dt_sintomas` > `dt_notificacao` serão sinalizados para revisão. Se a diferença for ≤ 7 dias, inverter as datas (possível erro de digitação). Diferenças maiores descartam o registro. Registros fora do período de análise serão filtrados. |
| **Métrica** | `registros_inconsistentes / total_registros × 100` — Meta: < 2%. |

---

### Checagem 5 — Integridade Referencial

| Item | Detalhe |
|---|---|
| **Tabelas alvo** | `notificacoes_dengue` ↔ `localidades`, `dados_climaticos` ↔ `localidades` |
| **Descrição** | Verificar se todos os valores de `id_localidade` nas tabelas fato existem na tabela dimensão `localidades`. |
| **Regra** | Nenhum `id_localidade` em `notificacoes_dengue` ou `dados_climaticos` pode ser "órfão" (sem correspondência em `localidades`). |
| **Ação corretiva** | Localidades ausentes devem ser adicionadas à tabela `localidades` com dados obtidos do IBGE. Se impossível identificar a localidade, o registro será marcado com `id_localidade = -1` (localidade desconhecida), que será adicionada à tabela dimensão. |
| **Métrica** | `registros_orfaos / total_registros × 100` — Meta: 0%. |

---

### Checagem 6 — Faixas Plausíveis de Dados Climáticos

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `dados_climaticos` |
| **Descrição** | Verificar se os valores numéricos dos dados meteorológicos estão dentro de faixas fisicamente plausíveis para o Brasil. |
| **Faixas** | `temperatura_media`: -10,0 a 50,0 °C. `temperatura_maxima`: -10,0 a 55,0 °C (deve ser ≥ `temperatura_media`). `temperatura_minima`: -15,0 a 45,0 °C (deve ser ≤ `temperatura_media`). `precipitacao_mm`: 0 a 400 mm. `umidade_relativa_media`: 0 a 100%. `velocidade_vento_media`: 0 a 50 m/s. |
| **Ação corretiva** | Valores fora da faixa serão tratados como outliers: registrados no log, e substituídos por `NULL` para não distorcer médias. Valores extremos mas plausíveis (ex.: chuvas de 300mm em eventos extremos) serão mantidos com flag. |
| **Métrica** | `registros_fora_faixa / total_registros × 100` — Meta: < 0,5%. |

---

### Checagem 7 — Consistência entre Data de Sintomas e Data de Notificação (Lag)

| Item | Detalhe |
|---|---|
| **Tabela alvo** | `notificacoes_dengue` |
| **Descrição** | Verificar se o intervalo (lag) entre `dt_sintomas` e `dt_notificacao` é razoável. Atrasos muito grandes podem indicar erro de registro ou subnotificação tardia. |
| **Regra** | O lag (`dt_notificacao` − `dt_sintomas`) deve estar entre 0 e 30 dias para a maioria dos casos. |
| **Ação corretiva** | Registros com lag > 60 dias serão sinalizados como suspeitos. Lag negativo (tratado na Checagem 4). Registros suspeitos serão mantidos mas com flag `lag_suspeito = True` para análise posterior. |
| **Métrica** | `registros_com_lag_excessivo / total_registros × 100` — Meta: < 5%. |

---

### Checagem 8 — Completude Mínima por Campo

| Item | Detalhe |
|---|---|
| **Tabelas alvo** | Todas |
| **Descrição** | Gerar um relatório geral de completude (% de preenchimento) para cada campo de cada tabela, incluindo campos opcionais. |
| **Regra** | Campos obrigatórios: completude ≥ 95%. Campos opcionais: completude ≥ 50% (para viabilizar análises que os utilizem). |
| **Ação corretiva** | Campos opcionais com completude < 30% serão excluídos da análise exploratória e do dashboard (não são confiáveis para gerar insights). Será registrado em documentação quais campos foram excluídos e por quê. |
| **Métrica** | Tabela com `campo | total_registros | preenchidos | % completude | status (OK/ALERTA/EXCLUÍDO)`. |

---

## Resumo das Checagens

| # | Checagem | Tabela(s) | Meta |
|---|---|---|---|
| 1 | Duplicidade de Registros | `notificacoes_dengue` | < 1% de duplicatas |
| 2 | Nulos em Campos Obrigatórios | `notificacoes_dengue`, `dados_climaticos` | ≥ 95% de completude |
| 3 | Domínio de Valores | `notificacoes_dengue` | 0% fora do domínio |
| 4 | Consistência Temporal | `notificacoes_dengue` | < 2% inconsistentes |
| 5 | Integridade Referencial | Fato ↔ `localidades` | 0% órfãos |
| 6 | Faixas Plausíveis | `dados_climaticos` | < 0,5% fora da faixa |
| 7 | Lag Sintomas → Notificação | `notificacoes_dengue` | < 5% com lag excessivo |
| 8 | Completude Mínima | Todas | ≥ 95% (obrigatórios), ≥ 50% (opcionais) |

---

## Ferramentas de Execução

- **Python 3.x** com **Pandas** para execução das checagens programáticas.
- Cada checagem será implementada como uma função individual em um notebook ou script, retornando o resultado e a métrica.
- Os resultados serão salvos em um relatório de qualidade (`relatorio_qualidade.csv`) e documentados no repositório.
