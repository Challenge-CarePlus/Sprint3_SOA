# Ecoa Fono

## Integrantes

* André de Sousa Neves – RM 553515
* Caio Sato Tominaga – RM 553633
* Eduardo Brites Coutinho – RM 552943
* Isabela Barcellos – RM 553746
* Thaís Gonçalves Leoncio – RM 553892

---

# Descrição do Projeto

O Ecoa Fono é uma API REST desenvolvida para a disciplina de Arquitetura Orientada a Serviços e Web Services.

O sistema foi criado com o objetivo de recomendar exercícios fonoaudiológicos personalizados com base na faixa etária e no objetivo do usuário, promovendo bem-estar, desenvolvimento da fala, comunicação e exercícios vocais.

O projeto utiliza arquitetura em camadas, persistência em banco MySQL e integração com serviço externo através da API ViaCEP.

---

# Objetivo

O objetivo do Ecoa Fono é disponibilizar uma API REST para cadastro de usuários e recomendação de exercícios fonoaudiológicos personalizados.

O funcionamento da API segue o seguinte fluxo:

1. O usuário é cadastrado no sistema informando nome, e-mail, CEP, faixa etária e objetivo desejado.

2. Durante o cadastro, a API consome o serviço externo ViaCEP para buscar automaticamente o endereço do usuário a partir do CEP informado.

3. O endereço retornado pelo ViaCEP é salvo junto com os dados do usuário, incluindo logradouro, cidade e estado.

4. O usuário pode atualizar suas preferências, alterando sua faixa etária e objetivo.

5. A API possui uma base de exercícios cadastrados no banco de dados, separados por faixa etária e objetivo.

6. Ao solicitar uma sessão, o sistema busca o usuário pelo ID e identifica suas preferências atuais.

7. Com base na faixa etária e no objetivo do usuário, a API seleciona os exercícios correspondentes.

8. A sessão é criada no banco de dados com os exercícios recomendados para aquele perfil.

9. O sistema permite consultar usuários, exercícios e sessões já criadas.

Dessa forma, o projeto aplica conceitos de Arquitetura Orientada a Serviços e Web Services, utilizando endpoints REST, integração com serviço externo, persistência em MySQL, validação de dados, tratamento de exceções e organização em camadas.

---

# Funcionalidades

## Usuários

* Cadastro de usuários
* Consulta de usuário por ID
* Atualização de preferências

## Exercícios

* Listagem de exercícios
* Filtro por faixa etária
* Filtro por objetivo

## Sessões

* Criação de sessões personalizadas
* Associação automática de exercícios
* Consulta de sessões por ID

## Integração Externa

* Consulta automática de endereço via ViaCEP

---

# Tecnologias Utilizadas

* Java 21
* Spring Boot 3.5.6
* Spring Data JPA
* MySQL
* Flyway
* Maven
* Lombok
* Validation API
* ViaCEP API
* Postman

---

# Arquitetura do Projeto

O projeto foi estruturado utilizando arquitetura em camadas.

## Fluxo da Aplicação

```text
                    ┌───────────────────────┐
                    │       Postman         │
                    │ Cliente HTTP/REST API │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │      Controller       │
                    │ Recebe requisições    │
                    │ e retorna respostas   │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │        Service        │
                    │ Regras de negócio     │
                    │ e integrações         │
                    └───────┬─────┬─────────┘
                            │     │
                            │     ▼
                            │  ┌────────────────┐
                            │  │ ViaCEP API     │
                            │  │ Serviço externo│
                            │  └────────────────┘
                            │
                            ▼
                    ┌───────────────────────┐
                    │      Repository       │
                    │ Acesso ao banco       │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │        MySQL          │
                    │ Persistência de dados │
                    └───────────────────────┘
```

---

# Estrutura de Pastas

```text
src/main/java/com/ecoafono

├── config
├── controller
├── domain/model
├── dto
├── enums
├── exception
├── repository
└── service
```

---

# Banco de Dados

O projeto utiliza MySQL como banco de dados principal.

## Criação do banco

