# Fase 3: Plano de Execução da Avaliação

## 1. Introdução

O objetivo desta fase é especificar o **Plano de Execução** da avaliação de qualidade do [AcheiUnB](https://github.com/unb-mds/2024-2-AcheiUnB), operacionalizando as decisões tomadas na [Fase 2](/fases/fase2). A Fase 3 traduz os [Objetivos de Medição](/fases/fase2#_2-objetivos-de-medição), as [Questões](/fases/fase2#_3-questões-e-hipóteses-de-medição) e, sobretudo, as [Métricas](/fases/fase2#_4-seleção-de-métricas) definidas pela equipe em um conjunto de procedimentos reprodutíveis, descrevendo o **método de avaliação**, os **recursos e o ambiente** necessários e o **cronograma** de execução que precederá a [Fase 4](/fases/fase4).

## 2. Definição do Método e Instruções

### 2.1 Visão Geral do Método

Considerando o [Escopo da Avaliação](/fases/fase1#_61-escopo-da-avalia%c3%a7%c3%a3o) declarado na Fase 1 em que o sistema **não está acessível em produção**, o método adotado é o de **Análise Estática e Inspeção Documental** sobre o repositório público do AcheiUnB. Esta escolha mantém o alinhamento metodológico já justificado na [Seção 7 da Fase 2](/fases/fase2#_7-consistência-entre-fase-1-e-fase-2) e garante a viabilidade de obtenção das medidas sem necessidade de execução em ambiente produtivo.

O processo de medição segue três princípios:

1. **Reprodutibilidade**: todos os comandos são versionáveis, executáveis em ambiente padronizado e operam sobre um *snapshot* congelado do repositório (commit SHA registrado no momento da execução).
2. **Rastreabilidade**: cada execução gera artefatos brutos (logs, relatórios JSON/XML, *screenshots*) armazenados no diretório `/dados-brutos/` do repositório da disciplina, com referência cruzada à métrica correspondente.
3. **Independência**: o método não exige conhecimento prévio do domínio de aplicação do AcheiUnB; o avaliador necessita apenas de familiaridade com ferramentas de análise estática padrão de mercado.

### 2.2 Procedimento por Métrica

A **Tabela 1** apresenta o procedimento operacional para cada uma das seis métricas estabelecidas na Fase 2, indicando ferramenta, comando de coleta e artefato gerado.

**Tabela 1:** Procedimento operacional por métrica.

| ID | Métrica | Ferramenta | Comando / Procedimento | Artefato de Saída |
| :--- | :--- | :--- | :--- | :--- |
| **M1.1** | Cobertura de Testes | `coverage` + `pytest` + Codecov | A partir da raiz do repositório: `cd API && coverage run -m pytest && coverage report -m && coverage xml -o ../coverage.xml`. Conferir também o relatório do Codecov para o repositório (cuja meta interna declarada em `codecov.yml` é de 90%). | `coverage.xml` + captura do relatório Codecov |
| **M1.2** | Conformidade da Infraestrutura | Inspeção manual + `docker compose config` | Aplicar o **Checklist de Infraestrutura** (Seção 2.3) sobre o repositório; validar sintaxe dos arquivos *Compose* em `API/` com `docker compose -f API/docker-compose.yml config -q` e `docker compose -f API/docker-compose.prod.yml config -q`. | Planilha preenchida do *checklist* + log do *compose* |
| **M2.1** | Densidade de Violações de Estilo | `ruff` + `black` (backend) + `prettier` (frontend) + `cloc` | `ruff check API/ --output-format=json -o ruff.json`; `black --check API/ 2> black.txt`; `npx prettier --check "web/src/**/*.{js,vue,css,html}" > prettier.txt 2>&1`; `cloc API/ web/src/ --json --out=cloc.json`. Cálculo: `(violações totais / KLOC total) × 1000`. | `ruff.json` + `black.txt` + `prettier.txt` + `cloc.json` |
| **M2.2** | Grau de Desacoplamento Arquitetural | Inspeção da árvore de diretórios + `madge` | Aplicar o **Checklist de Desacoplamento** (Seção 2.3) sobre o repositório; verificar dependências cíclicas no frontend com `npx madge --circular --extensions js,vue web/src/`. | Planilha preenchida do *checklist* + saída do `madge` |
| **M3.1** | Taxa de Credenciais *Hardcoded* | `gitleaks` + verificação manual com `grep` | `gitleaks detect --source . --no-git --report-format json --report-path gitleaks.json`; revisão manual de falsos positivos (especialmente em `.env.production`/`.env.vm` do frontend). Classificar achados por categoria (código de produção, *logs*/*prints*, testes/*mocks*). Opcionalmente, leverage do relatório **Bandit** já gerado pela pipeline de CI (`bandit_security_report.html`) como evidência complementar. | `gitleaks.json` + planilha de classificação |
| **M3.2** | Percentual de Rotas Protegidas | Inspeção de `urls.py` + `views.py` | Inventariar todos os *endpoints* a partir dos arquivos `API/AcheiUnB/urls.py`, `API/users/urls.py`, `API/chat/urls.py`, `API/reports/urls.py` e `API/support/urls.py`; classificar quais manipulam dados sensíveis; para cada um, verificar nas *views* correspondentes a presença de `@permission_classes`, `IsAuthenticated`, ou autenticação via `rest_framework_simplejwt`/MSAL. Cálculo: `(rotas protegidas / rotas sensíveis totais) × 100`. | Planilha de inventário de rotas |

### 2.3 *Checklists* Auxiliares

Para garantir objetividade nas métricas baseadas em inspeção (M1.2 e M2.2), os *checklists* abaixo formalizam os itens avaliados.

**Tabela 2:** *Checklist* de Conformidade da Infraestrutura (M1.2).

| # | Item Avaliado | Conforme? |
| :--- | :--- | :---: |
| 1 | Presença de `docker-compose.yml` no diretório `API/` (raiz do backend) | Sim |
| 2 | Presença de `docker-compose.prod.yml` para ambiente produtivo | Sim |
| 3 | Declaração explícita dos serviços essenciais (`web`, `db`, `celery`, `celery-beat`, `redis`) | Sim |
| 4 | Presença de `Dockerfile` para o backend (preferencialmente *multistage*) | Sim |
| 5 | Presença de configuração do Celery (`celery.py` ou equivalente em `API/AcheiUnB/`) | Sim |
| 6 | Declaração de *healthchecks* nos serviços críticos | Não |
| 7 | Presença de `.env.example` documentando variáveis necessárias | Não |
| 8 | Documentação de *build* e execução no `README.md` (ou `Makefile`) | Sim |
| 9 | Validação sintática do *Compose* (`docker compose config -q` retorna 0) | Não |

**Tabela 3:** *Checklist* de Desacoplamento Arquitetural (M2.2).

| # | Item Avaliado | Conforme? |
| :--- | :--- | :---: |
| 1 | Diretórios `API/` (backend) e `web/` (frontend) isolados na raiz do repositório | Sim |
| 2 | Inexistência de *imports* cruzados entre `API/` e `web/` | Sim |
| 3 | `package.json` exclusivo em `web/`; `requirements.txt` e `pyproject.toml` exclusivos em `API/` | Sim |
| 4 | Pipeline de CI com etapas segregadas por camada (testes/lint backend; build frontend) | Parcialmente |
| 5 | Ausência de código de backend dentro de `web/` (e vice-versa, exceto *template* de servir o SPA) | Sim |
| 6 | Inexistência de dependências cíclicas no frontend (`madge --circular` retorna vazio) | Sim |

> No item 4, na validação da etapa de Continuous Integration temos uma separação clara entre backend e frontend, porém não foi encontrado a etapa de build do frontend, por isso o "Parcialmente".

### 2.4 Tratamento de Casos Especiais

- **Métricas não calculáveis**: caso uma ferramenta não retorne dados (ex.: ausência total de testes inviabilizando M1.1), a métrica recebe o **Nível 1 (Insuficiente)** com justificativa registrada.
- **Falsos positivos**: na M3.1, achados claramente falsos (ex.: valores em arquivos `.example`) são removidos manualmente, com registro da decisão em planilha auditável.
- **Versões divergentes**: o ponto de referência oficial é o *commit* congelado declarado na Seção 3.3.

## 3. Especificação dos Recursos e Ambiente de Avaliação

### 3.1 Requisitos de Hardware

| Recurso | Especificação Mínima |
| :--- | :--- |
| Processador | x86_64 *dual-core* 2.0 GHz |
| Memória RAM | 8 GB |
| Armazenamento | 10 GB livres |
| Conexão | Acesso à internet para *clone* do repositório e instalação de dependências |

### 3.2 Requisitos de Software

As versões abaixo são fixadas para garantir reprodutibilidade entre execuções e entre avaliadores.

**Tabela 4:** Versões fixadas das ferramentas de avaliação.

| Ferramenta | Versão | Finalidade |
| :--- | :--- | :--- |
| Sistema operacional | Ubuntu 22.04 LTS ou WSL2 equivalente | Ambiente base de execução (compatível com a pipeline CI do projeto) |
| Python | 3.10.x | Versão utilizada pela pipeline CI do AcheiUnB (`.github/workflows/pipeline-ci.yaml`) |
| Node.js | 20 LTS | Execução do frontend Vue 3 + Vite e das ferramentas `prettier`/`madge` |
| Docker Engine | 24.x ou superior | Validação dos artefatos *Compose* em `API/` |
| `coverage` + `pytest` + `pytest-django` | versões declaradas em `API/requirements.txt` | Coleta de M1.1 |
| `ruff` | 0.0.287 (conforme `API/requirements.txt`) | Coleta de M2.1 (backend — *linting*) |
| `black` | 24.3.0 (conforme `API/requirements.txt`) | Coleta de M2.1 (backend — verificação de formatação) |
| `prettier` | 3.4.2 (conforme `web/package.json`) | Coleta de M2.1 (frontend — verificação de formatação) |
| `cloc` | 1.98 | Contagem de KLOC para normalização de M2.1 |
| `madge` | 8.x | Verificação de ciclos para M2.2 (frontend) |
| `gitleaks` | 8.x | Coleta de M3.1 |
| `bandit` / `safety` | versões da pipeline CI | Evidência complementar para M3.1 (já executadas no estágio `security-scan` do CI) |

### 3.3 Massa de Dados

A massa de dados da avaliação consiste no **código-fonte público do AcheiUnB** obtido por *git clone* do repositório oficial. Para garantir reprodutibilidade absoluta, é definido um **commit de referência** que será congelado no início da Fase 4 e registrado no relatório de execução:

```bash
git clone https://github.com/unb-mds/2024-2-AcheiUnB.git
cd 2024-2-AcheiUnB
git checkout e917733
```

Toda a execução posterior, incluindo eventuais re-execuções para fins de auditoria, utilizará exclusivamente este *snapshot*.

### 3.4 Perfil dos Avaliadores

Os avaliadores são membros da equipe Jean Sammet, estudantes de Engenharia de Software da UnB/FCTE, com:

- Conhecimento intermediário de Python (Django + DRF) e JavaScript (Vue 3 + Vite);
- Familiaridade com Docker e arquitetura cliente-servidor;
- Leitura crítica de relatórios de CI/CD (GitHub Actions);
- Não há exigência de conhecimento prévio sobre o domínio de aplicação do AcheiUnB, uma vez que as métricas escolhidas são de natureza estrutural e não funcional.

## 4. Cronograma de Avaliação

O cronograma abaixo distribui as atividades da Fase 4 ao longo de **sete dias úteis**, agrupando as métricas por característica de qualidade para maximizar foco e minimizar troca de contexto. Cada bloco de coleta gera dados brutos que serão consolidados antes da etapa de análise.

**Tabela 5:** Cronograma de execução da Fase 4.

| Dia | Atividade | Métricas Envolvidas | Responsáveis | Entregável |
| :---: | :--- | :--- | :--- | :--- |
| D1 | Preparação do ambiente, *clone* do repositório, registro do *commit* de referência | — | Toda a equipe | `commit-referencia.txt` |
| D2 | Coleta das métricas de **Confiabilidade** | M1.1, M1.2 | Todos | `coverage.xml`, *checklist* M1.2 |
| D3 | Coleta das métricas de **Manutenibilidade** | M2.1, M2.2 | Todos | `ruff.json`, `black.txt`, `prettier.txt`, `cloc.json`, *checklist* M2.2 |
| D4 | Coleta das métricas de **Segurança** | M3.1, M3.2 | Todos | `gitleaks.json`, inventário de rotas |
| D5 | Consolidação dos dados brutos no repositório (`/dados-brutos/`) e validação cruzada entre duplas | Todas | Toda a equipe | Diretório `/dados-brutos/` organizado |
| D6 | Cálculo dos níveis de pontuação por métrica, IQ por característica e IQG global | Todas | Toda a equipe | Planilha consolidada de resultados |
| D7 | Redação do relatório da Fase 4, revisão por pares, *release* | Todas | Toda a equipe | `fase4.md` + *release* EU2 |

## 5. Consistência entre Fase 2 e Fase 3

A **Tabela 6** explicita a rastreabilidade entre cada métrica estabelecida na [Seção 4 da Fase 2](/fases/fase2#_4-seleção-de-métricas) e o instrumento operacional correspondente desta Fase 3, demonstrando que o Plano de Avaliação atende integralmente às exigências de medição previamente definidas.

**Tabela 6:** Rastreabilidade Fase 2 → Fase 3.

| F2: Métrica | F2: Critério de Julgamento | F3: Ferramenta | F3: Procedimento | Justificativa de Coerência |
| :--- | :--- | :--- | :--- | :--- |
| **M1.1** | Faixas de 0 a ≥80% de cobertura | `coverage` + `pytest` + Codecov | Seção 2.2 — `coverage run -m pytest` + relatório Codecov | Mede exatamente o que F2 exige: percentual de linhas cobertas por testes automatizados, replicando o mesmo método já adotado pela pipeline CI do projeto |
| **M1.2** | Faixas de 0 a 100% dos itens do *checklist* | Inspeção + `docker compose config` | Seção 2.2 + Tabela 2 (9 itens) | A Fase 2 definiu o uso de *checklist*; a Fase 3 formaliza seus itens, eliminando subjetividade |
| **M2.1** | Faixas de erros por KLOC | `ruff` + `black` + `prettier` + `cloc` | Seção 2.2 — execução combinada e normalização por KLOC | Reproduz a unidade de medida (defeitos/KLOC) exigida em F2, utilizando exatamente as ferramentas de *linting*/formatação adotadas pelo projeto (`ruff`/`black` no backend e `prettier` no frontend) |
| **M2.2** | Faixas de separação arquitetural | Inspeção + `madge` | Seção 2.2 + Tabela 3 (6 itens) | A Fase 2 definiu o uso de *checklist*; a Fase 3 formaliza seus itens, com apoio do `madge` para detecção objetiva de ciclos no frontend Vue |
| **M3.1** | Faixas por número e localização de ocorrências | `gitleaks` + revisão manual (+ `bandit` da CI) | Seção 2.2 — classificação por categoria (produção / *logs* / testes) | A ferramenta endereça diretamente as três categorias previstas nos níveis de pontuação da F2; relatórios Bandit/Safety da CI servem como evidência complementar |
| **M3.2** | Faixas de percentual de rotas protegidas | Inspeção dos `urls.py` + `views.py` | Seção 2.2 — inventário dos cinco arquivos `urls.py` e verificação de `@permission_classes`/JWT/MSAL | Reproduz a fórmula percentual definida na F2 sobre o conjunto real de *endpoints* da API |

Como pode-se ver, cada métrica da Fase 2 foi transformada em um procedimento operacional específico na Fase 3, preservando a mesma unidade de análise, o mesmo foco de qualidade e o mesmo critério de julgamento.

Adicionalmente, o método de análise estática, os recursos de ferramentas de mercado e o cronograma de 7 dias são integralmente compatíveis com o [Escopo da Avaliação declarado na Fase 1](/fases/fase1#_61-escopo-da-avalia%c3%a7%c3%a3o) e reforçado na [Seção 7 da Fase 2](/fases/fase2#_7-consistência-entre-fase-1-e-fase-2), garantindo **rastreabilidade total** das fases do projeto.

## 6. Uso de Inteligência Artificial

Em conformidade com a exigência e a ética da disciplina de Qualidade de Software, certificamos que a Inteligência Artificial (Modelo LLM *Antigravity/Gemini/Claude*) foi utilizada no papel de **Assistente de Redação e de Estruturação**, auxiliando na organização das tabelas de procedimentos e na revisão gramatical do texto e como **Revisor do Repositório** do AcheiUnB como auxiliar para verificar as métricas de segurança.

A seleção das ferramentas de avaliação, a definição dos itens dos *checklists* (Tabelas 2 e 3), as versões fixadas das ferramentas e a justificativa de coerência apresentada na Seção 5 foram pensadas, discutidas e validadas inteiramente por intervenção humana dos membros da equipe. Todo o conteúdo gerado pela IA foi submetido à leitura crítica individual por cada autor, com adaptação ao contexto específico do AcheiUnB realizada exclusivamente pelos integrantes do grupo. Nenhuma figura inadequada, ícone excessivo ou conclusão de dados foi gerada artificialmente.

---

## Bibliografia

- **ACHEIUNB.** Repositório do Projeto Analisado. Disponível em: <https://github.com/unb-mds/2024-2-AcheiUnB>. Acesso em: 11 jun. 2026.

- **ISO/IEC.** ISO/IEC 25040:2011 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — Evaluation process. International Organization for Standardization, 2011.

- **ISO/IEC.** ISO/IEC 25010:2011 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models. International Organization for Standardization, 2011.

- **BASILI, V. R. et al.** The Goal/Question/Metric Paradigm. *Fraunhofer IESE*, 2003.

- **GITLEAKS.** *Open-source secret scanner*. Disponível em: <https://github.com/gitleaks/gitleaks>. Acesso em: 11 jun. 2026.

- **MADGE.** *Module dependency graph generator*. Disponível em: <https://github.com/pahen/madge>. Acesso em: 11 jun. 2026.

---

## Histórico de Versão

| Versão |    Data    | Descrição                                                                                  | Autor           |
| :----: | :--------: | ------------------------------------------------------------------------------------------ | --------------- |
|  1.0   | 10/06/2026 | Criação da estrutura base do documento Fase 3                                              | [Euller Júlio](https://github.com/potatoyz908) |
|  1.1   | 10/06/2026 | Popula documento da Fase 3                                              | [Diogo Oliveira](https://github.com/Diogo-Olivv) |
|  1.2   | 12/06/2026 | Correções de formatação e informações e reforço de algumas ideias, conforme  especificações de entrega | [João Nascimento](https://github.com/JMPNascimento) |