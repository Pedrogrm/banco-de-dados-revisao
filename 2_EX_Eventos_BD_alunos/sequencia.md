# Resolução

## Implementação SQL

### 1. Criação do Banco de Dados

```sql
-- Criação do banco de dados
CREATE DATABASE eventos CHARACTER SET utf8mb4;
Use eventos:

-- Seleciona o banco de dados

```

### 2. Criação das Tabelas

```sql
-- Tabela de palestrantes
CREATE TABLE palestrantes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    especialidade VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
);






-- Tabela de eventos
CREATE TABLE eventos (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    date_evento DATE,
    local VARCHAR(100) NOT NULL,
    capacidade INT,
    Palestrante_id INT,
);







-- Tabela de inscrições
CREATE TABLE inscricoes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    eventos_id INT NOT NULL,
    nome_participante VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    data_inscricao TIMESTAMP,
    presente TINYINT,
    
);


-- Criando relação entre tabelas com chave estrangeira
ALTER TABLE inscricoes
ADD CONSTRAINT fk_inscricoes_eventos
FOREIGN KEY (eventos_id) REFERENCES eventos(id);

-- Criando relação entre tabelas com chave estrangeira
ALTER TABLE eventos
ADD CONSTRAINT fk_eventos_palestrantes
FOREIGN KEY (palestrantes_id) REFERENCES palestrantes(id);




```

### 3. Inserção de Dados Iniciais

```sql
-- Inserir palestrantes
INSERT INTO palestrantes (nome, especialidade, email)
VALUES ('Maria Silva, especialista em Inteligência Artificial, email: maria@exemplo.com');

INSERT INTO palestrantes (nome, especialidade, email)
VALUES ('João Santos, especialista em Marketing Digital, email: joao@exemplo.com');


-- Inserir eventos
INSERT INTO eventos (titulo, date_evento, local, capacidade, Palestrante_id)
VALUES ('Workshop de IA, data: 15/11/2023, local: Auditório Principal, capacidade: 100 pessoas, 1');

INSERT INTO eventos (titulo, date_evento, local, capacidade, Palestrante_id)
VALUES ('Conferência de Marketing, data: 10/12/2023, local: Sala de Convenções, capacidade: 200 pessoas, 2');


-- Inserir algumas inscrições
INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('1, Carlos Oliveira, email :carlos@email.com, data: 15/11/2023');

INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('1, Ana Souza, email :ana@email.com, data: 15/11/2023');

INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('2, Bruno Lima, email: bruno@email.com, date: 10/12/2023');

```

### 4. View para Consulta

```sql
-- View para listar eventos com detalhes
CREATE VIEW vw_eventos_detalhados AS
SELECT 
    e.id,
    e.titulo,
    e.data_evento,
    e.local,
    e.capacidade,
    p.nome AS palestrante,
    p.especialidade,
    COUNT(i.id) AS total_inscritos,
    e.capacidade - COUNT(i.id) AS vagas_disponiveis
FROM 
    eventos e
LEFT JOIN 
    palestrantes p ON e.palestrante_id = p.id
LEFT JOIN 
    inscricoes i ON e.id = i.evento_id
GROUP BY 
    e.id, e.titulo, e.data_evento, e.local, e.capacidade, p.nome, p.especialidade;

-- Consultar a view
SELECT * FROM vw_eventos_detalhados;
```

### 5. Consultas Úteis

```sql
-- 1. Listar todos os eventos com suas respectivas informações
    SHOW FULL TABLES WHERE Table_Type = 'VIEW';


-- 2. Mostrar apenas eventos com vagas disponíveis (usando a view criada)
SELECT id, titulo, data_evento, local, vagas_disponiveis 
FROM vw_eventos_detalhados
WHERE vagas_disponiveis > 0;

-- 3. Listar participantes inscritos em um evento específico



```
