# CryptoWallet Pro: Modelagem e Implementação de Banco de Dados

Este projeto implementa o modelo relacional do sistema **"CryptoWallet Pro"** utilizando DDL (Data Definition Language) e DML (Data Manipulation Language) em um ambiente MySQL 8.0, replicável no DB Fiddle. 

O objetivo é demonstrar a criação de tabelas, inserção de dados de exemplo e a execução de consultas SQL para gerenciamento de usuários, carteiras, criptomoedas e transações.


## Estrutura do Banco de Dados (Diagrama ER)

O sistema "CryptoWallet Pro" é modelado com base no seguinte Diagrama de Entidade-Relacionamento (ER):

![Diagrama Entidade-Relacionamento](Diagrama%20Entidade-Relacionamento.png)


## Descrição das Tabelas e Colunas

Abaixo, detalhamos cada tabela e suas respectivas colunas, que compõem o esquema do banco de dados do CryptoWallet Pro:

### `Usuario`
Armazena informações sobre os usuários do sistema.

| Coluna      | Tipo         | Descrição                                         |
| :---------- | :----------- | :------------------------------------------------ |
| `usuario_id`| `INT`        | Chave primária, identificador único do usuário.   |
| `nome`      | `VARCHAR(100)`| Nome completo do usuário.                         |
| `email`     | `VARCHAR(150)`| Endereço de e-mail do usuário (deve ser único).   |
| `criado_em` | `DATETIME`   | Data e hora de criação do registro do usuário.    |

### `Carteira`
Representa as carteiras de criptomoedas de cada usuário.

| Coluna       | Tipo          | Descrição                                                 |
| :----------- | :------------ | :-------------------------------------------------------- |
| `carteira_id`| `INT`         | Chave primária, identificador único da carteira.          |
| `usuario_id` | `INT`         | Chave estrangeira referenciando o `usuario_id` da tabela `Usuario`, indicando a qual usuário a carteira pertence. |
| `nome`       | `VARCHAR(100)`| Nome descritivo da carteira.                              |
| `saldo`      | `DECIMAL(18,8)`| Saldo atual da carteira em termos de valor total de criptomoedas (decimal com 18 dígitos no total e 8 após a vírgula). |
| `criado_em`  | `DATETIME`    | Data e hora de criação da carteira.                       |

### `Criptomoeda`
Contém a lista de criptomoedas suportadas pelo sistema.

| Coluna      | Tipo         | Descrição                                               |
| :---------- | :----------- | :------------------------------------------------------ |
| `cripto_id` | `INT`        | Chave primária, identificador único da criptomoeda.     |
| `nome`      | `VARCHAR(50)`| Nome completo da criptomoeda (ex: Bitcoin, Ethereum).   |
| `simbolo`   | `VARCHAR(10)`| Símbolo da criptomoeda (ex: BTC, ETH) - deve ser único. |

### `Transacao`
Registra todas as transações realizadas nas carteiras.

| Coluna          | Tipo           | Descrição                                                                                                             |
| :-------------- | :------------- | :-------------------------------------------------------------------------------------------------------------------- |
| `transacao_id`  | `INT`          | Chave primária, identificador único da transação.                                                                     |
| `carteira_id`   | `INT`          | Chave estrangeira referenciando o `carteira_id` da tabela `Carteira`, indicando em qual carteira a transação ocorreu. |
| `cripto_id`     | `INT`          | Chave estrangeira referenciando o `cripto_id` da tabela `Criptomoeda`, indicando qual criptomoeda foi transacionada.  |
| `quantidade`    | `DECIMAL(18,8)`| Quantidade da criptomoeda envolvida na transação.                                                                     |
| `valor_total`   | `DECIMAL(18,2)`| Valor total da transação em moeda local.                                                                              |
| `data_transacao`| `DATETIME`     | Data e hora em que a transação foi realizada.                                                                         |

---

## Como Reproduzir o Ambiente no DB Fiddle

Para replicar o ambiente do banco de dados no DB Fiddle (MySQL 8.0), siga os passos abaixo:

