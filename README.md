

# Projeto Unidade 1: Grafos de Co-ocorrência Gastronômica (NLP + ED2)

Este repositório contém o desenvolvimento do primeiro trabalho prático da disciplina de **Estrutura de Dados 2 (UFRN)**. O projeto aplica algoritmos de grafos e técnicas de **Reconhecimento de Entidades Nomeadas (NER)** para mapear a inteligência de dados por trás de cardápios de alta gastronomia.

## Escopo e Objetivos
O objetivo central é transformar descrições textuais não estruturadas em uma estrutura de dados de Grafo. Através desta modelagem, buscamos analisar a conectividade entre ingredientes, técnicas de preparo e perfis de consumo, realizando uma comparação transregional entre os restaurantes:
* **Camarões (Ponta Negra, Natal/RN)**: Baseado no arquivo `Cardapio+Ponta+Negra.pdf`.
* **Coco Bambu (Fortaleza/CE)**: Baseado em extração dinâmica via console do *Live Menu*.

---

## Organização do Repositório
Para viabilizar a análise comparativa, os dados estão organizados nas seguintes pastas:
* `data/camaroes_ponta_negra/`: Contém o PDF original, o arquivo sujo `menu_cm.txt` e o arquivo limpo camaroes_limpo.txt.
* `data/coco_bambu/`: Contém o arquivo `menu_cb.txt` extraído da web e o arquivo limpo `cocobambu_limpo.txt`.
* `data/processados/`: Armazena os resultados estruturados em JSON (`ner_camaroes.json` e `ner_cocobambu.json`) e a matriz de adjacencia.csv
* `data/imagens/`: Tem as exportações de imagens estáticas (PNG) dos grafos gerados.
* `data/visualizacoes_interativas/`: Contém os mapas interativos em formato HTML (Pyvis).
* `src/`: Contém o notebook `proc_grafos_gen.ipynb` com todo o pipeline de execução.

---

## Detalhamento das Tags (NER)
A extração de informações categoriza os termos dos cardápios para dar semântica aos nós do grafo:

| TAG | Descrição | Exemplos no Projeto |
| :--- | :--- | :--- |
| **`PRATO`** | Identificador único do prato para análise de similaridade. | Camarão Internacional, Filé à Parmegiana. |
| **`ING`** | Ingredientes base da receita (Nós centrais). | Camarão, Carne de Sol, Peixe, Bacalhau. |
| **`ACOMP`** | Guarnições e acompanhamentos. | Arroz de Leite, Batata Palha, Baião de Dois. |

---

## Metodologia e Plano de Ação

### 1. Preparação e Coleta
Conversão de dados não estruturados em formatos processáveis:
* **Parsing de PDF (Camarões):**: Extração de texto bruto do arquivo Cardapio+Ponta+Negra.pdf.
* **Captura Dinâmica (Coco Bambu):**: Extração de dados via console do navegador para contornar a renderização dinâmica do Live Menu.
* **Evolução do Pipeline:)**: Aplicação de um pipeline de LLM-based Extraction, permitindo lidar com a variabilidade de descrições literárias dos cardápios.

### 2. Reconhecimento de Entidades (NER)
Transformação das descrições literárias em categorias formais utilizando 
* **Extração via IA Generativa:**: Utilização de modelos de linguagem (Gemini) para realizar o Reconhecimento de Entidades Nomeadas (NER) de forma contextual. Isso permitiu separar com precisão o que é Nome do Prato, Descrição Narrativa e a lista atômica de Ingredientes.
* **Normalização de Dados:**: Garantia de que ingredientes idênticos em ambos os restaurantes (ex: "Nata" vs "Nata Fresca") fossem mapeados para o mesmo identificador de nó, viabilizando o cálculo de similaridade.
* **Persistência em JSON:**: Os dados foram exportados para arquivos JSON estruturados, servindo como input para a biblioteca NetworkX.

### 3. Análise de Grafos e Similaridade Gastronômica
* **Modelagem de Grafo Bipartido:** Construção de uma rede onde Pratos e Ingredientes são nós distintos, permitindo mapear a composição de cada receita.
* **Mapeamento de Similaridade (Jaccard):** Aplicação do Índice de Jaccard para comparar pratos homônimos com base na vizinhança de seus ingredientes (Common Neighbors).
* **Destaques Visuais:** O pipeline destaca automaticamente os "Ingredientes Comuns" (shared hubs) nas visualizações, utilizando arestas coloridas para evidenciar a similaridade transregional entre Camarões e Coco Bambu.



---

## Visualização Interativa (Pyvis)
Além das imagens estáticas, disponibilizamos versões interativas dos grafos para exploração dinâmica (zoom, arraste e detalhes ao passar o mouse).

> [!TIP]
> Use os links abaixo para visualizar diretamente via GitHub (HTML Preview):
> * [🗺️ Mapa Interativo: Entradas](https://htmlpreview.github.io/?https://github.com/Arleswasb/ED2_P1/blob/main/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_entradas.html)
> * [🗺️ Mapa Interativo: Pratos Principais](https://htmlpreview.github.io/?https://github.com/Arleswasb/ED2_P1/blob/main/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_pratos_principais.html)
> * [🗺️ Mapa Interativo: Saladas](https://htmlpreview.github.io/?https://github.com/Arleswasb/ED2_P1/blob/main/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_saladas.html)
> * [🗺️ Mapa Interativo: Sobremesas](https://htmlpreview.github.io/?https://github.com/Arleswasb/ED2_P1/blob/main/Projeto_ED2_P1/data/visualizacoes_interativas/similaridade_sobremesas.html)

---

## Guia de Execução: Passo a Passo

1.  **Ambiente**: Ativar o ambiente virtual e instalar as dependências (`pip install networkx spacy pymupdf matplotlib scipy pyvis`).
2.  **Processamento NER**: Executar o pipeline de extração via LLM/NER para converter os cardápios em JSON estruturado (armazenados em `data/processados/`).
3.  **Análise e Exportação**: Rodar o notebook `src/proc_grafos_gen.ipynb`. O pipeline irá realizar automaticamente:
    *   O cálculo de similaridade entre os menus.
    *   A exportação de imagens estáticas (PNG) de alta resolução para `data/imagens/`.
    *   A geração de mapas interativos (HTML) para `data/visualizacoes_interativas/`.
5. As instruções detalhadas constam no pdf anexo.

---

## Identificação

**Universidade**: Universidade Federal do Rio Grande do Norte (UFRN)
**Disciplina**: DCA3304 - Estrutura de Dados II
**Professor**: Prof. Dr. Ivanovitch Medeiros Dantas da Silva

**Alunos**:
* Werber Arles de Souza Barradas (@Arleswasb)
* Nilton Fontes Barreto Neto
