

# Projeto Unidade 1: Grafos de Co-ocorrência Gastronômica (NLP + ED2)

Este repositório contém o desenvolvimento do primeiro trabalho prático da disciplina de **Estrutura de Dados 2 (UFRN)**. O projeto aplica algoritmos de grafos e técnicas de **Reconhecimento de Entidades Nomeadas (NER)** para mapear a inteligência de dados por trás de cardápios de alta gastronomia.

## Escopo e Objetivos
O objetivo central é transformar descrições textuais não estruturadas em uma estrutura de dados de Grafo. Através desta modelagem, buscamos analisar a conectividade entre ingredientes, técnicas de preparo e perfis de consumo, realizando uma comparação transregional entre os restaurantes:
* **Camarões (Ponta Negra, Natal/RN)**: Baseado no arquivo `Cardapio+Ponta+Negra.pdf`.
* **Coco Bambu (Fortaleza/CE)**: Baseado em extração dinâmica via console do *Live Menu*.

---

## Organização do Repositório
Para viabilizar a análise comparativa, os dados estão organizados nas seguintes pastas:
* `data/camaroes_ponta_negra/`: Contém o PDF original e o arquivo `menu_cm.txt`.
* `data/coco_bambu/`: Contém o arquivo `menu_cb.txt` extraído da web.
* `data/processados/`: Armazena os resultados estruturados em JSON (`ner_camaroes.json` e `ner_cocobambu.json`) e as exportações de imagens dos grafos.
* `notebooks/`: Contém o arquivo `proc_grafos.ipynb` com todo o pipeline de execução.

---

## Detalhamento das Tags (NER)
A extração de informações categoriza os termos dos cardápios para dar semântica aos nós do grafo:

| TAG | Descrição | Exemplos no Projeto |
| :--- | :--- | :--- |
| **`PRATO`** | Identificador único do prato para análise de similaridade. | Camarão Internacional, Filé à Parmegiana. |
| **`ING`** | Ingredientes base da receita (Nós centrais). | Camarão, Carne de Sol, Peixe, Bacalhau. |
| **`TEC`** | Técnicas de preparo que conectam pratos. | Grelhado, Gratinado, Flambado, Salteado. |
| **`REG`** | Elementos da culinária regional (Regionalismos). | Queijo Coalho, Nata, Manteiga da Terra. |
| **`ACOMP`** | Guarnições e acompanhamentos. | Arroz de Leite, Batata Palha, Baião de Dois. |
| **`SIZE`** | Atributo de nó para perfil de consumo. | Individual, Para Duas Pessoas. |

---

## Metodologia e Plano de Ação

### 1. Preparação e Coleta
Conversão de dados não estruturados em formatos processáveis:
* **Extração de Texto (Camarões)**: Utilização da biblioteca `PyMuPDF` para o parsing do arquivo `Cardapio+Ponta+Negra.pdf`.
* **Extração Dinâmica (Coco Bambu)**: Captura via console do navegador para lidar com a renderização dinâmica do site.
* **Limpeza (Regex)**: Aplicação de expressões regulares para remover preços, códigos internos e gramagens.

### 2. Reconhecimento de Entidades (NER)
Transformação das descrições literárias em categorias formais utilizando o **spaCy**:
* **Motor de NER Único**: Aplicação de um `EntityRuler` unificado para ambos os restaurantes, garantindo que ingredientes idênticos gerem o mesmo identificador de nó.
* **Persistência**: Exportação dos dados estruturados para arquivos **JSON**, garantindo a modularidade entre a fase de NLP e a fase de Grafos.

### 3. Análise de Grafos e Similaridade Gastronômica
Modelagem dos dados em uma rede complexa para quantificar a semelhança entre as cidades.



* **Modelagem Bipartida**: O grafo é composto por nós de **Prato** e nós de **Atributo** (Ingredientes/Técnicas).
* **Índice de Similaridade de Jaccard**: Cálculo matemático para validar a semelhança entre pratos homônimos ou similares, baseado na interseção e união de suas vizinhanças de nós.
* **Visualização e Subgrafos**: Utilização do layout de mola (`spring_layout`) e extração de subgrafos induzidos (ex: categoria "Camarão") para mitigar a densidade visual e destacar as "pontes" regionais.
* **Representação Algébrica**: Conversão da rede em uma **Matriz de Adjacência** esparsa para processamento automatizado.



---

## Guia de Execução: Passo a Passo

1.  **Ambiente**: Ativar o ambiente virtual e instalar as dependências (`pip install networkx spacy pymupdf matplotlib scipy`).
2.  **Extração e Limpeza**: Executar as células de processamento inicial para gerar os arquivos `.txt` limpos.
3.  **Processamento NER**: Rodar o motor do spaCy para gerar os arquivos `.json` estruturados.
4.  **Análise de Grafos**: Executar as células de construção do `NetworkX` para gerar as visualizações e cálculos de similaridade.
5. As instruções detalhadas constam no pdf anexo.

---

## Identificação

**Universidade**: Universidade Federal do Rio Grande do Norte (UFRN)
**Disciplina**: DCA3304 - Estrutura de Dados II
**Professor**: Prof. Dr. Ivanovitch Medeiros Dantas da Silva

**Alunos**:
* Werber Arles de Souza Barradas (@Arleswasb)
* Nilton Fontes Barreto Neto