```sql
CREATE DATABASE ecoa_fono
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

---

# Flyway

O Flyway foi utilizado para versionamento e criação automática das tabelas do banco de dados.

## Migrations

* V1__create-table-usuarios.sql
* V2__create-table-exercicios.sql
* V3__create-table-sessoes.sql
* V4__create-table-sessao-exercicios.sql
* V5__insert-exercicios-iniciais.sql

---

# Integração Externa

O sistema consome a API ViaCEP para preenchimento automático de endereço do usuário.

## API utilizada

```text
https://viacep.com.br/ws/{cep}/json/
```

---

# Endpoints

## Usuários

### Criar usuário

```http
POST /usuarios
```

### Buscar usuário

```http
GET /usuarios/{id}
```

### Atualizar preferências

```http
PUT /usuarios/{id}/preferencias
```

---

## Exercícios

### Listar exercícios

```http
GET /exercicios
```

### Filtrar exercícios

```http
GET /exercicios?faixaEtaria=ADULTO&objetivo=VOZ
```

---

## Sessões

### Criar sessão

```http
POST /sessoes
```

### Buscar sessão

```http
GET /sessoes/{id}
```

---

# Como Executar o Projeto

## 1. Clonar o repositório

```bash
git clone https://github.com/Challenge-CarePlus/Sprint3_SOA.git
```

---

## 2. Criar o banco MySQL

```sql
CREATE DATABASE ecoa_fono;
```

---

## 3. Configurar o application.properties

```properties
spring.datasource.url=jdbc:mysql://localhost/ecoa_fono
spring.datasource.username=root
spring.datasource.password=sua_senha
```

---

## 4. Executar o projeto

Executar a classe:

```text
EcoafonoApplication.java
```

---

# Exemplos de Requisição

## Criar usuário

```json
{
  "nome": "Maria Oliveira",
  "email": "mariaoliveira@email.com",
  "cep": "01310930",
  "faixaEtaria": "ADOLESCENTE",
  "objetivo": "COMUNICACAO"
}
```

---

## Criar sessão

```json
{
  "idUsuario": 2
}
```

---

# Diagramas

## Diagrama de Entidades

```text
┌──────────────────────────┐
│         Usuario          │
├──────────────────────────┤
│ id                       │
│ nome                     │
│ email                    │
│ cep                      │
│ logradouro               │
│ cidade                   │
│ estado                   │
│ faixaEtaria              │
│ objetivo                 │
└─────────────┬────────────┘
              │ 1
              │
              │
              │ N
┌─────────────▼────────────┐
│         Sessao           │
├──────────────────────────┤
│ id                       │
│ data                     │
│ usuario_id               │
└─────────────┬────────────┘
              │ N
              │
              │
              │ N
┌─────────────▼────────────┐
│       Exercicio          │
├──────────────────────────┤
│ id                       │
│ nome                     │
│ descricao                │
│ instrucao                │
│ faixaEtaria              │
│ objetivo                 │
└──────────────────────────┘
```

---

## Casos de Uso

### Cadastro de Usuário

* Usuário informa seus dados.
* Sistema consulta o ViaCEP.
* Sistema salva o usuário no banco.

### Consulta de Exercícios

* Usuário consulta exercícios disponíveis.
* Sistema filtra por faixa etária e objetivo.

### Atualização de Preferências

* Usuário altera faixa etária e objetivo.

### Criação de Sessão

* Sistema identifica o perfil do usuário.
* Sistema busca exercícios compatíveis.
* Sistema cria sessão personalizada.

### Consulta de Sessão

* Usuário consulta sessões já criadas.

---

# Testes Realizados

Os testes da API foram realizados utilizando Postman.

## Fluxos testados

* Cadastro de usuário
* Consulta de CEP
* Atualização de preferências
* Listagem de exercícios
* Criação de sessões
* Busca de sessões
* Validação de campos obrigatórios
* Tratamento de CEP inválido
* Tratamento de usuário inexistente

---

# Considerações Finais

O projeto Ecoa Fono permitiu aplicar os conceitos de SOA e Web Services através da construção de uma API REST completa, utilizando integração com serviços externos, persistência em banco de dados relacional, tratamento de erros, validações e arquitetura organizada em camadas.
