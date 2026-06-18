# Dicionário de Dados — Versão 1

**Projeto:** Monitoramento e Prevenção de Focos de Dengue com Participação Cidadã  
**Data:** Junho/2026  
**Escopo:** Dados de notificações de Dengue (SINAN/DataSUS) e dados climáticos (INMET)

---

## 1. Tabela: `notificacoes_dengue`

Fonte primária: **SINAN / DataSUS** — Download de planilhas CSV/XLSX via portal de Dados Abertos.

| Campo | Tipo Esperado | Descrição | Origem | Regra de Preenchimento / Validação |
|---|---|---|---|---|
| `id_notificacao` | `INTEGER` (PK) | Identificador único da notificação | Gerado (sequencial) | Obrigatório. Autoincremento. Não pode ser nulo nem duplicado. |
| `dt_notificacao` | `DATE` | Data em que a notificação foi registrada no sistema | SINAN | Obrigatório. Formato `YYYY-MM-DD`. Deve ser ≥ `dt_sintomas`. Não pode ser data futura. |
| `dt_sintomas` | `DATE` | Data do início dos primeiros sintomas do paciente | SINAN | Obrigatório. Formato `YYYY-MM-DD`. Deve ser ≤ `dt_notificacao`. |
| `semana_epidemiologica` | `INTEGER` | Semana epidemiológica correspondente à data de notificação | SINAN | Obrigatório. Valor entre 1 e 53. |
| `ano_notificacao` | `INTEGER` | Ano da notificação | SINAN | Obrigatório. Valor entre 2020 e ano corrente. |
| `id_localidade` | `INTEGER` (FK) | Referência ao município/bairro de residência do paciente | SINAN → `localidades` | Obrigatório. Deve existir na tabela `localidades`. |
| `sexo` | `VARCHAR(1)` | Sexo do paciente | SINAN | Obrigatório. Domínio: `M` (masculino), `F` (feminino), `I` (ignorado). |
| `faixa_etaria` | `VARCHAR(20)` | Faixa etária do paciente (agrupada) | SINAN (derivado) | Obrigatório. Domínio: `0-4`, `5-9`, `10-19`, `20-39`, `40-59`, `60+`. |
| `classificacao_final` | `VARCHAR(30)` | Classificação final do caso | SINAN | Obrigatório. Domínio: `Dengue`, `Dengue com Sinais de Alarme`, `Dengue Grave`, `Descartado`, `Inconclusivo`. |
| `evolucao_caso` | `VARCHAR(20)` | Desfecho / evolução do caso | SINAN | Obrigatório. Domínio: `Cura`, `Óbito pelo Agravo`, `Óbito por Outras Causas`, `Ignorado`. |
| `dt_encerramento` | `DATE` | Data de encerramento/investigação do caso | SINAN | Opcional. Se preenchido, formato `YYYY-MM-DD` e ≥ `dt_notificacao`. |
| `criterio_confirmacao` | `VARCHAR(20)` | Critério utilizado para confirmação do caso | SINAN | Opcional. Domínio: `Laboratorial`, `Clínico-Epidemiológico`. |

---

## 2. Tabela: `localidades`

Fonte primária: **SINAN / IBGE** — Códigos de município e nomes de bairros/distritos.

| Campo | Tipo Esperado | Descrição | Origem | Regra de Preenchimento / Validação |
|---|---|---|---|---|
| `id_localidade` | `INTEGER` (PK) | Identificador único da localidade | Gerado / IBGE | Obrigatório. Não pode ser nulo nem duplicado. |
| `codigo_ibge` | `VARCHAR(7)` | Código IBGE do município (7 dígitos) | IBGE | Obrigatório. Exatamente 7 caracteres numéricos. |
| `nome_municipio` | `VARCHAR(100)` | Nome do município | IBGE | Obrigatório. Texto não vazio. |
| `nome_bairro` | `VARCHAR(100)` | Nome do bairro ou distrito sanitário | SINAN / Prefeitura | Opcional. Texto livre. Padronizar para maiúsculas e sem acentos na limpeza. |
| `uf` | `VARCHAR(2)` | Sigla da Unidade Federativa | IBGE | Obrigatório. Domínio: siglas válidas de UF (ex.: `SP`, `RJ`, `MG`…). |
| `latitude` | `DECIMAL(9,6)` | Latitude do centroide da localidade | IBGE / Geocodificação | Opcional. Valor entre -33.75 e 5.27 (limites do Brasil). |
| `longitude` | `DECIMAL(9,6)` | Longitude do centroide da localidade | IBGE / Geocodificação | Opcional. Valor entre -73.99 e -34.79 (limites do Brasil). |
| `populacao_estimada` | `INTEGER` | População estimada do município/bairro | IBGE | Opcional. Valor > 0. Usado para cálculo de taxa de incidência. |

