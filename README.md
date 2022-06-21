-- Criando usuário do banco de dados Congresso

CREATE USER grupo6 WITH
  NOSUPERUSER
  CREATEDB
  CREATEROLE
  LOGIN
  ENCRYPTED PASSWORD '123456'
;

-- Criando banco de dados congresso

CREATE DATABASE congresso WITH
  owner      = grupo6
  template   = template0
  encoding   = 'UTF-8'
  lc_collate = 'pt_BR.UTF-8'
  lc_ctype   = 'pt_BR.UTF-8'
;

CREATE SCHEMA congresso AUTHORIZATION grupo6;
COMMENT ON SCHEMA congresso IS 'Schema para Sistema congresso.';

-- Configura o SEARCH_PATH do usuário grupo6.
ALTER USER grupo6 SET SEARCH_PATH TO congresso, "$user", public;

-- Ajusta o SEARCH_PATH da conexão atual ao banco de dados.
SET SEARCH_PATH TO congresso, "$user", public;


CREATE TABLE palestrantes (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(5) NOT NULL,
                email VARCHAR NOT NULL,
                descricao_do_curriculum VARCHAR NOT NULL,
                CONSTRAINT palestrantes PRIMARY KEY (codigo)
);


CREATE TABLE programas (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                CONSTRAINT programas PRIMARY KEY (codigo)
);


CREATE TABLE palestras (
                codigo VARCHAR(5) NOT NULL,
                classificacao VARCHAR(5) NOT NULL,
                Nome_da_Palestra VARCHAR(50) NOT NULL,
                CONSTRAINT palestras PRIMARY KEY (codigo)
);


CREATE TABLE materiais_necessarios (
                codigo VARCHAR(5) NOT NULL,
                materiais VARCHAR(50) NOT NULL
);


CREATE TABLE Organizadores (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                atribuicao VARCHAR(50) NOT NULL,
                CONSTRAINT organizadores PRIMARY KEY (codigo)
);


CREATE TABLE patrocinadores (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                ramo_de_atividade VARCHAR(10) NOT NULL,
                pessoa_de_Contato VARCHAR(50) NOT NULL,
                CONSTRAINT patrocinadores PRIMARY KEY (codigo)
);


CREATE TABLE endereco (
                codigo VARCHAR(5) NOT NULL,
                logradouro VARCHAR(10) NOT NULL,
                numero CHAR(4) NOT NULL,
                complemento VARCHAR(50),
                bairro VARCHAR(20) NOT NULL,
                cidade VARCHAR(10) NOT NULL,
                uf CHAR(2) NOT NULL,
                cep CHAR NOT NULL
);


CREATE TABLE categorias (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                Valores NUMERIC(5,2) NOT NULL,
                CONSTRAINT categorias PRIMARY KEY (codigo)
);


CREATE TABLE locais (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                tamanho VARCHAR(10) NOT NULL,
                pessoa_de_Contato VARCHAR(50) NOT NULL,
                CONSTRAINT locais PRIMARY KEY (codigo)
);


CREATE TABLE telefones_de_contato (
                telefone CHAR(14) NOT NULL,
                celular CHAR(14) NOT NULL,
                ddi CHAR(2) NOT NULL,
                ddd CHAR(2) NOT NULL,
                numero CHAR(9) NOT NULL,
                codigo VARCHAR(5) NOT NULL
);


CREATE TABLE visitantes (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                Email VARCHAR NOT NULL,
                CONSTRAINT visitantes PRIMARY KEY (codigo)
);


CREATE TABLE congressos (
                codigo VARCHAR(5) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                descricao VARCHAR NOT NULL,
                data_de_inicio DATE NOT NULL,
                data_de_fim DATE NOT NULL,
                horario_de_inicio TIME NOT NULL,
                horario_de_fim TIME NOT NULL,
                publico_alvo VARCHAR(50) NOT NULL,
                CONSTRAINT congressos PRIMARY KEY (codigo)
);


CREATE TABLE usam (
                codigo VARCHAR(5) NOT NULL,
                CONSTRAINT usam_pk PRIMARY KEY (codigo)
);


CREATE TABLE patrocinio (
                codigo VARCHAR(5) NOT NULL,
                valor NUMERIC(5,2) NOT NULL,
                forma_de_pagamento CHAR(2) NOT NULL,
                CONSTRAINT patrocinio PRIMARY KEY (codigo)
);


CREATE TABLE organizam (
                codigo VARCHAR(5) NOT NULL,
                CONSTRAINT organizam_pk PRIMARY KEY (codigo)
);


CREATE TABLE inscrevem_se (
                codigo VARCHAR(5) NOT NULL,
                CONSTRAINT inscrevem_se_pk PRIMARY KEY (codigo)
);


CREATE TABLE valoram (
                codigo VARCHAR(5) NOT NULL,
                CONSTRAINT valoram_pk PRIMARY KEY (codigo)
);


ALTER TABLE endereco ADD CONSTRAINT palestrantes_endereco_fk
FOREIGN KEY (codigo)
REFERENCES palestrantes (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE palestras ADD CONSTRAINT palestrantes_palestras_fk
FOREIGN KEY (codigo)
REFERENCES palestrantes (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE usam ADD CONSTRAINT programas_usam_fk
FOREIGN KEY (codigo)
REFERENCES programas (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE palestras ADD CONSTRAINT programas_palestras_fk
FOREIGN KEY (codigo)
REFERENCES programas (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE materiais_necessarios ADD CONSTRAINT palestras_materiais_necessarios_fk
FOREIGN KEY (codigo)
REFERENCES palestras (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE organizam ADD CONSTRAINT organizadores_organizam_fk
FOREIGN KEY (codigo)
REFERENCES Organizadores (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE patrocinio ADD CONSTRAINT patrocinadores_patroc_nio_fk
FOREIGN KEY (codigo)
REFERENCES patrocinadores (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE endereco ADD CONSTRAINT patrocinadores_endereco_fk
FOREIGN KEY (codigo)
REFERENCES patrocinadores (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

/*
Warning: Relationship has no columns to map:
*/
ALTER TABLE locais ADD CONSTRAINT endereco_locais_fk
FOREIGN KEY ()
REFERENCES endereco ()
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE valoram ADD CONSTRAINT categorias_valoram_fk
FOREIGN KEY (codigo)
REFERENCES categorias (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE congressos ADD CONSTRAINT locais_congressos_fk
FOREIGN KEY (codigo)
REFERENCES locais (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE telefones_de_contato ADD CONSTRAINT locais_telefones_de_conato_fk
FOREIGN KEY (codigo)
REFERENCES locais (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE inscrevem_se ADD CONSTRAINT visitantes_inscrevem_se_fk
FOREIGN KEY (codigo)
REFERENCES visitantes (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE valoram ADD CONSTRAINT congressos_valoram_fk
FOREIGN KEY (codigo)
REFERENCES congressos (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE inscrevem_se ADD CONSTRAINT congressos_inscrevem_se_fk
FOREIGN KEY (codigo)
REFERENCES congressos (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE organizam ADD CONSTRAINT congressos_organizam_fk
FOREIGN KEY (codigo)
REFERENCES congressos (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE patrocinio ADD CONSTRAINT congressos_patroc_nio_fk
FOREIGN KEY (codigo)
REFERENCES congressos (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE usam ADD CONSTRAINT congressos_usam_fk
FOREIGN KEY (codigo)
REFERENCES congressos (codigo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;
