# python-emotion-mining

Curso sobre mineração de emoção em textos utilizando a linguagem Python.

## Seções

1. Mineração de textos e classificação
2. Pré-processamento dos textos (stop words, stemming, etc)
3. Detectando emoções em textos com Naive Bayes (NLTK - Natural Language Toolkit)
4. Avaliação do algoritmo
5. Considerações finais

---

### 1. Mineração de textos e classificação

#### Formas de armazenamento de textos

- Livros, jornais, revistas, páginas web, blogs, redes sociais, e-mails, PDF, XML e JSON, etc.
- Geralmente não possuem um "esquema"  para descrever sua estrutura
- Texto "livre" vs texto "formatado"

#### Classificadores

- mensagem (assunto, texto) &rarr; algoritmo classificador &rarr; classe da mensagem (normal, spam, etc)

- texto de notícia &rarr; algorítimo classificador multirrótulo &rarr; tópicos (esporte, rio de janeiro, economia)

#### Agrupamentos _(Clustering)_

- IBGE na descoberta de bairros com nomes similares (`Jardim América` = `Jdim América`)

- Detecção de plágio

#### Extração de informação

Texto "livre" &rarr; `{ livro: "Utopia", ano: 1516, pais: "Brasil", autor: "Thomas More"}`

Utilização de ontologias, representando conceitos e relacionamentos em um determinado domínio
`Livro { nome: "Utopia" }` é escrito por `Autor { nome: "Thomas More"}` em `País {nome: "Brasil"}` quando `Data { ano: 1516 }`

NLTK &rarr; ferramentas prontas para identificação de pessoas/empresas em textos

#### Associações

