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
* `data/visualizacoes_interativas/`: Mapas dinâmicos em HTML.
* `src/`: Notebooks Jupyter contendo o pipeline de execução e modelagem.

---

## Metodologia e Conceitos de Estrutura de Dados

### 1. Modelagem de Dados (Grafo Bipartido)
A estrutura fundamental é um **Grafo Bipartido** $G = (U, V, E)$, onde os vértices são divididos em dois conjuntos disjuntos:
* **Conjunto $P$ (Pratos)**: Vértices que representam a entidade final do cardápio.
* **Conjunto $I$ (Ingredientes)**: Vértices que representam os componentes atômicos.
* **Arestas ($E$)**: Uma conexão $e = \{p, i\}$ existe se, e somente se, o ingrediente $i$ compõe o prato $p$. Não existem conexões diretas entre dois pratos ou entre dois ingredientes nesta camada.

### 2. Estratégia de Subgrafos e Redução de Ruído
Para a disciplina de ED2, a eficiência visual e a densidade do grafo são críticas. Utilizamos a técnica de **Subgrafos Induzidos** para:
* **Isolamento por Contexto**: Divisão em Macro-Categorias (Entradas, Principais, etc.) para reduzir o *clutter* (poluição visual) e permitir análises de nicho.
* **Filtragem de Nós Isolados**: Remoção de ingredientes de grau 1 (que aparecem em apenas um prato), focando nos **Hubs de Conectividade** que realmente definem a similaridade entre os restaurantes.

### 3. Métrica de Similaridade (Índice de Jaccard)
Utilizamos o Coeficiente de Jaccard para medir a similaridade entre as vizinhanças dos nós de pratos. Para dois pratos $A$ e $B$, a similaridade é a razão entre o número de ingredientes comuns e o total de ingredientes únicos combinados:
$$J(A,B) = \frac{|N(A) \cap N(B)|}{|N(A) \cup N(B)|}$$
*Onde $N(x)$ representa o conjunto de vizinhos do nó $x$.*

---

## Plano de Ação e Desenvolvimento

### 1. Preparação e Coleta (Ingestão de Dados)
* **a) Parsing de PDF (Camarões)**: Extração de texto bruto tratada para lidar com quebras de linha e segmentação.
* **b) Captura Dinâmica (Coco Bambu)**: Extração via console JS para capturar o DOM renderizado, contornando proteções de sites dinâmicos.
* **c) Normalização via LLM**: Uso de Gemini para realizar o NER, garantindo que "Camarão Médio" e "Camarão Grelhado" fossem normalizados para o nó central "Camarão".

### 2. Detalhamento das Macro-Categorias
A análise foi segmentada para extrair semânticas específicas de cada etapa da experiência gastronômica:
* **Entradas**: Avalia a similaridade em petiscos e "finger foods".
* **Pratos Principais**: Onde reside a maior densidade de dados e as proteínas base (Camarão, Peixe, Carne).
* **Saladas**: Foco em frescor e ingredientes vegetais.
* **Sobremesas**: Identifica a convergência em bases de confeitaria (Ninho, Nutella, Frutas).

### 3. Detalhamento das Tags (NER)
| TAG | Função no Grafo | Descrição Técnica |
| :--- | :--- | :--- |
| **`PRATO`** | Nó de Origem | Vértice âncora para a análise de similaridade. |
| **`ING`** | Nó Central (Hub) | Vértices "ponte" que conectam os dois restaurantes através da vizinhança comum. |

---

## Visualização Interativa (GitHub Pages)

Os grafos foram exportados via **Pyvis**, permitindo interação física (força de molas) para explorar a topologia da rede:

* [**📍 Mapa de Entradas**](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_entradas.html)
* [**📍 Mapa de Saladas**](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_saladas.html)
* [**📍 Mapa de Pratos Principais**](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_pratos_principais.html)
* [**📍 Mapa de Sobremesas**](https://arleswasb.github.io/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_sobremesas.html)



---

## Guia de Execução

1.  **Ambiente**: Instalar dependências: `pip install networkx matplotlib scipy pyvis pandas`.
2.  **Execução**: Rodar o notebook `src/proc_grafos_gen.ipynb`. O script gerará as matrizes de adjacência CSV, que são a representação tabular do grafo, essenciais para auditoria dos dados.
3.  **Análise**: Os resultados no console do notebook exibirão os Top 5 pratos com maior similaridade técnica entre Natal e Fortaleza.

---

## 📢 Apresentação do Projeto

Os slides utilizados para a defesa deste projeto foram desenvolvidos em **LaTeX (Beamer)** e detalham a fundamentação teórica de grafos bipartidos e a métrica de Jaccard aplicada.

* [Visualizar Apresentação (PDF)](Projeto_ED2_P1/docs/apresentação.pdf)
* [Código-fonte da Apresentação (.tex)](Projeto_ED2_P1/docs/apresentação.tex)

---

## Identificação

**UFRN - DCA3304 - Estrutura de Dados II**
**Professor**: Dr. Ivanovitch Medeiros Dantas da Silva  

**Alunos**:
* Werber Arles de Souza Barradas (@Arleswasb)
* Nilton Fontes Barreto Neto