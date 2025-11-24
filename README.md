# SQL Prática – SQLite

Este repositório contém exercícios e estudos de SQL usando **SQLiteOnline (sqliteonline.com)**.  
Aqui você encontrará consultas simples, intermediárias e avançadas, além de relatórios e dashboards construídos somente com SQL.

---

## 📚 Conteúdos Praticados

- SELECT  
- WHERE  
- JOIN (INNER / LEFT)  
- GROUP BY + HAVING  
- ORDER BY  
- LIMIT  
- Subqueries  
- CTE (WITH)  
- Relatórios/Análises

---

## 🗄 Estrutura do Repositório

```
exercicios/ → exercícios individuais com soluções  
relatorios/ → consultas completas estilo dashboard  
banco.sql → script para criação do banco  
README.md → documentação e explicações  
```

---

# 🏗 Banco de Dados Usado

As tabelas **clientes** e **pedidos** simulam um ambiente de vendas.

## ✔ Script de criação (`banco.sql`)
```sql
CREATE TABLE clientes (
    id INTEGER PRIMARY KEY,
    nome TEXT,
    cidade TEXT
);

CREATE TABLE pedidos (
    id INTEGER PRIMARY KEY,
    cliente_id INTEGER,
    valor REAL,
    data DATE,
    FOREIGN KEY(cliente_id) REFERENCES clientes(id)
);

INSERT INTO clientes VALUES
(1, 'Ana', 'São Paulo'),
(2, 'Bruno', 'Rio de Janeiro'),
(3, 'Carlos', 'Belo Horizonte'),
(4, 'Daniela', 'São Paulo');

INSERT INTO pedidos VALUES
(1, 1, 150.50, '2024-01-10'),
(2, 1, 299.99, '2024-02-05'),
(3, 2, 75.00,  '2024-02-20'),
(4, 3, 623.00, '2024-03-01'),
(5, 4, 120.00, '2024-03-15'),
(6, 4, 980.00, '2024-04-02');
```

---

# 🧪 Exercícios (com explicação)

## 📌 1. Pedidos maiores que 200 (`01_select_where.sql`)
```sql
SELECT *
FROM pedidos
WHERE valor > 200;
```

---

## 📌 2. Clientes da cidade de São Paulo
```sql
SELECT *
FROM clientes
WHERE cidade = 'São Paulo';
```

---

## 📌 3. INNER JOIN – pedidos com nomes dos clientes
```sql
SELECT 
    p.id AS pedido,
    c.nome AS cliente,
    p.valor
FROM pedidos p
INNER JOIN clientes c ON c.id = p.cliente_id;
```

---

## 📌 4. LEFT JOIN – clientes sem pedidos
```sql
SELECT 
    c.nome,
    p.id AS pedido
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.id IS NULL;
```

---

## 📌 5. Total de pedidos por cliente
```sql
SELECT 
    c.nome,
    SUM(p.valor) AS total_gasto
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.id
ORDER BY total_gasto DESC;
```

---

## 📌 6. Top 3 pedidos
```sql
SELECT *
FROM pedidos
ORDER BY valor DESC
LIMIT 3;
```

---

## 📌 7. Clientes com compras acima da média
```sql
SELECT 
    c.nome,
    SUM(p.valor) AS total_cliente
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.id
HAVING total_cliente > (
    SELECT AVG(valor) FROM pedidos
);
```

---

# 📊 Relatórios Avançados

## ✔ Total gasto por cliente
```sql
SELECT 
    c.nome,
    SUM(p.valor) AS total_gasto
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.id
ORDER BY total_gasto DESC;
```

---

## ✔ Vendas por cidade
```sql
SELECT 
    c.cidade,
    SUM(p.valor) AS total_vendas
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.cidade
ORDER BY total_vendas DESC;
```

---

## ✔ Top clientes por faturamento
```sql
SELECT 
    c.nome,
    COUNT(p.id) AS qtd_pedidos,
    SUM(p.valor) AS total
FROM clientes c
JOIN pedidos p ON p.cliente_id = c.id
GROUP BY c.id
ORDER BY total DESC
LIMIT 5;
```

---

## ✔ Pedidos acima da média por cliente
```sql
SELECT
    c.nome,
    p.id AS pedido,
    p.valor
FROM pedidos p
JOIN clientes c ON c.id = p.cliente_id
WHERE p.valor > (
    SELECT AVG(valor)
    FROM pedidos p2
    WHERE p2.cliente_id = p.cliente_id
);
```

---

## ✔ Vendas mensais
```sql
WITH por_mes AS (
    SELECT 
        strftime('%Y-%m', data) AS mes,
        SUM(valor) AS total_mes
    FROM pedidos
    GROUP BY mes
)
SELECT *
FROM por_mes
ORDER BY mes;
```

---