- Correlação entre palavras  
  - "60% dos textos que contêm a palavra 'Internacional' também contêm a palavra 'Grêmio'. 3% de todos os textos contêm ambas as palavras". _Representação: {"Internacional"} <-> {"Grêmio}_
  - "A presença do termo 'Pelé' aumenta 5 vezes a chance de ocorrência dos termos 'Copa' e '1970'". _Representação: {"Pelé"} <-> {"Copa", "1970"}_

#### Casamento de esquemas

- Correspondências semânticas
  - Importador para migrar dados de um software antigo para um sistema novo, análise das bases de dados antiga e nova, campo a campo.

- Algoritmo como matcher (PLN - Processamento de Linguagem Natural)
  - É necessário fazer uma análise de ontologias;
  - Consulta do usuário: "encontre os livros do Robert C. Martin", aplica algoritmo de matcher e obtém a seguinte consulta:

  ```sql
  SELECT titulo, ano, resumo FROM publicacoes WHERE autor like '%Robert C. Martin%' and tipo = 'livro'
  ```

#### Recuperação da informação

- Localizar e ranquear documentos relevantes em uma coleção;
- Indexação (API Lucene - Java)

| Palavra    | Ponteiro 1  | Ponteiro 2   | Ponteiro 3   | Ponteiro 4   |
|------------|-------------|--------------|--------------|--------------|
| "goleador" | Documento 3 | Documento 12 |              |              |
| "goleiro"  | Documento 7 | Documento 1  | Documento 27 | Documento 19 |
| "gol"      | Documento 1 | Documento 2  |              |              |

Documento 1: "... um belo **gol** no segundo tempo..."  
Documento 2: "Não aconteceu nenhum **gol** por conta de ..."

#### Sumarização de documentos

    Texto com bastante linhas <- aplicar algoritmo de sumarização -> Resumo do texto com os principais pontos em poucas linhas

#### Abordagens da mineração de textos

- Estatística
  - Frequência dos termos, ignorando informações semânticas;

- Processamento de linguagem natural (PLN)
  - Interpretação sintática e semântica das frases;
  - Fazer o computador entender textos escritos em linguagem humana.

#### Mineração de emoção

- Classificação: aplicar algoritmo em texto e extrair as emoções: "Alegria", "Tristeza", "Medo", etc;
- Monitoramento de marcas, entidades, figuras sociais;
- Gestão de crises;
- Entender o que as pessoas pensam;
- Classificação por polaridade (valência): submeter um texto em um algoritmo: "Positivo", "Neutro" e "Negativo"
- Classificação por emoção (6 bases): Supresa, Alegria, Tristeza, Medo, Disgosto e Raiva _(segundo estudo do Paul Ekman)_

#### Classificação

- Cada registro pertence a uma classe que possui um conjunto de atributos previsores;
- Objetiva-se descobrir um relacionamento entre os atributos previsores e o atributo meta;
- O valor do atributo meta é conhecido através de aprendizagem superviosionada;

##### Tabela de treinamento (Classificação de risco de crédito)

Atributos previsores: histórica do crédito, divida, garantias, renda anual;  
Atributo meta (classe): risco.

| História do crédito | Dívida | Garantias | Renda anual           | Risco    |
|---------------------|--------|-----------|-----------------------|----------|
| Ruim                | Alta   | Nenhuma   | < 15.000              | Alto     |
| Desconhecida        | Alta   | Nenhuma   | >= 15.000 a <= 35.000 | Alto     |
| Desconhecida        | Baixa  | Nenhuma   | >= 15.000 a <= 35.000 | Moderado |
| Desconhecida        | Baixa  | Nenhuma   | < 15.000              | Alto     |
| Desconhecida        | Baixa  | Nenhuma   | > 35.000              | Baixo    |
| Desconhecida        | Baixa  | Adequada  | > 35.000              | Baixo    |
| Ruim                | Baixa  | Nenhuma   | < 15.000              | Alto     |
| Ruim                | Baixa  | Adequada  | > 35.000              | Moderado |
| Boa                 | Baixa  | Nenhuma   | > 35.000              | Baixo    |
| Boa                 | Alta   | Adequada  | > 35.000              | Baixo    |
| Boa                 | Alta   | Nenhuma   | < 15.000              | Alto     |
| Boa                 | Alta   | Nenhuma   | >= 15.000 a <= 35.000 | Moderado |
| Boa                 | Alta   | Nenhuma   | > 35.0000             | Baixo    |
| Ruim                | Alta   | Nenhuma   | >= 15.000 a <= 35.000 | Alto     |

##### Tabela de testes (Classificação de risco de crédito)

| História do crédito | Dívida | Garantias | Renda anual           |
|---------------------|--------|-----------|-----------------------|
| Ruim                | Alta   | Adequada  | < 15.000              |
| Desconhecida        | Alta   | Adequada  | < 15.000              |
| Desconhecida        | Baixa  | Nenhuma   | > 35.000              |
| Boa                 | Alta   | Adequada  | >= 15.000 a <= 35.000 |

##### Tabela de treinamento (Classificação de vendas de livros)

Atributos previsores: sexo, país, idade;  
Atributo meta (classe): comprar.

| Sexo | País       | Idade | Comprar |
|------|------------|-------|---------|
| M    | França     | 25    | Sim     |
| M    | Inglaterra | 21    | Sim     |
| F    | França     | 23    | Sim     |
| F    | Inglaterra | 34    | Sim     |
| F    | França     | 30    | Não     |
| M    | Alemanha   | 21    | Não     |
| M    | Alemanha   | 20    | Não     |
| F    | Alemanha   | 18    | Não     |
| F    | França     | 34    | Não     |
| F    | França     | 34    | Não     |
| M    | França     | 55    | Não     |
| M    | Inglaterra | 25    | Sim     |
| M    | Alemanha   | 48    | Sim     |
| F    | Inglaterra | 23    | Não     |

##### Tabela de testes (Classificação de vendas de livros)

| Sexo | País       | Idade |
|------|------------|-------|
| M    | França     | 38    |
| F    | Inglaterra | 25    |
| M    | Alemanha   | 55    |
| F    | França     | 20    |



---

### 2. Pré-processamento dos textos

_Sem conteúdo ainda_.

---

### 3. Detectando emoções em textos com Naive Bayes

_Sem conteúdo ainda_.

---

### 4. Avaliação do algoritmo

_Sem conteúdo ainda_.

---

### 5. Considerações finais

_Sem conteúdo ainda_.

---

## Referências

- [Mineração de Emoção em Textos com Python e NLKT @ Udemy](https://www.udemy.com/mineracao-de-emocao-em-textos-com-python-e-nltk)

- [Markdown Table Generator](https://www.tablesgenerator.com/markdown_tables)