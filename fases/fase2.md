# Fase 2: Definição da Medição (GQM)

## 1. Objetivos de Medição (Nível Conceitual)

A definição dos objetivos de medição segue o template estruturado do paradigma GQM (Goal, Question, Metric), alinhando-se às três características de qualidade priorizadas na Fase 1 (**Confiabilidade**, **Manutenibilidade** e **Segurança**) e ao propósito de avaliação do sistema **AcheiUnB** por meio de análise estática e documental.

### Objetivo 1: Foco em Confiabilidade
| Elemento GQM | Descrição |
| :--- | :--- |
| **Analisar** | A estrutura técnica, configurações de ambiente (Docker/Celery) e a suíte de testes automatizados do AcheiUnB. |
| **Com o propósito de** | Avaliar |
| **Com relação a** | Confiabilidade (Maturidade, Disponibilidade, Tolerância a falhas, Recuperabilidade). |
| **Do ponto de vista de** | Desenvolvedores e Comunidade acadêmica da UnB (usuários). |
| **No contexto de** | Garantia de estabilidade e funcionamento contínuo do sistema de achados e perdidos da universidade. |

### Objetivo 2: Foco em Manutenibilidade
| Elemento GQM | Descrição |
| :--- | :--- |
| **Analisar** | O código-fonte, a estrutura de diretórios, a configuração de pipeline (CI/CD) e a documentação técnica do AcheiUnB. |
| **Com o propósito de** | Avaliar |
| **Com relação a** | Manutenibilidade (Modularidade, Reusabilidade, Analisabilidade, Modificabilidade, Testabilidade). |
| **Do ponto de vista de** | Equipe de desenvolvimento e futuros mantenedores do projeto open source. |
| **No contexto de** | Evolução contínua e facilidade de manutenção do sistema por equipes rotativas em ambiente acadêmico. |

### Objetivo 3: Foco em Segurança
| Elemento GQM | Descrição |
| :--- | :--- |
| **Analisar** | Os mecanismos de controle de acesso (autenticação MSAL), uso de variáveis de ambiente e restrições do banco de dados do AcheiUnB. |
| **Com o propósito de** | Avaliar |
| **Com relação a** | Segurança (Confidencialidade, Integridade, Ausência de repúdio, Rastreabilidade de uso, Autenticidade). |
| **Do ponto de vista de** | Administração da universidade e estudantes (titulares dos dados). |
| **No contexto de** | Proteção de dados institucionais, identidade estudantil e restrição de acesso exclusivo à comunidade da UnB. |

## 2. Questões (Nível Operacional)



## 3. Hipóteses (Nível Operacional)


## 4. Seleção de Métricas (Nível Quantitativo)



## 5. Níveis de Pontuação e Critérios de Julgamento


## 6. Representação da Hierarquia GQM (Gráfico GQM)


## 7. Consistência entre Fase 1 e Fase 2


---

## Histórico de Versão

| Versão |    Data    | Descrição                                                                                  | Autor           |
| :----: | :--------: | ------------------------------------------------------------------------------------------ | --------------- |
|  1.0   | 10/06/2026 | Criação da estrutura base do documento Fase 2                                              | [Euller Júlio](https://github.com/potatoyz908)         |
|  1.1   | 10/06/2026 | Implementação dos Objetivos de Medição (Nível Conceitual) seguindo o template estruturado GQM | [Euller Júlio](https://github.com/potatoyz908)          |
