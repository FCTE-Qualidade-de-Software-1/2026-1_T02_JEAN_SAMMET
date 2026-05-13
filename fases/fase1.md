# Fase 1

## 1. Requisitante e partes interessadas

### 1.1. Requisitante:

* **Comunidade acadêmica da Universidade de Brasília (UnB)**: com destaque para os estudantes, uma vez que o sistema foi desenvolvido para atender à necessidade de centralizar e facilitar o processo de registro e recuperação de objetos perdidos dentro da universidade

### 1.2. Partes interessadas:

* **Estudantes da UnB:** são os principais usuários, pois utilizam o sistema para registrar itens perdidos ou encontrados.
* **Pessoas que encontram itens perdidos:** interagem com a plataforma para devolver objetos.
* **Equipe mantenedora/desenvolvedores:** responsáveis pela evolução do sistema, correções e manutenção.
* **Administração ou setores de apoio da universidade:** podem se beneficiar de um canal mais organizado para achados e perdidos.

---

## 2. Descrição estruturada do software selecionado

O **AcheiUnB** é uma aplicação web de código aberto voltada ao gerenciamento de achados e perdidos da UnB. O projeto permite registrar e localizar objetos, facilitando o contato entre quem perdeu e quem encontrou o item. 

Sua escolha como objeto de avaliação se deve aos seguintes fatores:

* **Impacto no contexto acadêmico**: O sistema foi desenvolvido para atender uma necessidade real da comunidade universitária da UnB, centralizando o processo de registro e recuperação de objetos perdidos, reduzindo a dependência de grupos informais de mensagens e melhorando a organização desse tipo de comunicação.
* **Arquitetura moderna e projeto open source**: O AcheiUnB utiliza tecnologias amplamente empregadas no desenvolvimento web moderno, como Vue.js, Django e Docker, além de possuir código aberto e documentação pública no GitHub.
* **Integração entre múltiplos componentes**: O sistema possui separação entre frontend e backend, autenticação integrada e serviços containerizados, permitindo avaliar diferentes aspectos de qualidade de software em um contexto realista de aplicações web modernas.

Do ponto de vista técnico, o AcheiUnB utiliza uma arquitetura cliente-servidor com separação entre frontend e backend. O frontend web, desenvolvido em Vue.js, realiza a comunicação com uma API implementada em Python/Django, responsável pelos módulos de usuários, busca e gerenciamento de itens. O sistema também utiliza Docker para containerização dos serviços e possui integração com banco de dados e autenticação baseada em Microsoft MSAL. Além dos módulos principais, o sistema utiliza o Celery para execução de tarefas assíncronas, permitindo melhor gerenciamento de processos em segundo plano.

![Arquitetura do Projeto](../images/arquitetura_AcheiUnB.png)

---

## 3. Classificação do tipo de produto

O AcheiUnB pode ser classificado como um software web de código aberto, de natureza cliente-servidor, com domínio específico voltado a achados e perdidos. Ele é distribuído sob licença MIT e foi implementado com tecnologias típicas de sistemas web modernos, o que reforça seu caráter de produto digital orientado a serviços online.

---

## 4. Propósito da avaliação

O propósito é avaliar a qualidade do AcheiUnB com ênfase nas características de {Nome da característica 1} e {Nome da característica 2}. Ademais, pode-se também verificar se a plataforma cumpre bem seu papel de centralizar o registro e a recuperação de itens perdidos na UnB, oferecendo uma experiência funcional, organizada e adequada ao uso pela comunidade acadêmica. 


## Modelo de qualidade e descrição
---

## 6. Escopo, profundidade e objetos de avaliação

### 6.1. Escopo da avaliação

A avaliação do AcheiUnB será realizada com foco nas características de **Confiabilidade**, **Manutenibilidade** e **Segurança**, conforme definidas no modelo de qualidade adaptado. Considerando que o sistema não está acessível para execução no momento da avaliação, o escopo será direcionado à análise documental e estática dos artefatos disponíveis no repositório.

Dessa forma, serão considerados como parte do escopo:

* Estrutura geral do repositório;
* Separação entre frontend, backend e documentação;
* Arquivos de configuração do ambiente;
* Evidências de autenticação e proteção de credenciais;
* Existência de testes automatizados e configuração de pipeline;
* Organização dos módulos e dependências;
* Evidências técnicas disponíveis na documentação e no código-fonte.

Não serão avaliados neste ciclo:

* Testes funcionais executados em ambiente real;
* Testes de carga ou desempenho;
* Validação com usuários finais;
* Auditoria completa de segurança;
* Avaliação de usabilidade, pois essa característica não pode ser escolhida nesta atividade;
* Disponibilidade real do sistema em produção.

### 6.2. Profundidade da avaliação

A profundidade da avaliação será limitada à análise dos artefatos disponíveis publicamente no repositório do projeto. Portanto, os resultados obtidos não devem ser interpretados como uma avaliação completa do comportamento do sistema em execução, mas sim como uma análise da qualidade observável a partir de sua documentação, código-fonte, configurações e estrutura técnica.

Essa abordagem permite verificar indícios de qualidade relacionados à manutenção, segurança e confiabilidade, mesmo sem acesso à aplicação em funcionamento. Dessa forma, a avaliação mantém coerência com a situação atual do sistema e evita prometer medições que dependam da execução da aplicação.

### 6.3. Objetos de avaliação

| Objeto de avaliação | Relação com a qualidade | Característica associada |
|---|---|---|
| Estrutura de diretórios `/API`, `/web` e `/docs` | Permite verificar separação de responsabilidades e organização do projeto | Manutenibilidade |
| Arquivos de configuração e Docker | Permitem analisar a organização do ambiente e dos serviços | Manutenibilidade / Confiabilidade |
| Configuração de testes e pipeline CI/CD | Indica suporte à verificação automatizada do sistema | Confiabilidade |
| Autenticação via Microsoft MSAL | Evidencia mecanismo de controle de identidade | Segurança |
| Uso de variáveis de ambiente | Indica proteção de credenciais e dados sensíveis | Segurança |
| Organização dos módulos de backend e frontend | Permite observar modularidade e separação de camadas | Manutenibilidade |
| Documentação do projeto | Apoia compreensão, manutenção e evolução do sistema | Manutenibilidade |
| Uso do Celery | Indica suporte a tarefas assíncronas e processos em segundo plano | Confiabilidade |

### 6.4. Limitações da avaliação

A principal limitação desta avaliação é a impossibilidade de acessar o sistema em execução. Por esse motivo, não será possível validar diretamente o comportamento real da aplicação, como login, cadastro de itens, busca, recuperação de objetos ou execução de fluxos completos pelo usuário.

Assim, a avaliação terá caráter documental e estático, com foco na análise dos artefatos disponíveis. Essa limitação será considerada nas fases seguintes, especialmente na definição das métricas e na interpretação dos resultados.


## Histórico de Versão

| Versão | Data | Descrição | Autor |
| :---: | :---: | --- | --- |
| 1.0 | 12/05/2026 | Elaboração inicial das seções 1 ao 4 | Jao Nascimento |
| 1.1 | 12/05/2026 | Elaboração do escopo, profundidade, objetivos de avaliação e Adição do histórico de versão | Euller |
