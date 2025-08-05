# Conceitos de banco de dados relacionais e não relacionais

---

## Banco de dados Relacional

É um tipo de banco de dados que organiza os dados em **tabelas**, com **linhas e colunas**, e estabelece **relações entre essas tabelas** através de **chaves primárias e estrangeiras**. Essa estrutura permite acessar e manipular informações de forma eficiente, mantendo a integridade dos dados.

### Exemplo de uso: Sistema de pedidos de uma pizzaria

Um banco de dados relacional pode ser usado em uma **pizzaria** para organizar informações sobre **clientes, pedidos e pizzas** em tabelas relacionadas, como:

- **Clientes** (`id_cliente`, nome, telefone)  
- **Pizzas** (`id_pizza`, sabor, tamanho, preço)  
- **Pedidos** (`id_pedido`, id_cliente, data)  
- **ItensPedido** (`id_pedido`, id_pizza`, quantidade)

Essas tabelas se relacionam por meio de **chaves estrangeiras**. Isso permite:

- Registrar quais clientes fizeram quais pedidos  
- Calcular o valor total de um pedido  
- Analisar os sabores mais pedidos

Essa estrutura relacional torna o sistema organizado, seguro e fácil de consultar com **SQL**.

---

## Banco de  não Relacional

Um **banco de dados não relacional** (ou **NoSQL**) é um tipo de banco de dados que **não usa tabelas tradicionais** para armazenar dados. Em vez disso, ele armazena os dados em **formatos flexíveis**, como:

- **Documentos** (ex: JSON)  
- **Grafos**  
- **Chave-valor**  
- **Colunas**

Esse modelo é ideal para aplicações com **grandes volumes de dados**, **estrutura variável** ou que exigem **alta performance e escalabilidade**.

### Exemplo de uso: Aplicativo de redes sociais

Um banco **NoSQL** (como o **MongoDB**) pode ser usado para armazenar os **perfis dos usuários** em **documentos JSON**, com campos como:

```json
{
  "id": "123",
  "nome": "Lucas Silva",
  "bio": "Apaixonado por fotografia",
  "amigos": ["456", "789"],
  "posts": [
    {
      "data": "2025-08-01",
      "conteudo": "Minha nova câmera chegou!"
    }
  ]
}
```
Como cada usuário pode ter diferentes informações, o formato flexível facilita o armazenamento e a consulta desses dados sem precisar de tabelas fixas.

---

## Conceito de Normalização

A normalização é basicamente uma técnica usada para organizar os dados em um banco de dados de forma mais eficiente e sem redundâncias desnecessárias. O principal objetivo dela é garantir que as informações fiquem organizadas, sem repetição de dados, o que torna o banco de dados mais rápido e fácil de manter.

Ao normalizar, você está basicamente limpando a bagunça de dados duplicados, o que ajuda a evitar problemas como inconsistências ao atualizar ou excluir informações. Além disso, facilita a criação de consultas, já que os dados ficam estruturados de forma lógica.

É como arrumar uma gaveta bagunçada, onde você separa as coisas de maneira que tudo fique no lugar certo e você consiga encontrar o que precisa sem perder tempo.

---

## Exemplo de tabela não normalizada

| Pedido_ID | Cliente_Nome | Cliente_Endereço | Produto_ID | Produto_Nome | Quantidade | Preço_Total |
|-----------|--------------|------------------|------------|--------------|------------|-------------|
| 1         | João Silva   | Rua A, 123       | 10         | Caneta       | 2          | 4,00        |
| 1         | João Silva   | Rua A, 123       | 11         | Lápis        | 1          | 2,00        |
| 2         | Maria Souza  | Av. B, 456       | 10         | Caneta       | 5          | 10,00       |

### Observação

- A tabela está não normalizada porque repete informações como o nome e o endereço do cliente sempre que ele faz um pedido com mais de um produto. Isso causa redundância, o que pode levar a erros, dificuldades para atualizar os dados e desperdício de espaço. A normalização ajuda a organizar melhor essas informações, eliminando repetições e facilitando a manutenção do banco de dados.

---

## Normalização até a 3ª Forma Normal (3FN)

Vamos ver o processo de normalização da tabela **não normalizada** até a **3ª Forma Normal (3FN)**.

### Tabela Inicial (Não Normalizada)

A tabela contém dados redundantes e campos multivalorados.

| Pedido_ID | Cliente_Nome | Cliente_Endereço | Produto_ID | Produto_Nome | Quantidade | Preço_Total |
|-----------|---------------|------------------|------------|--------------|------------|--------------|
| 1         | João Silva    | Rua A, 123       | 10         | Caneta       | 2          | 4,00         |
| 1         | João Silva    | Rua A, 123       | 11         | Lápis        | 1          | 2,00         |
| 2         | Maria Souza   | Av. B, 456       | 10         | Caneta       | 5          | 10,00        |

---

### 1ª Forma Normal (1FN)

A **1ª Forma Normal (1FN)** exige que a tabela não contenha **campos multivalorados**. Devemos garantir que cada campo tenha um único valor atômico por célula.

#### Tabela em 1FN:

| Pedido_ID | Cliente_Nome | Cliente_Endereço | Produto_ID | Produto_Nome | Quantidade | Preço_Total |
|-----------|---------------|------------------|------------|--------------|------------|--------------|
| 1         | João Silva    | Rua A, 123       | 10         | Caneta       | 2          | 4,00         |
| 1         | João Silva    | Rua A, 123       | 11         | Lápis        | 1          | 2,00         |
| 2         | Maria Souza   | Av. B, 456       | 10         | Caneta       | 5          | 10,00        |

---

## 2ª Forma Normal (2FN)

A **2ª Forma Normal (2FN)** exige que a tabela esteja na 1FN e que as **dependências parciais** sejam eliminadas. Ou seja, todos os campos dependem da chave primária inteira.

### Tabelas em 2FN:

#### Tabela `Clientes`

| Cliente_ID | Nome        | Endereço     |
|------------|-------------|--------------|
| 1          | João Silva  | Rua A, 123   |
| 2          | Maria Souza | Av. B, 456   |

---

#### Tabela `Produtos`

| Produto_ID | Nome    |
|------------|---------|
| 10         | Caneta  |
| 11         | Lápis   |

---

#### Tabela `Pedidos`

| Pedido_ID | Cliente_ID |
|-----------|------------|
| 1         | 1          |
| 2         | 2          |

---

#### Tabela `Itens_Pedido`

| Pedido_ID | Produto_ID | Quantidade | Preço_Total |
|-----------|------------|------------|--------------|
| 1         | 10         | 2          | 4,00         |
| 1         | 11         | 1          | 2,00         |
| 2         | 10         | 5          | 10,00        |

---

### 3ª Forma Normal (3FN)

A **3ª Forma Normal (3FN)** exige que a tabela esteja na 2FN e que não haja **dependências transitivas**. Ou seja, não pode haver dependência de atributos não chave em relação a outras colunas não chave.

### Tabelas em 3FN:

As tabelas da 3FN são as mesmas da 2FN, pois já resolvemos as dependências transitivas:

####  Tabela `Clientes`

| Cliente_ID | Nome        | Endereço     |
|------------|-------------|--------------|
| 1          | João Silva  | Rua A, 123   |
| 2          | Maria Souza | Av. B, 456   |

---

####  Tabela `Produtos`

| Produto_ID | Nome    |
|------------|---------|
| 10         | Caneta  |
| 11         | Lápis   |

---

####  Tabela `Pedidos`

| Pedido_ID | Cliente_ID |
|-----------|------------|
| 1         | 1          |
| 2         | 2          |

---

#### Tabela `Itens_Pedido`

| Pedido_ID | Produto_ID | Quantidade | Preço_Total |
|-----------|------------|------------|--------------|
| 1         | 10         | 2          | 4,00         |
| 1         | 11         | 1          | 2,00         |
| 2         | 10         | 5          | 10,00        |

---

### Relacionamentos entre as Tabelas

- **Clientes (1) — (N) Pedidos**: Um cliente pode fazer vários pedidos.
- **Pedidos (1) — (N) Itens_Pedido**: Um pedido pode ter vários itens.
- **Produtos (1) — (N) Itens_Pedido**: Um produto pode aparecer em vários pedidos.

---

## Estrutura Não Relacional (MongoDB)

Abaixo está o exemplo de estrutura equivalente ao modelo relacional apresentado anteriormente, mas agora utilizando o **MongoDB**, um banco de dados não relacional, no formato JSON:

### Exemplo de Documento JSON

```json
{
  "pedido_id": 1,
  "cliente": {
    "nome": "João Silva",
    "endereco": "Rua A, 123"
  },
  "itens": [
    {
      "produto_id": 10,
      "nome": "Caneta",
      "quantidade": 2,
      "preco_total": 4.00
    },
    {
      "produto_id": 11,
      "nome": "Lápis",
      "quantidade": 1,
      "preco_total": 2.00
    }
  ]
}
```
### Explicação sobre a Eliminação da Normalização

- **Dados Encapsulados**:  
  No MongoDB, os dados são armazenados em documentos JSON, agrupando todas as informações relacionadas (cliente e itens do pedido) em um único documento, evitando a necessidade de múltiplas tabelas.

- **Eliminação de Normalização**:  
  Não há necessidade de normalização, pois os dados são armazenados de forma completa e hierárquica no documento. Informações repetitivas, como nome e endereço do cliente, não precisam ser armazenadas separadamente.

- **Flexibilidade**:  
  O modelo não relacional oferece flexibilidade, permitindo que os dados sejam ajustados facilmente conforme as necessidades da aplicação, sem a necessidade de mudanças complexas em tabelas.

- **Desempenho**:  
  A estrutura de documentos melhora o desempenho de leitura, pois os dados relacionados (ex: pedido e itens) estão juntos em um único documento, eliminando a necessidade de joins.

### Quando o Modelo Não Relacional é Mais Adequado

- **Dados Não Estruturados ou Semi-Estruturados**:  
  Ideal para dados que não possuem uma estrutura fixa.

- **Escalabilidade Horizontal**:  
  Quando é necessário distribuir grandes volumes de dados entre vários servidores.

- **Alta Velocidade de Leitura**:  
  Quando o sistema exige alta taxa de leitura e acesso rápido a dados relacionados.
