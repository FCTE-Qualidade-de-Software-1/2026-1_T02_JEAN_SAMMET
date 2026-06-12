# Fase 4: Execução, Análise e Julgamento

## 1. Introdução

Nessa última fase do processo de avaliação do produto [AcheiUnB](https://github.com/unb-mds/2024-2-AcheiUnB), serão descritas a obtenção dos dados e a apresentação dos [artefatos](/fases/fase3?id=_22-procedimento-por-m%c3%a9trica) citados de acordo com a Fase 3, além da conversão desses dados em métricas para comparação com os [níveis de pontuação definidos](/fases/fase2?id=_5-n%c3%adveis-de-pontua%c3%a7%c3%a3o-e-crit%c3%a9rios-de-julgamento) na Fase 2 e da resposta aos [objetivos](/fases/fase2?id=_2-objetivos-de-medi%c3%a7%c3%a3o) e às [questões](/fases/fase2?id=_3-quest%c3%b5es-e-hip%c3%b3teses-de-medi%c3%a7%c3%a3o) introduzidas pelo método GQM. Ao final, será justificada a coerência dos resultados com o [propósito](/fases/fase1?id=_4-prop%c3%b3sito-da-avalia%c3%a7%c3%a3o) postulado na Fase 1, bem como discutido o julgamento final da equipe e exibidas as ações concretas de melhoria que a equipe de desenvolvimento poderá realizar.

## 2. Execução (Obtenção das Medidas)

A execução da avaliação foi realizada sobre o *snapshot* congelado do repositório do AcheiUnB, registrado no **commit de referência** (`e91773380c5259007d748d85e998a27362537339`) definido pela equipe. Todas as medições seguiram o método de **análise estática e inspeção documental** estabelecido na Fase 3, com registro dos artefatos brutos em pastas específicas dentro de `dados-brutos`, de forma a garantir reprodutibilidade, rastreabilidade e auditoria dos resultados. 

Para a **M1.1 (Cobertura de Testes)**, foi executado `cd API && coverage run -m pytest && coverage report -m && coverage xml -o ../coverage.xml`, a partir do backend do projeto, para obter o relatório consolidado de cobertura. Como evidências, foram armazenados o XML e o relatório textual do codecov com o valor final da cobertura.

Para a **M1.2 (Conformidade da Infraestrutura)**, a equipe inspecionou os arquivos de containerização e orquestração do backend, incluindo `Dockerfile`, `docker-compose.yml`, `docker-compose.prod.yml` e a integração com Celery. Também foram registrados os resultados da validação do `docker compose config`. Ademais, foi feita a realização da inspeção manual, via checklist da seção 2.3 da fase 3, a qual também recebeu evidências próprias que justificam a presença e ausência de cada item.

Para a **M2.1 (Densidade de Violações de Estilo)**, foram executadas as ferramentas `ruff`, `black`, `prettier` e `cloc` sobre o código-fonte do frontend e backend, com o objetivo de obter o número de violações de estilo e normalizá-lo por KLOC. Foram preservados os relatórios das ferramentas das execuções.

Para a **M2.2 (Grau de Desacoplamento Arquitetural)**, foram coletadas evidências da separação entre frontend e backend, da estrutura modular dos diretórios e da inexistência de dependências circulares no frontend por meio do `madge`. Por fim, houve a realização da inspeção manual, a qual também recebeu evidências próprias que justificam a presença e ausência de cada item.

Para a **M3.1 (Taxa de Credenciais Hardcoded)**, foi executado o `gitleaks` sobre o repositório, com posterior inspeção manual do resultado encontrado. A ocorrência registrada foi documentada com o arquivo bruto, captura de tela e análise contextual do arquivo afetado.

Para a **M3.2 (Percentual de Rotas Protegidas)**, foram inventariadas as rotas da API a partir dos arquivos `urls.py` e `views.py`, com apoio dos relatórios de `permission_classes`, `authentication_classes`, `IsAuthenticated` e `APIView`, registrando o conjunto de rotas consideradas sensíveis e sua proteção por autenticação/autorização.

---

## 3. Dados Brutos (Arquivos e Imagens)

Os artefatos brutos foram organizados por métrica, para facilitar a auditoria e a localização dos resultados. Eles se encontram em uma pasta dedicada na root do projeto e possui a seguinte estrutura adotada:

```text
dados-brutos/
├── M1_1_Cobertura_Testes/
│   ├── coverage_codecov.txt
│   └── coverage.xml
├── M1_2_Infraestrutura/
│   ├── ausencia_env.png
│   ├── celery_imports.txt
│   ├── django_celery_integracao.txt
│   ├── docker_compose_config.txt
│   ├── docker_compose_error.png
│   ├── docker_compose_prod.txt
│   ├── docker-compose.log
│   ├── dockerfile.txt
│   └── shared_tasks.txt
├── M2_1_Padroes_Codigo/
│   ├── black.txt
│   ├── cloc.json
│   ├── prettier.txt
│   └── ruff.json
├── M2_2_Arquitetura_Modularizacao/
│   ├── estrutura_backend.txt
│   ├── estrutura_frontend.txt
│   ├── madge.png
│   └── madge.txt
├── M3_1_Gestao_Segredos/
│   ├── gitleaks.json
│   └── gitleaks.png
└── M3_2_Protecao_Rotas/
    ├── api_view.txt
    ├── apiviews.txt
    ├── authentication_classes.txt
    ├── is_authenticated.txt
    └── permission_classes.txt
```

## 4. Análise e Respostas GQM (Métricas e Questões)

Os dados coletados foram transformados em métricas e comparados com os níveis de pontuação definidos na Fase 2, mantendo rastreabilidade com o plano de execução da Fase 3. A leitura dos resultados considera não apenas os valores numéricos, mas também o contexto arquitetural do AcheiUnB, a natureza das evidências obtidas e a relação entre cada métrica e a questão GQM correspondente.

### 4.1 Resultados por métrica

| Métrica                                    |                                                 Resultado | Nota | Questão |
| ------------------------------------------ | --------------------------------------------------------: | ---: | ------- |
| M1.1 – Cobertura de Testes                 |                                                       43% |    2 | Q1.1    |
| M1.2 – Conformidade da Infraestrutura      |                               6/9 itens conformes (66,7%) |    2 | Q1.2    |
| M2.1 – Densidade de Violações de Estilo    |                                       2,14 violações/KLOC |    4 | Q2.1    |
| M2.2 – Grau de Desacoplamento Arquitetural |                                 0 dependências circulares |    4 | Q2.2    |
| M3.1 – Taxa de Credenciais Hardcoded       |                             1 ocorrência de chave privada |    3 | Q3.1    |
| M3.2 – Percentual de Rotas Protegidas      |                                                      100% |    4 | Q3.2    |

### 4.2 Interpretação por questão

**Q1.1 – Cobertura de testes automatizados**
A cobertura global obtida foi de **43%**, o que confirma a existência de uma suíte de testes, mas ainda em patamar intermediário. O resultado não invalida a estratégia de teste adotada pelo projeto, porém mostra que a base de testes ainda não cobre o sistema de forma ampla o suficiente para sustentar alterações com alto grau de confiança. Em termos práticos, isso significa que o projeto possui validação automatizada, mas ainda não em nível robusto de proteção contra regressões. A cobertura declarada no `codecov.yml` superior a 90%, não se confirmou, o que faz a métrica responder à questão de forma apenas parcialmente satisfatória.

**Q1.2 – Infraestrutura e tolerância a falhas**
A infraestrutura do projeto apresenta uma base clara de containerização e execução: `Dockerfile`, `docker-compose.yml`, `docker-compose.prod.yml`, configuração de Celery e tarefas assíncronas já implementadas no backend. Ainda assim, o checklist consolidado fechou em **6 de 9 itens**, pois faltam evidências ou itens completos em pontos relevantes para reprodutibilidade e robustez operacional, como `healthchecks`, `.env.example` e a validação completa do `docker compose config -q` sem dependência de um arquivo `.env` ausente. Isso mostra que o sistema já tem uma infraestrutura estruturada, mas a completude operacional ainda não atinge o nível máximo. A hipótese de configuração “completa e funcional” não foi confirmada integralmente, embora a base de implantação esteja bem encaminhada.

**Q2.1 – Analisabilidade e padronização**
A análise estática mostrou que o `ruff` não encontrou violações e o `black` não apontou arquivos fora do padrão. O `prettier`, por sua vez, identificou 31 arquivos com ajustes de formatação, mas, quando a quantidade total de violações é normalizada pelo tamanho da base de código analisada, a densidade resultante é de 2,14 violações por KLOC. Esse valor é baixo e, pela escala definida, fica dentro do nível máximo de qualidade. Isso sugere que a padronização de código é forte e que os desvios de estilo encontrados no frontend não comprometem o quadro geral de analisabilidade do projeto. Em outras palavras, o sistema ainda possui pequenos pontos de formatação a corrigir, mas o volume de inconsistências é reduzido em relação ao tamanho total do código.

**Q2.2 – Modularidade e desacoplamento**
A análise do frontend com `madge` processou os arquivos `src` com sucesso e não encontrou dependências circulares. Em conjunto com a organização do repositório, isso evidencia uma separação clara entre backend e frontend. O backend está dividido em apps Django com responsabilidades distintas, enquanto o frontend se organiza em componentes, views, assets e rotas. O resultado mostra que a estrutura modular foi preservada, sem sinais de acoplamento circular que dificultassem manutenção ou evolução. Assim, a hipótese de isolamento físico e baixo acoplamento interno se confirma, e a métrica atinge o nível mais alto da escala.

**Q3.1 – Confidencialidade e exposição de segredos**
O `gitleaks` detectou uma ocorrência de `private-key` em `API/certs/localhost.key`. A análise manual mostrou que essa chave está associada ao certificado `localhost.crt`, montado no `docker-compose.yml` para o serviço Nginx local, e não apareceu no arquivo de produção. Por se tratar apenas um artefato de desenvolvimento local, essa métrica é classficada como nível 3, pois, sgundo os níveis definidos na Fase 2, a presença de uma única credencial exposta, sem evidência de uso em produção, enquadra a métrica no nível 3. Ainda assim, a prática recomendada seria remover a chave do repositório e gerar certificados localmente durante o processo de desenvolvimento. A questão de confidencialidade, portanto, não mostra vazamento de credenciais operacionais de produção, mas mostra como uma falta de atenção poderia comprometer gravemente a segurança do projeto.

**Q3.2 – Autenticidade e proteção de rotas**
A métrica M3.2 avaliou o percentual de rotas que manipulam dados sensíveis e que possuem mecanismos explícitos de autenticação ou autorização. Para isso, foi feito um inventário dos endpoints definidos nos módulos `users`, `chat`, `reports` e `support`, seguido da verificação das classes de permissão nas views do Django REST Framework. Foram consideradas sensíveis as rotas associadas a autenticação, perfis de usuários, mensagens, denúncias, suporte, itens do próprio usuário e operações administrativas, pois todas envolvem algum grau de exposição de dados pessoais ou de controle da aplicação. A análise dos arquivos `permission_classes.txt`, `authentication_classes.txt`, `is_authenticated.txt`, `apiviews.txt` e `api_view.txt` mostrou que todas as 17 rotas sensíveis identificadas em `inventario_rotas.csv` utilizam algum mecanismo de proteção, como `IsAuthenticated`, `IsAuthenticatedOrReadOnly` ou `IsAdminUser`. O resultado de 100% de cobertura de proteção confirma a hipótese da equipe e mostra que o projeto trata a autenticidade e o controle de acesso de forma consistente.

### 4.3 Consolidação por característica

| Característica   | Métricas   |                    IQ |
| ---------------- | ---------- | --------------------: |
| Confiabilidade   | M1.1, M1.2 | (2 + 2) / 2 = **2,0** |
| Manutenibilidade | M2.1, M2.2 | (4 + 4) / 2 = **4,0** |
| Segurança        | M3.1, M3.2 | (3 + 4) / 2 = **3,5** |

A consolidação por característica mostra um quadro bastante equilibrado. A **Confiabilidade** ficou em nível intermediário porque, embora a infraestrutura esteja bem montada, a cobertura de testes ainda é limitada. A **Manutenibilidade** atingiu o melhor resultado possível, apoiada por ausência de ciclos no frontend, estrutura modular do backend e baixa densidade de violações de estilo. A **Segurança** ficou alta, com proteção total das rotas sensíveis e apenas uma ocorrência pontual de chave privada no repositório, tratada como artefato local de desenvolvimento.

### 4.4 Índice Global

O **Índice de Qualidade Global (IQG)** é calculado pela média aritmética simples dos IQs das três características:

IQG = (Confiabilidade + Manutenibilidade + Segurança) / 3

Substituindo os valores obtidos:

IQG = (2,0 + 4,0 + 3,5) / 3

IQG ≈ 3,17

Portanto, o AcheiUnB alcança **IQG ≈ 3,17**, o que o coloca na faixa de **Aceito** segundo a classificação definida na Fase 2. O resultado indica qualidade global boa, com destaque claro para a manutenibilidade e desempenho consistente em segurança, enquanto a principal fragilidade permanece concentrada na cobertura de testes.

## 5. Coerência dos Resultados com o Propósito (Fase 1)

<!-- Mostre como os resultados do julgamento da qualidade atendem diretamente ao propósito e ao uso pretendido estabelecido na Fase 1. -->

## 6. Julgamento, Conclusões e Sugestões de Melhoria

<!-- Apresente o julgamento final embasado nos critérios, traduza as conclusões em ações concretas e justifique sugestões de melhorias para a equipe de desenvolvimento. -->

## 7. Uso de Inteligência Artificial

Em conformidade com a exigência e a ética da disciplina de Qualidade de Software, certificamos que a Inteligência Artificial (Modelo LLM *Antigravity/Gemini/Claude*) foi utilizada no papel de **Assistente de Redação e de Estruturação**, auxiliando na organização das tabelas de procedimentos e na revisão gramatical do texto e como **Revisor do Repositório** do AcheiUnB como auxiliar para verificar as métricas de segurança.

A coleta de dados, sua seguinte interpretação, compreensão dos resultados por eles obtidos, mais o julgamento crítico e revisões realizadas foram de ação única e exclusiva da intervenção humana dos membros da equipe. O uso de IA concentrou-se apenas no auxílio da escrita desse documento e na organização dos dados coletados, devidona quantidade que foi obtido.Todo o conteúdo gerado pela IA foi submetido à leitura crítica individual por cada autor, com adaptação ao contexto específico do AcheiUnB realizada exclusivamente pelos integrantes do grupo. Nenhuma figura inadequada, ícone excessivo ou conclusão de dados foi gerada artificialmente.

---

## Bibliografia

- **ACHEIUNB.** Repositório do Projeto Analisado. Disponível em: <https://github.com/unb-mds/2024-2-AcheiUnB>. Acesso em: 11 jun. 2026.

- **ISO/IEC.** ISO/IEC 25010:2011 — Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models. International Organization for Standardization, 2011.

---

## Histórico de Versão

| Versão |    Data    | Descrição                                                                                  | Autor           |
| :----: | :--------: | ------------------------------------------------------------------------------------------ | --------------- |
|  1.0   | 10/06/2026 | Criação da estrutura base do documento Fase 4                                              | [Euller Júlio](https://github.com/potatoyz908) |
|  1.1   | 11/06/2026 | Adicionar seção de introdução e referências bibliográficas no documento da fase 4                                              | [Willian Silva](https://github.com/Wooo589) |
|  1.2   | 12/06/2026 | Obtenção, execução, análise e julgamentos dos dados obtidos | [João Nascimento](https://github.com/JMPNascimento) |

