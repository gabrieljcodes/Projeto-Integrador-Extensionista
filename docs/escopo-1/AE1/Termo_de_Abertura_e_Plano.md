# Atividade de Estudo 1 (AE1) - Projeto Integrador Extensionista

## Tema Sugerido: Monitoramento e Prevenção de Focos de Dengue com Participação Cidadã

---

## 1. Termo de Abertura + Briefing

### Contexto e Justificativa (Extensão)
O Brasil enfrenta anualmente surtos de Dengue, e a nossa região não é exceção. O controle de focos do mosquito *Aedes aegypti* depende não apenas das ações do poder público, mas também da conscientização e ação da comunidade. A extensão universitária, ao conectar o conhecimento acadêmico de análise de dados com a comunidade, pode fornecer informações valiosas para direcionar mutirões de limpeza e campanhas educativas. O projeto justifica-se por utilizar dados abertos de saúde pública e dados climáticos para prever e mapear áreas de maior risco, engajando a população na prevenção de forma direcionada.

### Objetivo Geral e Objetivos Específicos
**Objetivo Geral:** 
Analisar os dados históricos e atuais de casos de Dengue na região, combinados com dados climáticos, para identificar padrões e direcionar uma ação de conscientização e prevenção na comunidade.

**Objetivos Específicos:**
- Mapear as áreas (bairros/distritos) com maior incidência de casos nos últimos anos.
- Identificar a correlação entre fatores climáticos locais e o aumento de casos.
- Criar um painel (dashboard) interativo e acessível para a comunidade acompanhar a situação.
- Promover uma oficina em uma associação de bairro ou escola local para apresentar os dados e orientar práticas de prevenção.

### Público-Alvo/Beneficiários
- **Beneficiários Diretos:** Moradores dos bairros com maior incidência de Dengue escolhidos para a ação, que receberão informações direcionadas.
- **Beneficiários Indiretos:** Secretaria de Saúde local, que pode utilizar os insights gerados, e a comunidade em geral através da redução de focos.

### Escopo
- **O que entra:** Coleta e limpeza de dados abertos governamentais sobre casos de Dengue; análise de dados climáticos locais; desenvolvimento de um dashboard simples; realização de uma (1) oficina de conscientização presencial ou online com a comunidade.
- **O que não entra:** Desenvolvimento de um aplicativo móvel complexo; ações práticas de pulverização de inseticidas (competência do poder público); análise de outras arboviroses que não a Dengue.

### Perguntas Analíticas
1. Quais são os bairros/regiões da cidade que concentram o maior número de casos notificados de Dengue nos últimos 2 a 3 anos?
2. Existe uma correlação forte e temporalmente observável entre os picos de precipitação/temperatura e o aumento de notificações de casos nas semanas seguintes?
3. Qual é o perfil demográfico (faixa etária, sexo) mais afetado pela Dengue na nossa região, para direcionar melhor a linguagem da campanha?

### Indicadores/KPIs e Definição de Sucesso
1. **Engajamento da Comunidade:** Número de participantes na oficina/palestra (Meta: mínimo de 30 pessoas).
2. **Acessibilidade dos Dados:** Número de visualizações ou acessos únicos ao painel/dashboard criado (Meta: 100 acessos nas primeiras semanas).
3. **Qualidade da Entrega:** Entrega do dashboard funcional com dados integrados e limpos.
- **Definição de Sucesso:** O projeto será considerado um sucesso se entregar o dashboard funcionando com dados abertos reais, e se a ação na comunidade (oficina) atingir a meta de público, gerando feedback positivo através de um questionário rápido aplicado no final.

### Fontes de Dados e Forma de Acesso
1. **Casos de Dengue:** SINAN (Sistema de Informação de Agravos de Notificação) / DataSUS - Acesso via portal de Dados Abertos do Governo Federal ou portal municipal (download de planilhas CSV/XLSX).
2. **Dados Climáticos:** INMET (Instituto Nacional de Meteorologia) - Acesso via portal de dados abertos ou API pública.

