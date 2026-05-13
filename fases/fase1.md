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