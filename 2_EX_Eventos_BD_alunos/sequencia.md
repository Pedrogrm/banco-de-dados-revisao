# Resolução

## Implementação SQL

### 1. Criação do Banco de Dados

```sql
-- Criação do banco de dados
CREATE DATABASE palestra CHARACTER SET utf8mb4;
USE palestra;

-- Tabela de palestrantes
CREATE TABLE palestrantes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    especialidade VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- Tabela de eventos
CREATE TABLE eventos (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(200) NOT NULL,
    date_evento DATE,
    local VARCHAR(100) NOT NULL,
    capacidade INT,
    palestrante_id INT
);

-- Tabela de inscrições
CREATE TABLE inscricoes (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    evento_id INT NOT NULL,
    nome_participante VARCHAR(100) NOT NULL,
    email VARCHAR(100),
    data_inscricao TIMESTAMP,
    presente TINYINT
);

-- Chave estrangeira: inscricoes -> eventos
ALTER TABLE inscricoes
ADD CONSTRAINT fk_inscricoes_eventos
FOREIGN KEY (evento_id) REFERENCES eventos(id);

-- Chave estrangeira: eventos -> palestrantes
ALTER TABLE eventos
ADD CONSTRAINT fk_eventos_palestrantes
FOREIGN KEY (palestrante_id) REFERENCES palestrantes(id);

```

### 3. Inserção de Dados Iniciais

```sql
-- Inserir palestrantes
INSERT INTO palestrantes (nome, especialidade, email)
VALUES ('Maria Silva', 'especialista em Inteligência Artificial', 'maria@exemplo.com');

INSERT INTO palestrantes (nome, especialidade, email)
VALUES ('João Santos', 'especialista em Marketing Digital', 'joao@exemplo.com');


-- Inserir eventos
INSERT INTO eventos (titulo, date_evento, local, capacidade, Palestrante_id)
VALUES ('Workshop de IA', '2023-11-15', 'Auditório Principal', '100','1');

INSERT INTO eventos (titulo, date_evento, local, capacidade, Palestrante_id)
VALUES ('Conferência de Marketing', '2023-12-10', 'Sala de Convenções', '200','2');


-- Inserir algumas inscrições
INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('1', 'Carlos Oliveira', 'carlos@email.com', '2023-11-15','1');

INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('1', 'Ana Souza', 'ana@email.com', '2023-11-15','1');

INSERT INTO inscricoes (evento_id, nome_participante, email, data_inscricao, presente)
VALUES ('2', 'Bruno Lima', 'bruno@email.com', '2023-12-10','1');

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
SELECT id, titulo, date_evento, local, vagas_disponiveis 
FROM vw_eventos_detalhados
WHERE vagas_disponiveis > 0;

-- 3. Listar participantes inscritos em um evento específico
    SELECT * FROM inscricoes WHERE evento_id = 1;

```