1.  Acesse o DB Fiddle: [https://www.db-fiddle.com/f/qvJh2UAgpn52eaYoAqZiw2/1](https://www.db-fiddle.com/f/qvJh2UAgpn52eaYoAqZiw2/1)
2.  No campo **"Schema SQL"**, copie e cole todo o código DDL (Data Definition Language) e DML (Data Manipulation Language) fornecido abaixo. Este código criará as tabelas e as populará com dados de exemplo.
3.  No campo **"Query SQL"**, copie e cole as consultas SQL de verificação que você deseja executar.
4.  Clique no botão **"Run"** para executar o script.

### Código Completo (Schema SQL)

```sql
-- Atividade B – Modelagem de Banco de Dados (ER) e SQL no DB Fiddle, UC Modelagem de Software, Montanha

-- DDL criação das tabelas
CREATE TABLE Usuario (
  usuario_id   INT AUTO_INCREMENT PRIMARY KEY,
  nome         VARCHAR(100) NOT NULL,
  email        VARCHAR(150) NOT NULL UNIQUE,
  criado_em    DATETIME    NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Carteira (
  carteira_id  INT AUTO_INCREMENT PRIMARY KEY,
  usuario_id   INT NOT NULL,
  nome         VARCHAR(100) NOT NULL,
  saldo        DECIMAL(18,8) NOT NULL DEFAULT 0.0,
  criado_em    DATETIME       NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (usuario_id) REFERENCES Usuario(usuario_id)
);

CREATE TABLE Criptomoeda (
  cripto_id    INT AUTO_INCREMENT PRIMARY KEY,
  nome         VARCHAR(50)  NOT NULL,
  simbolo      VARCHAR(10)  NOT NULL UNIQUE
);

CREATE TABLE Transacao (
  transacao_id    INT AUTO_INCREMENT PRIMARY KEY,
  carteira_id     INT NOT NULL,
  cripto_id       INT NOT NULL,
  quantidade      DECIMAL(18,8) NOT NULL,
  valor_total     DECIMAL(18,2) NOT NULL,
  data_transacao DATETIME      NOT NULL,
  FOREIGN KEY (carteira_id) REFERENCES Carteira(carteira_id),
  FOREIGN KEY (cripto_id)   REFERENCES Criptomoeda(cripto_id)
);

-- DML inserção de dados
INSERT INTO Usuario (nome, email) VALUES
  ('Lucas Andrade', 'lucas.andrade@gmail.com'),
  ('Marina Torres', 'marina.torres@gmail.com'),
  ('Rafael Moreira', 'rafael.moreira@gmail.com');

INSERT INTO Carteira (usuario_id, nome, saldo) VALUES
  (1, 'Cofre Digital', 0.0),
  (1, 'Carteira de Longo Prazo', 0.0),
  (2, 'Carteira Principal', 0.0),
  (3, 'Reservas Estratégicas', 0.0);

INSERT INTO Criptomoeda (nome, simbolo) VALUES
  ('Solana', 'SOL'),
  ('Cardano', 'ADA'),
  ('Litecoin', 'LTC');

INSERT INTO Transacao (carteira_id, cripto_id, quantidade, valor_total, data_transacao) VALUES
  (1, 1, 10.00000000, 900.00, '2025-05-10 10:30:00'),
  (1, 2, 150.00000000, 450.00, '2025-05-11 13:45:00'),
  (2, 1, 5.00000000, 475.00, '2025-05-12 09:20:00'),
  (3, 3, 2.50000000, 350.00, '2025-05-13 11:00:00'),
  (4, 2, 200.00000000, 600.00, '2025-05-14 08:50:00');
```

---

### `Exemplos de Consultas e Resultados`

Aqui estão as consultas de verificação solicitadas e seus respectivos resultados esperados no DB Fiddle:

a) Listar todas as transações de Lucas Andrade (usuário_id = 1), mostrando data, símbolo da criptomoeda, quantidade e valor

Consulta SQL:
```
SELECT t.data_transacao, c.simbolo, t.quantidade, t.valor_total
FROM Transacao AS t
JOIN Carteira AS w ON t.carteira_id = w.carteira_id
JOIN Usuario AS u ON w.usuario_id = u.usuario_id
JOIN Criptomoeda AS c ON t.cripto_id = c.cripto_id
WHERE u.usuario_id = 1
ORDER BY t.data_transacao;
```
Resultado Esperado:

| data_transacao      | simbolo | quantidade   | valor_total  |
| :------------------ | :------ | :----------- | :----------- |
| 2025-05-10 10:30:00 | SOL     | 10.00000000  | 900.00       |
| 2025-05-11 13:45:00 | ADA     | 150.00000000 | 450.00       |


b) Total investido por carteira

Consulta SQL:
```
SELECT w.nome AS carteira, SUM(t.valor_total) AS total_investido
FROM Transacao AS t
JOIN Carteira AS w ON t.carteira_id = w.carteira_id
GROUP BY w.carteira_id;
```
Resultado Esperado:

| carteira                | total_investido |
| :---------------------- | :-------------- |
| Cofre Digital           | 1350.00         |
| Carteira de Longo Prazo | 475.00          |
| Carteira Principal      | 350.00          |
| Reservas Estratégicas   | 600.00          |

c) Quantidade total de cada criptomoeda em todas as carteiras:
Consulta SQL

```
SELECT c.simbolo, SUM(t.quantidade) AS total_quantidade
FROM Transacao AS t
JOIN Criptomoeda AS c ON t.cripto_id = c.cripto_id
GROUP BY c.cripto_id;
```
Resultado Esperado:
| simbolo | total_quantidade |
| :------ | :--------------- |
| SOL     | 15.00000000      |
| ADA     | 350.00000000     |
| LTC     | 2.50000000       |

---

### `Link para o DB Fiddle`

Você pode acessar a implementação completa do banco de dados no DB Fiddle através do link:
https://www.db-fiddle.com/f/qvJh2UAgpn52eaYoAqZiw2/1


