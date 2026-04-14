# Grafos de Co-ocorrência Gastronômica: Camarões vs Coco Bambu

Este projeto aplica algoritmos de **Teoria dos Grafos** e **Processamento de Linguagem Natural (NLP)** para comparar as estruturas técnicas de cardápios de dois grandes ícones da gastronomia nordestina. Desenvolvido para a disciplina de **Estrutura de Dados 2 (UFRN)**.

## Escopo e Objetivos
O objetivo central é transformar descrições textuais não estruturadas em uma estrutura de dados de Grafo. Através desta modelagem, buscamos analisar a conectividade entre ingredientes, realizando uma comparação transregional entre os restaurantes:
* **Camarões (Ponta Negra, Natal/RN)**: Baseado no arquivo `Cardapio+Ponta+Negra.pdf`.
* **Coco Bambu (Fortaleza/CE)**: Baseado em extração dinâmica via console do *Live Menu*.

---

## Organização do Projeto

A estrutura foi desenhada para garantir a separação entre dados brutos, processados e saídas visuais:

* `data/processados/`: Arquivos JSON estruturados após a etapa de NER (Named Entity Recognition).
* `data/imagens/`: Exportações estáticas (PNG) de alta resolução dos grafos.
* `data/matriz_adj/`: Matrizes de adjacência (CSV) geradas por categoria.
* `data/visualizacoes_interativas/`: Mapas dinâmicos em HTML (Grafos Bipartidos e Projeções).
* `src/`: Notebooks Jupyter contendo o pipeline de execução e modelagem.

---

## Metodologia e Conceitos de Estrutura de Dados

### 1. Modelagem de Dados (Grafo Bipartido)
A estrutura fundamental é um **Grafo Bipartido** $G = (U, V, E)$, onde os vértices são divididos em dois conjuntos disjuntos:
* **Conjunto $P$ (Pratos)**: Vértices que representam a entidade final do cardápio.
* **Conjunto $I$ (Ingredientes)**: Vértices que representam os componentes atômicos.
* **Arestas ($E$)**: Uma conexão $e = \{p, i\}$ existe se, e somente se, o ingrediente $i$ compõe o prato $p$.

### 2. Projeção de Similaridade (One-Mode Projection)
Para sintetizar os resultados, aplicamos uma **Projeção Monopartida**. Nesta camada, os ingredientes são omitidos e os pratos são conectados diretamente entre si.
* **Filtro de Significância**: Apenas pratos com **Índice de Jaccard $\ge 0.50$** são mantidos na visualização de projeção.

### 3. Métrica de Similaridade (Índice de Jaccard)
Utilizamos o Coeficiente de Jaccard para medir a similaridade entre as vizinhanças dos nós de pratos:

$$J(A,B) = \frac{|N(A) \cap N(B)|}{|N(A) \cup N(B)|}$$

---

## Resultados e Visualizações Unificadas

Abaixo apresentamos o acesso consolidado a todos os artefatos gerados pelo pipeline. As **Visualizações Interativas (HTML)** permitem explorar a topologia da rede através de forças de mola, enquanto os **Snapshots (PNG)** fornecem registros estáticos de alta resolução para o relatório técnico.

| Categoria | Estrutura (Bipartido) | Similaridade (Projeção) | Matriz de Adjacência |
| :--- | :---: | :---: | :---: |
| **Entradas** | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_entradas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/similaridade_entradas.png) | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/projecao_entradas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/projecao_entradas.png) | [CSV](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/matriz_adj/matriz_entradas.csv) |
| **Pratos Principais** | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_pratos_principais.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/similaridade_pratos_principais.png) | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/projecao_pratos_principais.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/projecao_pratos_principais.png) | [CSV](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/matriz_adj/matriz_principais.csv) |
| **Saladas** | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_saladas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/similaridade_saladas.png) | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/projecao_saladas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/projecao_saladas.png) | [CSV](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/matriz_adj/matriz_saladas.csv) |
| **Sobremesas** | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_sobremesas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/similaridade_sobremesas.png) | [HTML](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/projecao_sobremesas.html) / [PNG](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/imagens/projecao_sobremesas.png) | [CSV](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/data/matriz_adj/matriz_sobremesas.csv) |

> **Dica de Explanação:** As arestas verdes nas projeções representam similaridade $J \ge 0.50$, evidenciando os "matches" diretos entre os cardápios.

---

## Guia de Execução e Apresentação

1.  **Execução**: Instalar dependências e rodar `src/proc_grafos_gen.ipynb`.
2.  **Slides (LaTeX)**: Detalham a fundamentação teórica.
    * [Visualizar Apresentação (PDF)](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/docs/apresentacao.pdf) | [Código-fonte (.tex)](https://github.com/arleswasb/Projeto_ED2_P1/blob/main/docs/apresentacao.tex)

---

## Identificação
**UFRN - DCA3304 - Estrutura de Dados II**
**Professor**: Dr. Ivanovitch Medeiros Dantas da Silva
**Alunos**: Werber Arles de Souza Barradas e Nilton Fontes Barreto Neto