---

## 3. Tabela: `dados_climaticos`

Fonte primária: **INMET** — Instituto Nacional de Meteorologia, dados de estações automáticas.

| Campo | Tipo Esperado | Descrição | Origem | Regra de Preenchimento / Validação |
|---|---|---|---|---|
| `id_registro_climatico` | `INTEGER` (PK) | Identificador único do registro climático | Gerado (sequencial) | Obrigatório. Autoincremento. Não pode ser nulo nem duplicado. |
| `id_localidade` | `INTEGER` (FK) | Referência à localidade mais próxima da estação | INMET → `localidades` | Obrigatório. Deve existir na tabela `localidades`. |
| `dt_registro` | `DATE` | Data da medição | INMET | Obrigatório. Formato `YYYY-MM-DD`. Não pode ser data futura. |
| `semana_epidemiologica` | `INTEGER` | Semana epidemiológica correspondente ao registro | Derivado de `dt_registro` | Obrigatório. Valor entre 1 e 53. |
| `temperatura_media` | `DECIMAL(4,1)` | Temperatura média diária (°C) | INMET | Obrigatório. Faixa plausível: -10.0 a 50.0 °C. |
| `temperatura_maxima` | `DECIMAL(4,1)` | Temperatura máxima do dia (°C) | INMET | Opcional. Deve ser ≥ `temperatura_media`. Faixa: -10.0 a 55.0 °C. |
| `temperatura_minima` | `DECIMAL(4,1)` | Temperatura mínima do dia (°C) | INMET | Opcional. Deve ser ≤ `temperatura_media`. Faixa: -15.0 a 45.0 °C. |
| `precipitacao_mm` | `DECIMAL(6,1)` | Precipitação acumulada no dia (mm) | INMET | Obrigatório. Valor ≥ 0. Faixa plausível: 0 a 400 mm. |
| `umidade_relativa_media` | `DECIMAL(4,1)` | Umidade relativa média do ar (%) | INMET | Opcional. Faixa: 0 a 100%. |
| `velocidade_vento_media` | `DECIMAL(4,1)` | Velocidade média do vento (m/s) | INMET | Opcional. Valor ≥ 0. Faixa plausível: 0 a 50 m/s. |

---

## 4. Tabela: `periodos` (Dimensão Temporal)

Fonte: **Derivada** — gerada a partir do calendário e das semanas epidemiológicas.

| Campo | Tipo Esperado | Descrição | Origem | Regra de Preenchimento / Validação |
|---|---|---|---|---|
| `id_periodo` | `INTEGER` (PK) | Identificador único do período | Gerado | Obrigatório. Autoincremento. |
| `ano` | `INTEGER` | Ano do período | Derivado | Obrigatório. Valor entre 2020 e ano corrente. |
| `mes` | `INTEGER` | Mês do período | Derivado | Obrigatório. Valor entre 1 e 12. |
| `semana_epidemiologica` | `INTEGER` | Semana epidemiológica | Derivado | Obrigatório. Valor entre 1 e 53. |
| `dt_inicio_semana` | `DATE` | Data de início da semana epidemiológica | Derivado | Obrigatório. Formato `YYYY-MM-DD`. |
| `dt_fim_semana` | `DATE` | Data de fim da semana epidemiológica | Derivado | Obrigatório. Formato `YYYY-MM-DD`. Deve ser `dt_inicio_semana` + 6 dias. |
| `estacao_ano` | `VARCHAR(15)` | Estação do ano (relevante para sazonalidade) | Derivado | Obrigatório. Domínio: `Verão`, `Outono`, `Inverno`, `Primavera`. |

---

## Notas Gerais

- **Anonimização:** Conforme definido na AE1, nenhum campo permite identificação individual. Dados são sempre agregados por localidade e faixa etária/sexo.
- **Formato de datas:** Todas as datas seguem o padrão ISO 8601 (`YYYY-MM-DD`).
- **Encoding:** Todos os arquivos CSV devem ser importados com encoding `UTF-8`.
- **Valores ausentes:** Campos marcados como "Obrigatório" que contenham nulo serão reportados no Plano de Qualidade de Dados. Campos opcionais com nulo serão mantidos como `NULL`.