### Aspectos Éticos/LGPD (Riscos e Cuidados)
- **Riscos:** Exposição de dados sensíveis de pacientes (identificação pessoal em registros de notificação de saúde).
- **Cuidados:** Serão utilizadas exclusivamente bases de dados abertas e já anonimizadas fornecidas pelos órgãos oficiais. Não haverá processamento de dados que permitam a reidentificação de indivíduos. Os dados serão apresentados de forma agregada (por bairro, cidade ou CEP). Nas ações com a comunidade, caso haja coleta de imagens ou listas de presença, será solicitado o consentimento prévio dos participantes (Termo de Consentimento Livre e Esclarecido - TCLE).

---

## 2. Plano Inicial do Projeto

### EAP/WBS (Entregáveis e Tarefas)
1. **Iniciação e Planejamento**
   1.1. Definição do escopo e elaboração do Termo de Abertura (AE1).
   1.2. Criação e configuração do repositório no GitHub.
2. **Coleta e Preparação de Dados**
   2.1. Obtenção dos dados do SINAN/DataSUS e INMET.
   2.2. Limpeza e tratamento dos dados (remoção de nulos, padronização).
   2.3. Integração das bases de dados (cruzamento por data e localidade).
3. **Análise e Desenvolvimento**
   3.1. Análise exploratória de dados (EDA) para responder às perguntas analíticas.
   3.2. Criação das visualizações de dados (gráficos e mapas de calor).
   3.3. Construção e publicação do Dashboard interativo (ex: Looker Studio, Streamlit).
4. **Ação Extensionista (Comunidade)**
   4.1. Preparação do material educativo (slides ou cartilhas baseadas nos dados).
   4.2. Agendamento e divulgação da oficina na comunidade (associação, escola, etc.).
   4.3. Realização da oficina e coleta de feedback de impacto.
5. **Encerramento**
   5.1. Elaboração do relatório final de projeto.
   5.2. Atualização final da documentação no repositório GitHub.

### Cronograma (Macro, por semana)
- **Semana 1:** Iniciação, elaboração do Termo de Abertura e configuração do repositório GitHub.
- **Semana 2:** Coleta das bases de dados (saúde e clima) e início da limpeza.
- **Semana 3:** Conclusão do tratamento de dados e Análise Exploratória (EDA).
- **Semana 4:** Construção do Dashboard e das principais visualizações.
- **Semana 5:** Preparação do material para a ação extensionista e organização do evento.
- **Semana 6:** Realização da oficina/palestra na comunidade.
- **Semana 7:** Coleta e análise dos feedbacks da comunidade, ajustes finais no Dashboard.
- **Semana 8:** Elaboração do relatório final e entrega da disciplina.

### Matriz de Riscos

| Risco | Impacto | Probabilidade | Mitigação |
|---|---|---|---|
| **1. Atraso ou indisponibilidade dos dados no portal do governo** | Alto | Média | Buscar fontes alternativas, como portais de transparência municipais ou estaduais, ou utilizar recortes temporais mais antigos que já estejam consolidados. |
| **2. Baixa adesão/presença da comunidade na oficina** | Alto | Média | Realizar divulgação com antecedência, utilizar redes sociais locais e buscar apoio de líderes comunitários ou diretores de escola para engajar o público. |
| **3. Dificuldades técnicas na criação do Dashboard** | Médio | Alta | Utilizar ferramentas no-code ou low-code acessíveis e de rápido aprendizado (ex: Looker Studio, Power BI) e focar em um Produto Mínimo Viável (MVP). |
| **4. Inconsistência ao cruzar dados de fontes diferentes (clima x casos)** | Médio | Alta | Realizar uma etapa de limpeza rigorosa de dados e documentar quaisquer limitações metodológicas no relatório final do projeto. |
| **5. Imprevistos de tempo do aluno/equipe para finalizar as tarefas** | Alto | Média | Seguir rigorosamente o cronograma estabelecido semanalmente, focando primeiramente nos itens descritos em "O que entra" no escopo. |
